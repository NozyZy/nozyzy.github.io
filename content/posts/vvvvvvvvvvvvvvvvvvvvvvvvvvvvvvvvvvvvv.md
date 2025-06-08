---
title: "Vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv"
subtitle: "Cybernight 2022 - Vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv"
date: 2022-11-03T23:39:48+01:00
draft: false
tags: ['Web']
categories:
  - 'Writeups'
  - 'Cybernight 2022'
---

## Category

> Web

## Description

> vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv
> 
> Vous avez essayé de vous déconnecter ?

## Difficulty

**Easy**

- Author: **MrSheepsheep**
---

## Write up

Le cauchemar de beaucoup, je le conçois.
Je vais vous conter comment m'est venue l'idée à partir de ce titre, si bien trouvé (*quoi ?!* vous me direz)

Je discute avec mes mate pendant une pause et je dis
> vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv ca correspond à au aucune chaine de caractères
> on dirait juste le bruit du roomba qui aspire
> vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

"le roomba qui aspire", retenez bien.

SINON, on tente la description (ajoutée plus tard durant le CTF) : se déconnecter <br/>
Là j'ouvre Burspsuite, un proxy, pour voir à quoi ressemblent les requêtes qui passent <br/>
Et je vois que le `/deconnexion` est effectué via une méthode GET. Or la "bonne" façon de faire une requete de déconnexion, c'est via un POST, puisque l'on envoie la demande de déconnexion.

Je tente donc de modifier la requete avec le proxy, POST ne donne rien d'intéressant.<br/>
Par contre, dès que je sens que la solution vient des méthodes, je tente toutjours la méthode OPTIONS, qui est censée nous fournir toutes les méthodes authorisées.

Le résultat est donc : `GET, POST, CLEAN, OPTIONS`
CLEAN<br/>
"Roomba qui aspire", ou autrement dit "qui **nettoie**", CLEAN en anglais

Je passe la méthode à CLEAN <br/>
J'ai en réponse un cookie `session: *une longue chaine*` <br/>
Je forward donc la réponse, le cookie est ajouté. <br/>
Je reviens sur ma page de compte (connecté), et apparaît en bas de ma page de compte :

> vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv
>
> CYBN{VVvvvVVvvVvvVvvvvvvVVVVVV}

🚩 


{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{VVvvvVVvvVvvVvvvvvvVVVVVV}`
{{< /admonition >}}
