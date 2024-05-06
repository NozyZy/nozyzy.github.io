---
title: "S0uS_l3s_r4d4r"
date: 2024-04-29T23:17:07+02:00
draft: false
tags: ['Steganography', 'Image']
categories:
  - 'Writeups'
  - 'Midnight Flag 2024'
---

# Midnight Flag 2024 - S0uS_l3s_r4d4r
---

## Category

> Steganography

## Description

*By memory*
> Recover the hidden flag
> 
> Format : MCTF{flag{XXXXXXXXX}}

## Files

[Gorille_wallpaper.jpg](Gorille_wallpaper.jpg)

## Difficulty

- Author: **kuorashi**
---

## Write up

[Aperisolve](https://aperisolve.fr/470d65fbda570aee3a4da6fef39e33ae) does not give anything interesting at first sight. But if you scroll into the **strings**, you'll find something interesting.

Push the thing a way further, doing this:

```bash
strings -n 8 Gorille_wallpaper.jpg
...
@wzJPB))v
,U#=b0t`
{La[!dm7
qB)P',W18b
import string; alph = string.ascii_letters;flag_enc = "109 115 104 110 { 130 111 131 104 111 132 108 122 123 145 104 122 149 121 118 119 149 118 123 }"
def decode_flag(flag):
    lst = flag.split(" ")
    new_flag = ""
    for item in lst:
        if item != '{' and item != '}':
            new_flag += alph[int(item) % 25]
        else:
            new_flag += item
    print(new_flag)
def encode_flag(flag):
    text = ""
    for i in flag:
        if i not in alph:
            text += i + " "
        else:
            text += f"{alph.index(i) + int(flag_enc.split(' ')[2])} + "
    print(text)
decode_flag(flag_enc)
1s;iWC)7buckd
GhQEgPh1
Ddl00wHX7
...
```

There is some python code:
```py
import string
alph = string.ascii_letters
flag_enc = "109 115 104 110 { 130 111 131 104 111 132 108 122 123 145 104 122 149 121 118 119 149 118 123 }"
def decode_flag(flag):
    lst = flag.split(" ")
    new_flag = ""
    for item in lst:
        if item != '{' and item != '}':
            new_flag += alph[int(item) % 25]
        else:
            new_flag += item
        print(new_flag)
def encode_flag(flag):
    text = ""
    for i in flag:
        if i not in alph:
            text += i + " "
        else:
            text += f"{alph.index(i) + int(flag_enc.split(' ')[2])} + "
        print(text)
decode_flag(flag_enc)
```

SO the given code... decodes the flag?

```python
jpek{flgelhiwxuewyvstysx}
```

Nope, that doesn't work. Even with **ROT**, thats dioes not work properly.

So, instead of thinking how to reverse this simple code, let's create every encoded character, and reconstruct the flag:

```python
import string
alph = string.ascii_letters
flag_enc = "109 115 104 110 { 130 111 131 104 111 132 108 122 123 145 104 122 149 121 118 119 149 118 123 }"

def encode_flag(flag):
    text = ""
    for i in flag:
        if i not in alph:
            text += i + " "
        else:
            text += f"{alph.index(i) + int(flag_enc.split(' ')[2])}"
    return text

encoded_chars = {}
for a in alph:
    encoded_chars.update({encode_flag(a): a})
flag = ""
for c in flag_enc.split():
    try:
        flag += encoded_chars.get(c)
    except:
        flag += c

print(flag)

# flag{AhBahCestPasTropTot}
```

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `MCTF{flag{AhBahCestPasTropTot}}`
{{< /admonition >}}
