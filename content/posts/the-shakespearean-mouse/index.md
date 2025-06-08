---
title: "The Shakespearean Mouse"
subtitle: "BackdoorCTF 2023 - The Shakespearean Mouse"
date: 2023-12-18T23:57:11+01:00
draft: false
tags: ['Steganography', 'Forensics']
categories:
  - 'Writeups'
  - 'BackdoorCTF 2023'
---

## Category

> Forensics

## Description

> wend f'rward with this quest of yours, f'r thee shalt findeth the flag at the endeth Attachments: https://staticbckdr.infoseciitr.in/shakespeareanmouse.zip

## Files

[shakespeareanmouse.zip](https://staticbckdr.infoseciitr.in/shakespeareanmouse.zip)

## Difficulty

496 points

- Author: **cy4n1d3**
---

![img.png](img.png)

## Write up

### The Shakespearan Langage
Inside the zip file we got two files: **out.bmp**, and **The Backdoor Lore** containing only text, at first sight, the script of drama play.

I rapidly think of a so of [esoteric language](https://esolangs.org/wiki/Main_Page), like [Poetic](https://esolangs.org/wiki/Poetic) or [Whitespace](https://esolangs.org/wiki/Whitespace), which are programming languages hidden in sor of "normal" text.

By searching for "Shakespeare" in this wiki, we go [that page](https://esolangs.org/wiki/Shakespeare). There is indeed a language called like this, and it looks really similar to ou text file:

{{< image src="img_1.png" caption="The Backdoor Lore compared to Shakespeare syntax" title="The Backdoor Lore compared to Shakespeare syntax" alt="The Backdoor Lore compared to Shakespeare syntax" >}}

We then search for an online compiler or interpreter, and get on [that website](https://esolangpark.vercel.app/ide/shakespeare) for example. It allows us to run the program in live:

![gif](https://i.imgur.com/pIo9rua.gif)

Output text:
> Hey guys i hope you loved shakespearean english, i always did. Anyways here is the password you might need, ohh not easily 4f440a001006a49f24a7de53c04eca3f79aef851ac58e460c9630d044277c8b0. Use it well ;)

### The password

We then get that password `4f440a001006a49f24a7de53c04eca3f79aef851ac58e460c9630d044277c8b0`, which looks like a 32-bytes hash, as SHA-256.

It is, indeed, a 32-bytes hash, but the output of a less famous algorithm. By looking on the internet the hexadecimal string, we ge that [unique page](https://q9f.github.io/secp256k1.cr/Secp256k1/Util.html), where we see tha it corresponds to **keccak(0xdeadbeef)**

{{< image src="img_2.png" caption="Corresponding password for hash" title="Corresponding password for hash" alt="Corresponding password for hash" >}}

The password is: **0xdeadbeef**

### A password, what for?

This is the moment I went 'we need to use the image'. It isn't called **out.bmp** for nothing, it indicates that it is probably the output of some sort of hiding tool. A tool that can embed data in image, using a password.

The boring step here is to find **the right tool**. By looking at all "stegano tool" that talks about the "bmp" format, I wasn't able to find anything.

However, on the [CTF Checklist](https://stegonline.georgeom.net/checklist) page of Stegonline, I found [OpenStego](https://www.openstego.com/), which doesn't quote the "bmp" format on its main page.
I was searching a tool for another chall, and I found out that it only supports "bmp" and "png" files to embed data, with a password.

With no more surprises, it is the right tool we were looking for:

{{< image src="img_3.png" caption="OpenStego extracts a .zip" title="OpenStego extracts a .zip" alt="OpenStego extracts a .zip" >}}

### The Mouse

Since the name of this challenge, we were talking about a mouse.
It makes complete sense here.

In the zip file extracted, we found two files: **finaldon.rec** and **instructions.txt**
The first one is not recognized, and the second one is just boring "shakespeare spoken" useless text.

If we take a look at the hexadecimal header of the **.rec** file, and its UTF-8 conversion, we find crucial information:

{{< image src="img_5.png" caption="Website and tool name" title="Website and tool name" alt="Website and tool name" >}}

We can get on the [automacrorecorder.com](https://automacrorecorder.com) website, and download **Auto Mouse Recorder**. Then we can read the **.rec** file:

{{< image src="img_6.png" caption="Mouse macro" title="Mouse macro" alt="Mouse macro" >}}

When we play the macro, we see the mouse moving around the top left corner, moving more and more to the right, sometime while maintaining the button pressed.

**It is drawing**

We can open paint and replay the macro:

![gif_draw](https://i.imgur.com/Y5H6vrF.gif)

![img_7.png](img_7.png)

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `flag{a_w1s3_m4n_kn0w5_h1ms3lf_to_b3_a_f00l_a1337}`
{{< /admonition >}}
