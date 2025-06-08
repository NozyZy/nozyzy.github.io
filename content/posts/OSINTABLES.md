---
title: "OSINTABLES"
subtitle: "404CTF - OSINTABLES"
date: 2023-06-04T23:34:58+02:00
draft: false
tags: ['OSINT']
categories:
  - 'Writeups'
  - '404CTF'
---

## Category

> OSINT

## Part 1/3

### Description

En pleine discussion au Procope, Cosette vous raconte autour d'une part de fraisier la première fois qu'elle a essayé d'envoyer une lettre à son bienfaiteur : Jean Valjean.
Débutante dans la démarche postale, elle s'est trompée sur les informations nécessaires, elle en a même oublié l'adresse du destinataire, n'écrivant que la sienne. Elle vous montre la photo ci-jointe et vous met au défi de trouver d'où elle l'a écrite.

---

Trouvez l'adresse d'envoi de la lettre (celle de Cosette).
Format : 404CTF{numero_adresse_rue_ville}

exemple : 404CTF{36_quai_des_orfevres_paris}

### Files

{{< image src="/osintables/photo.jpg" caption="Picture of the letter" title="Picture of the letter" alt="Picture of the letter" >}}


### Difficulty

**EASY** - 200 points

- Author : **Neptales**
---

{{< image src="/osintables/screenshot.png" caption="Chall screenshot" title="Chall screenshot" alt="Chall screenshot" >}}

### Write up

We got a letter written on it

> my home (Cosette)
> LXXXIII
> 
> Victor Hugo Street
> VE.......
> 
> Q4......

The capital letters ar certainly romanian number, so LXXXIII = 83, that could be the street number
Then we have the street, Victor Hugo
And probably the beginning of the city's name, VE...

What if I type what I have in GMAPS ?

{{< image src="/osintables/maps%20screen.png" caption="gmaps result" title="gmaps result" alt="gmaps result" >}}

The first result could match... I guess ?

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `404CTF{83_rue_victor_hugo_vergeze}`
{{< /admonition >}}

---

## Part 2/3

### Description

Impressionée par votre déduction Cosette décide donc de s'ouvrir à vous, elle recherche désespérement l'adresse de Jean Valjean mais ne la trouve pas.

Elle dit ne connaître que quelques informations à son sujet :
- il habite Paris
- il est dans un des immeubles avec le plus d'étages à proximité de sa station de métro.
- il déteste les buveurs d'alcool.
- il aime être proche de son argent.

Elle vous donne également son adresse email: **jean.valjean750075@gmail.com**

Essayez de prendre contact avec lui pour trouver son adresse.

---

Format : 404CTF{numero_adresse_rue}

exemple : 404CTF{36_quai_des_orfevres}

### Difficulty

**Medium** - 968 points

- Author : **Neptales**
---

{{< image src="/osintables/screenshot2.png" caption="Chall screenshot" title="Chall screenshot" alt="Chall screenshot" >}}

### Write up

We need to find him now.
Having a gmail address, we know we can find intersting things with tools like Ghunt or [Epieos](https://epieos.com/)

If we give his email to Epieos, we effectively get something intereting:


{{< image src="/osintables/epieos.png" caption="Epieos result" title="Epieos result" alt="Epieos result" >}}

Gmaps's contribution and G+ are empty... However, the calendar have some information for us:

{{< image src="/osintables/calendar.png" caption="calendar" title="calendar" alt="calendar" >}}

Jean Valjean scheduled a special week in paris in May, and each of these appointments have a place and time, and even a duration from his home :

{{< image src="/osintables/first.png" caption="first appointment" title="first appointment" alt="first appointment" >}}

JVJ uses a different mean fo transport for each these appointments. So, we need to find a tool that calculates the area where he could have travelled from, and search for overlapping locations.

I found this website : [Smappen](https://www.smappen.fr/app/)
It allows us to make what we wanted, find a location by its travel duration, classed by meaning of transport.


{{< image src="/osintables/map.png" caption="overlapping maps" title="overlapping maps" alt="overlapping maps" >}}

I made that map, and deduced that the first appointment was made by bike, because others transports were overlapping too much, or not at all.
I circled two probable zones, the little one being the most precise: Pigalle.

I went to Pigalle in maps, near the metro station.
My goal was to find:

- one of the tallest building
- far from any bar
- near a bank

I haven't been far away from the station, and found the nearest bank from the station.
The building is 7 floors up, biggest equivalent around.
No bar around.

{{< image src="/osintables/pigalle.png" caption="7 place pigalle" title="7 place pigalle" alt="7 place pigalle" >}}

It could be the 7 Pl. Pigalle (bank address) or the 16 Frochot Street (apartment entry)

The more logical to me, the apartment, was not good, however the bank was good !

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `404CTF{7_place_pigalle}`
{{< /admonition >}}


---

## Part 3/3

### Description

Cosette a enfin trouvé l'adresse de Jean Valjean, mais il n'est pas chez lui. La jeune fille en a assez : elle le retrouvera coûte que coûte ! Mais comment faire, lorsque tout ce à quoi elle a accès est un aperçu d'un bureau derrière une fenêtre ?
Retrouvez le lieu où se trouve Jean Valjean.

---

Format : 404CTF{numero_adresse_rue}

exemple : 404CTF{36_quai_des_orfevres}

### Difficulty

**EXTREME** - 1000 points

- Author : **Kamiro#3528**

### Files

{{< image src="/osintables/osintables.jpg" caption="JVJ desk" title="JVJ desk" alt="JVJ desk" >}}

---

### Write-Up

If only...
