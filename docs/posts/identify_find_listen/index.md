# Identify, Find, Listen


## Category

> Hardware

## Part 1 - Identify

### Description

> Un agent ennemi s’est infiltré à la Cybernight !
> Nous l'avons vu passer sur une caméra avec un sac rempli de matériel puis sortir du campus.
>
> Nous pensons qu’il essaye de nous espionner sur le réseau wifi mais nous ne savons pas comment. Une idée de ce qu'il a pu faire ?
>
> Challenge à effectuer en physique uniquement aux 4 étages du bâtiment A.
> N’hésitez pas à fouiller sur d’autres étages.
> Pas besoin d’aller demander la caméra au gardien.


### Difficulty

**EASY**

- Author: **Maestran**
---

### Write up

Etant à l'étage challenger, juste à côté de la salle admin, je me dis que je devrais pas trop avoir à bouger de ma chaise s'ils veulent avoir un oeil sur leur chall "sur place"

Ca parle de WiFi, donc premier reflexe, regarder les réseaux Wifi :
{{< image src="/Identify_Find_Listen/img.png" caption="Sus WiFi" title="Sus WiFi" alt="Sus WiFi" >}}


Suspect, n'est-ce pas ?
Ce n'est évidemment pas le flag comme ça, et si c'était encodé ? Genre en **base64**

Je capte donc <br/>
{{< image src="/Identify_Find_Listen/img_1.png" caption="Flag" title="Flag" alt="Flag" >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{ca_capte?}`
{{< /admonition >}}


## Part 2 - Find Me

### Description

> Ok, ce point wifi n'est clairement pas légitime. Il faudrait le trouver sur le campus.
>
> A vous de trouver une méthode pour le localiser !
>
> Challenge à effectuer en physique uniquement


### Difficulty

**EASY**

- Author: **Maestran**
---

### Write up

{{< admonition danger "Debout">}}
Va falloir se lever de sa chaise 😫 (en vrai génial)
{{</ admonition >}}

En activant les paramètres développeurs pour android, on peut voir sous chaque réseau Wifi deux choses :
- l'adresse MAC de l'AP
- la puissance en dB (donc négative)

Grâce à l'un, on peut trouver la marque du fabricant (*data missing*) : Raspberry PI

Grâce à la deuxième, potentiellement calculer une distance.

On se dit que flemme, c'est en salle admin, on trouve une Raspberry, "vire la, elle porte à confusion" qu'ils disent, donc c'est pas la bonne

Et là, qu'est-ce qu'on voit au fond de la salle, caché derrière un sac ! (très smart)

{{< image src="/Identify_Find_Listen/img_3.png" caption="Sus Rasp" title="Sus Rasp" alt="Sus Rasp" >}}


{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{WTF_Rogue_AP?!}`
{{< /admonition >}}


## Part 3 - Yo Listen

### Description

> Bon super, on a trouvé son matos. Laissons le tourner pour l'instant. Il serait intéressant de voir ce que ce truc diffuse en boucle sur les postes qui se connectent.
>
> Le code du wifi est "toto1234@".
>
> Challenge à effectuer en physique uniquement


### Difficulty

**EASY**

- Author: **Maestran**

### Files

{{< admonition info "Capture non fournie" >}}
[capture](/Identify_Find_Listen/rasp.pcapng) :
Capture effectuée pendant le chall
{{</ admonition >}}

---

### Write up

Bon on a domc une Rasp, et le mot de passe de sa WiFi

On s'y connecte.

Apparemment elle "diffuse" en boucle, bah go ouvrir Wireshark :
{{< image src="/Identify_Find_Listen/img_4.png" caption="Packets" title="Packets" alt="Packets" >}}


Mouais beaucoup de merde. On va virer les requetes DNS venant de mon ordi, et ne garder que ce que la rasp envoi (avec l'ip source 192.168.3.1):
{{< image src="/Identify_Find_Listen/img_5.png" caption="Sus packets" title="Sus packets" alt="Sus packets" >}}


C'est déjà mieux.
3 Choses intéressantes à noter :

- Quand on à un peu l'habitude, doit pas y avoir autant d'UDP sans raison.
- L'ip de destination de ces paquets UDP est un broadcast, donc envoie à tout le monde, ca "diffuse", donc très certainement ce qu'on cherche
- ya un petit message dans chacun de ces paquets (rien que du troll) : ![/Identify_Find_Listen/img_6.png](/Identify_Find_Listen/img_6.png)


Ne gardons plus que ces paquets alors :
{{< image src="/Identify_Find_Listen/img_7.png" caption="Clean Sus packets" title="Clean Sus packets" alt="Clean Sus packets" >}}


Ca c'est clean.

Bon maintenant ya des paquets de protocoles bizares, mais malformés : **BOOTP** et **XTACACS**

Des trucs qui n'ont rien à faire là quoi

Passons, mais regardons ce qui diffère d'un paquet à l'autre...
Le port source c'est normal, mais par contre le port de destination à l'air bien choisi
Et si c'était de l'**ASCII** ?? <br/>

| Letter | ASCII code |
|--------|------------|
| C      | 67         |
| Y      | 89         |
| B      | 66         |
| N      | 78         |

***TIENS TIENS TIENS***
{{< image src="/Identify_Find_Listen/img_8.png" caption="Sus ASCII" title="Sus ASCII" alt="Sus ASCII" >}}


BOOTP utilise le port 67, d'où le fait qu'il soit considéré comme malformé : *ce n'est PAS une requête BOOTP*


Et bien allons-y pour extraire en plain text (txt) la capture, et essayons d'extraire ça à l'aide de python:

Exemple :
{{< image src="/Identify_Find_Listen/img_9.png" caption="Sus WiFi" title="Sus WiFi" alt="Sus WiFi" >}}


Solving :
```py
with open('ports.txt') as f:
	lines = [l.strip() for l in f.readlines()]
	flag = ""
	for line in lines:
		if 'Destination Port:' in line:
			port = line.split('Destination Port:')[1].split()[0].strip()
			c = chr(int(port))
			flag += c

print(flag)
```

Ce qui donne : `{s_13_t0_l1st3n}CYBN{t's_n1c3t0_s3n}CYN{t's_n1c3_t0_l1s3nCYB{1t's_n1c3_t0_l1st3n}CBN{1t's_c3_t0_lst3n}CYBN{1t'_n1c3_t0_l1st3n}`

Puisque l'UDP à pour habitude de perde ses paquets dans la nature, on l'affiche plusieurs fois, et on essaie de le reconstituer, et bingo


{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{1t's_n1c3_t0_l1s3n}`
{{< /admonition >}}

