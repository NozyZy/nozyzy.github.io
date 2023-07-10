---
title: "HeroCTF v5 - EMD"
date: 2023-05-23T23:22:43+02:00
draft: false
tags: ['Steganography', 'Images']
categories:
  - 'Writeups'
  - 'HeroCTFv5'
---

# HeroCTF v5 - EMD
---

## Category

> Steganography

## Description

Our agents discovered that a group of Breton extremists were communicating with each other through postcards representing the landscape of the region. We have managed to intercept an image and we are convinced that it contains information that is crucial to dismantling this group.
Some information to help you:
- The size of the pixel groups is 2
- The size of the hidden message is 841 characters
- The image must be divided into groups containing 2 pixels that follow each other on the x-axis.
- No secret key has been used during the encryption, so there is no random permutation of the pixels
  Good luck!

This challenge uses the most basic "Exploiting Modification Direction" technique described by Xinpeng Zhang and Shuozhong Wang in their paper "Efﬁcient Steganographic Embedding by Exploiting Modiﬁcation Direction"

Format : **Hero[]** WARNING : For this challenge, square brackets [] are used and not braces {}.

## Files

{{< image src="/EMD/vannesHidden.png" caption="Embedded image" title="Embedded image" alt="Embedded image" >}}


## Difficulty

**HARD** - 500 points

- Author : **Thibz**
---


## Write up

Some data has been hidden in the file with a relatively non-common method (not LSB for example), so our steps will be :
- find documentation about this method
- understand how it works
- extract

### Find documentation

The technique called "Exploiting Modification Direction" has few papers and documents online, often related to those two names. The first one I found was long and complex: [EMD](https://www.sciencedirect.com/science/article/pii/S0898122110000179)

That doesn't fit with the precise name given in the description: "Efﬁcient Steganographic Embedding by Exploiting Modiﬁcation Direction"
Typing that sentence gives us teh right link first: [IEEE](https://ieeexplore.ieee.org/document/4020540)
We just need to sign in and download that odf file: [EMD PDF](/EMD/3_Efficient20Steganographic20Embedding20by20Exploiting20Modification20Direction.pdf)

### Understand the process

{{< admonition tip "EMD EMBEDDING" >}}
The interesting part is the "**II. EMD EMBEDDING**", wich is not that long. I advise you to read it.
{{< /admonition >}}

After reading some math stuff, we learn that we can use groups of {{< raw >}}\( n \){{< /raw >}} pixels to hide data in base {{< raw >}}\( 2n + 1 \){{< /raw >}}
In our case, groups of 2 pixels are used, so {{< raw >}}\( n = 2 \){{< /raw >}} and {{< raw >}}\( 2n+1 = 5 \){{< /raw >}}. Data is hidden in base-5.

The second thing to understand, is that this technique uses a grayscale image to hide data, because it needs a single decimal value, not 3 values like RGB. Grayscale is just a value between 0 (black) and 255 (white).
In each pixel group (2 in our case), {{< raw >}}\( g_{i} \){{< /raw >}} is the gray value of the {{< raw >}}\( i \){{< /raw >}} pixel, where {{< raw >}}\( i \){{< /raw >}} starts at 1, not 0.
The very important function to hide data is called {{< raw >}}\( f \){{< /raw >}}, which is a sum of all {{< raw >}}\( g_{i} * i \){{< /raw >}} in a group, modulo {{< raw >}}\( 2n+1 \){{< /raw >}}.
{{< raw >}}\( f \){{< /raw >}} is called the **extraction function**, and is also use to embed data. 
In our case, {{< raw >}}\( f(g_{1}, g_{2}) = (g_{1} + g_{2} * 2) mod 5\){{< /raw >}}

This is all we have to know for the extraction process. Really.
It looks horrifying first, then the math are not that complicated, and extraction is easy : sum, multiplication with and modulo.

{{< admonition tip "How to extract data" false >}}
- Groups of n pixels
- For each pixel in group, multiply the pixel value by its position (1,2,3,...)
- Add the result to the previous result
- Result modulo (2n+1)
- The result is the data, in base-(2n+1)
{{< /admonition >}}

### Extraction

Time to do some Python.

I'm going to write first the extraction function that extract the data from a group of n pixels.

```python
# digit extraction
def extract(grays):
        n = len(grays)
        m = (2*n + 1)

        f = 0
        # f += g_i * i
        for i in range(len(grays)):
                f += grays[i] * (i+1)
        f = f % m

        return f
```
> This code is literally the implementation of {{< raw >}}\( f(g_{1}, g_{2},..., g_{n}) = [\sum_{i=1}^i(g_{i} * n)] mod (2n+1)\){{< /raw >}}

Then, we can open the image using [Pillow](https://pypi.org/project/Pillow/), and make pixel groups:

```python
# Open image
img = Image.open('vannesHidden.png')

n = 2

# Divide image in pixel groups
pixel_groups = [
                (img.getpixel((x, y)), img.getpixel((x + 1, y))) 
                    for y in range(img.height) 
                        for x in range(0, img.width, n)
                ]
```

Having that, we can process to the extraction of the base-5 data: 

```python
# get the extracted value for each pixel group
# turn it in str, not int
# concatenate
base5 = ''.join(map(str,[extract(pixels) for pixels in pixel_groups]))
```

We got that kind of stuff:
```
322404401424401112431404401112421344401342420112433401420431432424401430112430421112
402342424112410420413342420400134112431404401112312401403410421420342413112303342431
432424342413112310342424412112421402112431404401112241432413402112421402112302421424
343410404342420112424401414342410420430112342420112421422401420112430422342344401112
434410431404112414432413431410422413401112410420402413432401420344401430112431404342
431112422424421432400413441112344413342410414430112410431430112424421421431430112342
420400112344432413431432424401141112112112301410430431401400112410420112431404401112
232413432343112421402112431404401112414421430431112343401342432431410402432413112343
342441430112410420112431404401112434421424413400134112431404401112241432413402112421
402112302421424343410404342420112410430112404421414401112431421112430421414401112402
421424431441112410430413342420400430112420401430431413401400112410420112342112430414
...
```

That is a very large amount of data, but we're gonna be smart.

Knowing a part of the plain text can help us a lot, and we know that the flag start with `Hero`. So we can convert `Hero` in base 5, and look for it in our sh*t.

`Hero : 242 401 424 421`

Time for some python again: 
```python
flag = ''
# We look for 'Hero'
begin = base5.find('242401424421')
while not flag.endswith(']'):
        # Convert from base5 (2n + 1) for n = 2 pixels
        # we admit that each char is composed of 3 digits
        flag += chr(int(base5[begin:begin + 3], 5))
        begin += 3
print(flag)
```

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `Hero[V4NN3S_C17Y_F7W]`
{{< /admonition >}}

---

#### Full code

```python
from PIL import Image

# Fonction d'extraction du digit
def extract(grays):
        n = len(grays)
        m = (2*n + 1)

        f = 0
        for i in range(len(grays)):
                f += grays[i] * (i+1)
        f = f % m

        return f


# Ouvrir l'image PNG
img = Image.open('vannesHidden.png')

n = 2
# Diviser l'image en groupes de pixels
pixel_groups = [(img.getpixel((x, y)), img.getpixel((x+1, y))) for y in range(img.height) for x in range(0, img.width, n)]

base5 = ''.join(map(str,[extract(pixels) for pixels in pixel_groups]))


flag = ''
# On cherche 'Hero'
begin = base5.find('242401424421')
while not flag.endswith(']'):
        # On converti depuis la base5 (2n + 1) pour n = 2 pixels
        flag += chr(int(base5[begin:begin+3],5))
        begin += 3
print(flag)
```


