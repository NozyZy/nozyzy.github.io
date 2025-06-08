# Dark Side


## Category

> Steganography

## Description

:fr:
Vous êtes étudiant en informatique et une mystérieuse personne a envoyé un dossier zip par e-mail à l’ensemble de votre classe.
Cette personne affirme que des réponses à vos prochains examens se trouvent un compte en ligne et que les indices menant à son compte son cachées dans ce dossier zip.
À vous de retrouver ces réponses sur le compte.

:uk:
You are a IT student and a mysterious person send an archive to the classe.
This person tells that all the answers for the following exams are behind an only account. To find this account, you have to follow the hidden hints that the archive will give you.
Your turn to find this answers!

## Files

Chall_Dark_Side.zip

## Difficulty

**Medium** - 497 points

- Author: **?**
---

## Write up

### zip ? tif ?

Opening the zip with **7z** lets you see that it contains another zip file, which is **encrypted**, and a **tif image**.

{{< image src="/Darkside/zip.png" caption="Zip content" title="Zip content" alt="Zip content" >}}

Maybe the tif image contains the password? But the image looks **corrupted**, I cannot open it.

What if we check its **file signature**? Using the ``file`` command, we learn that this file is considered as a **JPEG** file:
````bash
$ file IMG_20221113_185634.tif
IMG_20221113_185634.tif: JPEG image data
````

Changing the extension from `.tif` to `.jpeg` does not work.

{{< admonition question "TIF or JPEG?" >}}
Maybe it is **not a real JPEG file**?
{{</ admonition >}}

Gotta check the **file signature** with my own eyes.

[This page](https://en.wikipedia.org/wiki/List_of_file_signatures) refers a lot of hexadecimal file signatures, and [hexed.it](https://hexed.it/) allows us to looks for the hexadecimal writting of our file.

{{< image src="/Darkside/file_header.png" caption="Original header" title="Original header" alt="Original header" >}}
This header gives to the file its signature. It is considered as JPEG image data because it begins with the JPEG header, as this screen below shows:

{{< image src="/Darkside/jpeg_headers.png" caption="JPEG header" title="JPEG header" alt="JPEG header" >}}
But we've seen that this image is **not** a proper JPEG file. Maybe it is a **TIF** file, as the extension supposes, and we just need to change the header for a TIF one?

{{< image src="/Darkside/tif.png" caption="TIF header" title="TIF header" alt="TIF header" >}}
We just replace ``FF D8 FF E0`` in the header by `49 49 2A 00`, and it should be considered as a real ``.tif`` file

{{< image src="/Darkside/new_header.png" caption="New header" title="New header" alt="New header" >}}

Export the file, and
{{< image src="/Darkside/new.png" caption="Fixed file" title="Fixed file" alt="Fixed file" >}}
[new file](/Darkside/new.tif)

### Password and a bird

{{< admonition info "base64 password" >}}
We got a **base64** encoded string: ``bW90ZGVwYXNzZXppcA==`` → `motdepassezip`
{{</ admonition >}}

We are now able to unzip the zip (woaw), with inside a little bird inside a QR code:

{{< image src="/Darkside/qr.png" caption="QR" title="QR" alt="QR" >}}

{{< admonition info "base32 password" >}}
This time its content is **base32** encoded string: ``IBCDI4TLL5JTCZDFG43A===`` → `@D4rk_S1de76`
{{</ admonition >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{@D4rk_S1de76}`
{{< /admonition >}}

