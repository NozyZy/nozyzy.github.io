---
title: "404CTF - Notes de bas de page"
subtitle: "404CTF - Notes de bas de page"
date: 2023-05-24T00:48:44+02:00
draft: false
tags: ['Forensics']
categories:
  - 'Writeups'
  - '404CTF'
---

## Category

> Forensics

## Description

En tendant l'oreille dans le salon, votre attention est captée par une conversation particulière. Là où les alentours dissertent sur des sujets de procédés littéraires toujours plus avancés, celle-ci semble en effet plutôt porter sur la philosophie naturelle. Vous vous approchez par curiosité, lorsque la marquise du Châtelet, alors en pleine explication des travaux d'un certain Leibniz, s’interrompt et vous interpelle :

« Vous me voyez ravie de vous rencontrer ici ! Nous n'avons pas eu la chance de nous rencontrer, mais on m'a parlé de vous et l'on m'a assuré que vous étiez formidable lorsqu'il s’agissait de retrouver les choses perdues. Voyez, j'ai travaillé il y a peu sur un projet qui me tient à cœur, et j'ai bien peur d'avoir perdu le fruit de mon travail par un triste accident. Heureusement, j'avais échangé à ce sujet avec un ami proche, et il est bien possible que notre correspondance ait gardé la trace d'une note de bas de page que je souhaiterais tout particulièrement retrouver. Auriez vous l’amabilité de m'assister dans ma mésaventure ? »


Aidez la marquise à retrouver la note qu'elle a perdue grâce à une sauvegarde de ses correspondances.

## Files

[backup.pst](/notesdebasdepage/backup.pst)

## Difficulty

**HARD** - 1000 points

- Author : **Smyler#7078**
---
{{< image src="/notesdebasdepage/chall.png" height="50%" caption="Chall screenshot" title="Chall screenshot" alt="Chall screenshot" >}}


## Write up

### Opening file

We got a `backup.pst`, and I don't know personally what it is. Let's search for that.

{{< image src="/notesdebasdepage/pst.png" caption="How to open pst" title="How to open pst" alt="How to open pst" >}}


I am on windows, so I will open the backup with Outlook.
It looks like this :
{{< image src="/notesdebasdepage/outlook.png" caption="Outlook data" title="Outlook data" alt="Outlook data" >}}


It talks about an important data sent in attached file. 
Here is the screenshot :   
{{< image src="/notesdebasdepage/Capture%20d’écran%202023-05-07%20210840.png" caption="Screenshot" title="Screenshot" alt="Screenshot" >}}

We can see at the bottom a `404CTF{`, which is our track. But how to recover it ?

### Acropalypse

In order to understand how we can recover it, following the recent cyber news should have helped a lot. Indeed, in March, a new vulnerability on Google Pixels cropped screenshot came, with a POC of how we can recover the original non-cropped image.
{{< x user="ItsSimonTimeItsSimonTime" id=1636857478263750656 >}}

Few days after, the same kind of vulnerability was found on Windows Snipping Tool :
{{< x user="David3141593" id=1638222624084951040 >}}

{{< admonition info "What is acropalypse ?" true >}}
Instead of **erasing** cropped data from the file, an **IEND chunk is added** in the file to cut the rest of the image.
The file size is in fact identical before and after cropping.
{{< /admonition >}}

### Recover

> Know more about PNG {{< link "https://en.wikipedia.org/wiki/PNG" >}}

We have the file, the false chunk, so we change it into IDAT chunk to recover the full image ?

That is not so easy. I didn't want to try recover the image by hand, because I knew it was complex, and that people have already written tools for that.

The biggest part of this chall for me, was to find the right tool tu use. I searched on Google, Twitter, Reddit, then directly on GitHub.

I'll help you, I found this one : {{< link "https://github.com/Absenti/acropalypse_png" >}}


The command to use was like 
```shell
python acropalypse_png.py restore windows "Capture d’écran 2023-05-07 210840.png" restore.png
```

The script takes few minutes to recover data, and find the resolution of the image, juste be patient, and then:

{{< image src="/notesdebasdepage/recovered.png" caption="Recovered image" title="Recovered image" alt="Recovered image" >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `404CTF{L3_f0rM1d@bl3_p09re35_d3s_lUm13re5}`
{{< /admonition >}}

