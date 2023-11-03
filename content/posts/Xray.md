---
title: "Xray"
date: 2022-11-03T23:37:49+01:00
draft: false
tags: ['Forensics', 'Steganography']
categories:
  - 'Writeups'
  - 'Cybernight 2022'
---

# Cybernight 2022 - Xray
---

## Category

> Forensics

## Description

> Pouvez-vous voir ce qui se cache derrière ce message ?

## Files

[XRAY](/Xray/XRAY-1.pdf)

## Difficulty

**EASY**

- Author: **MrSheepSheep**
---

## Write up

On a un pdf avec écrit ![img_2.png](/Xray/img_2.png)

On cherche donc comment extraire des informations d'un pdf, là où le 'xray pdf' ne donne pas grand-chose

Je tombe sur https://www.pdf-online.com/osa/extract.aspx?o=img en cherchant '**pdf analyser**'
Celui-ci demande un pdf en input et essaie de sortir du texte et des images

Le texte ne donne rien, mais les images :
{{< image src="/Xray/img.png" caption="Embedded Image" title="Embedded Image" alt="Embedded Image" >}}


{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{Hid3_&_S3ek}`
{{< /admonition >}}
