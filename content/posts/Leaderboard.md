---
title: "Leaderboard"
date: 2022-11-03T23:39:23+01:00
draft: false
tags: ['Web']
categories:
  - 'Writeups'
  - 'Cybernight 2022'
---

# Cybernight - Leaderboard
---

## Category

> Web

## Description

> vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv
>
> Soumettez un score Roombaverse Simulator avec votre propre compte
>
> Note : vous devez télécharger et jouer à Roombaverse Simulator pour ce challenge.

## Files

Roombaverse.exe

## Difficulty

**HARD**

- Author: **MrSheepSheep**
---

## Write up

### Requêtes DNS et HTTPS
Alors en jouant au jeu du roomba simulator, on peut collecter 6 billes jaunes, et ce message apparait :
{{< image src="/Leaderboard/img.png" caption="Win message" title="Win message" alt="Win message" >}}

(PS : récupérer les 6 billes est déjà un challenge en soi avec son propre flag)

C'est exactement la première étape demandée dans la description, et ensuite ?
ça ne soumet aucun score sur notre compte puisque nous ne spécifions à aucun moment notre compte à l'application.

On se doute donc que la soumission passe forcément par des requêtes web.
Si on flush notre DNS (`ipconfig /flushdns` sur windows), ouvre wireshark, lance le simulator et réussi un score ; on remarque qu'une requête DNS est envoyée :
{{< image src="/Leaderboard/img_1.png" caption="DNS request" title="DNS request" alt="DNS request" >}}

{{< admonition info >}}
Conclusion : la requête http est donc dirigée vers https://roombaverse.cybernight-c.tf/
{{</ admonition >}}

On ne voit pas la suite, parce que Wireguard chiffre le tout :
{{< image src="/Leaderboard/img_2.png" caption="Wireguard packets" title="Wireguard packets" alt="Wireguard packets" >}}

### Man In The Middle

On peut donc tenter une stratégie qui évite de passer par le VPN du CTF : **intercepter la requête en se faisant passer pour le serveur**. 
Pour cette recette, il nous faut :
- Un nom de domaine local correspondant
- Un serveur HTTPS fonctionnel

Commençons par éditer notre fichier `/etc/hosts` (Linux) / `C:\Windows\System32\drivers\etc\hosts` (Windows) avec les droits administrateurs, et ajouter la ligne :
```bash
127.0.0.1   roombaverse.cybernight-c.tf
```

Ensuite, on crée un certificat avec sa clé pour avoir un serveur https fonctionnel :
```bash
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365
```

Désormais, on peut lancer un petit serveur https avec python :
```python
from http.server import HTTPServer, BaseHTTPRequestHandler
import ssl


httpd = HTTPServer(('localhost', 443), BaseHTTPRequestHandler)

httpd.socket = ssl.wrap_socket (httpd.socket, 
        keyfile="cert/key.pem", 
        certfile='cert/cert.pem', server_side=True)

httpd.serve_forever()
```

**Bien spécifier le port 443 !!!**


On lance donc ledit serveur, puis on flush de nouveau le DNS, puis on lance le simulator, et gagnons le jeu 
Regardons ensuite notre console de serveur !
{{< image src="/Leaderboard/img_3.png" caption="Web requests" title="Web requests" alt="Web requests" >}}

***TIENS TIENS TIENS***


On réinitialise notre DNS, on flush blabla, et on copie-colle la requête sur la bonne adresse, en remplaçant par son pseudo :
{{< image src="/Leaderboard/img_5.png" caption="Request with my username" title="Request with my username" alt="Request with my username" >}}

Et si on regarde notre profil :
{{< image src="/Leaderboard/img_6.png" caption="Profile" title="Profile" alt="Profile" >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{Roomba_In_Th3_M1ddle}`
{{< /admonition >}}
