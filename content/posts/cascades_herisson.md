---
title: "Avez-vous vu les cascades du hérisson ?"
subtitle: "404CTF - Avez-vous vu les cascades du hérisson ?"
date: 2023-05-31T19:09:23+02:00
draft: false
tags: ['Radio-frequency', 'Steganography']
categories:
  - 'Writeups'
  - '404CTF'
  
lightgallery: false

---

# 404CTF - Avez-vous vu les cascades du hérisson ?
---

## Category

> Radio-Frequency

## Description

Après avoir rencontré Simone, cette dernière vous propose de découvrir une nouvelle personne. Quoi de mieux qu’un cache-cache pour apprendre à mieux se connaître ? Cela vous changera de l'air du café littéraire Le Procope.
Vous arrivez dans un lieu qui lui est cher, un lieu d’enfance rempli de souvenirs. C’est ici qu’elle passait ces vacances d’Été, dans le parc de Meyrignac, fondé en 1880 par son grand père Ernest BERTRAND DE BEAUVOIR.

Lors de votre partie, non loin de là, vous parvenez habilement à trouver Simone, cependant le troisième joueur demeure parfaitement introuvable. Votre cerveau titanesque a eu la bonne idée de faire ce jeu dehors, en pleine nature, pour plus de difficulté. Sublime. Vous voilà errant au milieu de nulle part. Toutefois, un bruit vous attire :

« SPLASHHHH... SHHHHH... SPLASHHHH... SHHHHH... » (Le son d'une cascade d'eau qui tombe et qui ruisselle)

Cette chute d'eau paraît ordinaire et suspicieuse. Peut-être parviendrez-vous à trouver ce charmant flibustier à travers les cascades du Hérisson avant qu'il soit l'heure de rentrer au Procope ?

---

Vous avez un oeil de lynx, ainsi vous apercevez que la chute d'eau s'écoule à une fréquence de 2 MHz

Format du flag : 404CTF{ceci_est_un_flag}

## Files

*file too big, oops*
[radiofile](/cascades_herisson/Herisson)

## Difficulty

**EASY** - 1000 points

- Author : **Racoon#8487**
---

{{< image src="/cascades_herisson/img.png" height="50%" caption="Chall screenshot" title="Chall screenshot" alt="Chall screenshot" >}}

## Write up

The file given is like a .raw, it is plain data that we must convert to see frequencies and hear sound.
We can use Audacity for that, by going to **File > Import > Raw data**

{{< image src="/cascades_herisson/audacity%20screen.png" caption="Audacity import .raw" title="Audacity import .raw" alt="Audacity import .raw" >}}

Then we have our wave, that we can listen to.
And, as expected, nothing useful to hear. It is just noise, the "cascade" noise.
As I already did one chall like this, I thought that, if we cannot hear, we might be able to see. In audacity, yes.

For that, we switch the view to Spectrogram: 

{{< image src="/cascades_herisson/spectrogram.png" caption="Spectrogram view" title="Spectrogram view" alt="Spectrogram view" >}}

And then...

{{< image src="/cascades_herisson/spectrum%20audacity.png" caption="Audacity Sprectrum view" title="Audacity Sprectrum view" alt="Audacity Sprectrum view" >}}

We have something interesting. However, the image we see is compressed to the bottom, and that will be tough to read it.
This is the reason why I chose to use **Sonic Visualiser** next.

Because of that, using **All Bins Linear** options:

{{< image src="/cascades_herisson/screenshot%20sonic.png" caption="Sonic Sprectrum view" title="Sonic Sprectrum view" alt="Sonic Sprectrum view" >}}
 
What I did next is :
- screenshot the text
- rotate and stick it together in paint
- open it in photoshop (or [photopea](https://www.photopea.com/) online equivalent)
- push the contrast and intensity at max
- read and draw

{{< image src="/cascades_herisson/max%20contrast.png" caption="Max contrast" title="Max contrast" alt="Max contrast" >}}

The image is reflected, like a mirror, so there is noise on the background. We need to find (and guess) the right letters and numbers in the foreground.
Something like this:

{{< image src="/cascades_herisson/draw.png" caption="Max contrast" title="Max contrast" alt="Max contrast" >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `404CTF{413X4NDR3_D4N5_UN3_C45C4d35_?}`
{{< /admonition >}}

{{< admonition note "Une faute ?" false >}}
For french sppeakers, oui il ya une faute à 'une *cascade**s***'
{{< /admonition >}}
