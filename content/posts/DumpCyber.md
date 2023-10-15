---
title: "DumpCyber"
date: 2023-10-13T19:51:18+02:00
draft: false
tags: ['forensics', 'dump']
categories:
  - 'Writeups'
  - 'ECW 2023'
---

# ECW 2023 prequals - DumpCyber
---

## Category

> Forensics

## Description

I am lost inside this dump, to escape I think I need my key and IV?
The challenge is available [here](https://drive.google.com/file/d/12qaHQ5s3Yu4MD9clevbdwjmUNBsQ3q9h).
Note: the flag format for this challenge is `flag{...}`.

Challenge made by: Yogosha

## Difficulty

**Medium** - 500 points

- Author: **SemahBA**
---

[screenshot](screen.png)

## Write up

### Understanding the context

#### Profile and objective
We got a RAM dump on a Windows 7, we'll use [volatility](https://github.com/volatilityfoundation/volatility) for the analysis.

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ vol -f task.raw imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/nozzy/Documents/ECW/task.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800027fd0a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff800027fed00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2023-08-17 16:20:26 UTC+0000
     Image local date and time : 2023-08-17 17:20:26 +0100```
```

The description lets us know that we'll have to look for a **key** and an **IV** (initial vector), which are used for symmetric encryption. The most common one is **AES**, and its most common mode using an IV is **CBC**. We'll keep that in mind.

#### Processes and commands
I am used to checking for the last commands run by the user, and what processes are running. So I ran the commands `cmdline` and `pstree`.
The only processes of interest were `notepad.exe` and `WinRar.exe` : 

cmdline:
```
WinRAR.exe pid:   2144
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\vboxuser\Desktop\c"
************************************************************************
WinRAR.exe pid:   1280
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\vboxuser\Desktop\generator.rar"
************************************************************************
notepad.exe pid:   2880
Command line : "C:\Windows\system32\NOTEPAD.EXE" C:\Users\vboxuser\Desktop\deesktop.ini
************************************************************************
notepad.exe pid:   2792
Command line : "C:\Windows\system32\NOTEPAD.EXE" C:\Users\vboxuser\Desktop\dessktop.ini
************************************************************************
WinRAR.exe pid:   2624
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\vboxuser\Desktop\file.rar"
```
Pstree:
```
0xfffffa80041c8510:explorer.exe                     1160   1140     36    969 2023-08-17 16:13:47 UTC+0000
. 0xfffffa8004480b30:VBoxTray.exe                    1428   1160     13    137 2023-08-17 16:13:48 UTC+0000
. 0xfffffa8000dc4b30:DumpIt.exe                      1320   1160      2     45 2023-08-17 16:20:19 UTC+0000
. 0xfffffa8001000b30:WinRAR.exe                      1280   1160      6    188 2023-08-17 16:20:22 UTC+0000
. 0xfffffa8000e2cb30:WinRAR.exe                      2624   1160      6    188 2023-08-17 16:20:25 UTC+0000
. 0xfffffa8001025b30:WinRAR.exe                      2144   1160      6    188 2023-08-17 16:20:21 UTC+0000
. 0xfffffa8000f8fb30:notepad.exe                     2880   1160      1     61 2023-08-17 16:20:23 UTC+0000
. 0xfffffa8000fe8b30:notepad.exe                     2792   1160      1     61 2023-08-17 16:20:24 UTC+0000
```

Two files seamed to be edited by the user : `deesktop.ini` and `dessktop.ini`
Note that they are fake versions of the common `desktop.ini` that exists on Windows. They might look interesting.

Then, three rar archives also exist : `file.rar`, `generator.rar` and `file.txt.rar`

#### Dumping the files

Now that we found interesting stuff, we're going to **dump** them and ake a look at it using the `filescan` and `dumpfiles` commands.

Using the first one, we are able to find every file loaded in memory by the computer, and locate their physical offset in the dump.
Here ar all the files we're looking for :

```
0x000000003fd737b0     16      0 RW---- \Device\HarddiskVolume1\Users\vboxuser\Desktop\deesktop.ini
0x000000003fcd2430     16      0 RW---- \Device\HarddiskVolume1\Users\vboxuser\Desktop\dessktop.ini
0x000000003fa18070     16      0 RW---- \Device\HarddiskVolume1\Users\vboxuser\Desktop\file.txt.rar
0x000000003fa144a0     16      0 RW---- \Device\HarddiskVolume1\Users\vboxuser\Desktop\generator.rar
0x000000000ef58430     16      0 RW---- \Device\HarddiskVolume1\Users\vboxuser\Desktop\file.rar
```
{{< admonition type=note title="dumpfiles" open=true >}}
We'll now use the hexadecimal offset to dump their content, with the command: `vol -f task.raw --profile=Win7SP1x64 dumpfiles -Q <offset> -D .`
{{< /admonition >}}

We now have these files:

```
deesktop.ini:   file.None.0xfffffa8003e37b10.dat
dessktop.ini:   file.None.0xfffffa8000c7d9e0.dat
file.txt.rar:   file.None.0xfffffa800487b860.dat
generator.rar:  file.None.0xfffffa8003fc9160.dat
file.rar:       NONE
```

All the dumped files are **rar archives**:

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW/dump]
└─$ for FILE in *; do file $FILE; done
file.None.0xfffffa8000c7d9e0.dat: RAR archive data, v5
file.None.0xfffffa8003e37b10.dat: RAR archive data, v5
file.None.0xfffffa8003fc9160.dat: RAR archive data, v5
file.None.0xfffffa800487b860.dat: RAR archive data, v5
```

Using `unrar x <file>` we can extract everything, that gave us the 4 resulting files `deesktop.ini  dessktop.ini  file.txt.enc  generator.exe`.

{{< admonition type=info title="nopad" open=true >}}
At this point we got:

- Two fake `desktop.ini` containing unreadable data.
- `file.txt.enc` with unreadable bytes data, apparently encrypted according to the extension .enc. This might be our encrypted flag.
- `generator.exe`, a 32-bit Windows executable file.
{{< /admonition >}}



### Keys and decryption

My first try, even before checking for the context, was to look for the key in memory with [findaes](https://sourceforge.net/projects/findaes/files/findaes-1.2.zip/download). This is a tool that I already used in that [article](/articles/decrypt-luks1-with-key-in-memory/), which allows us to look for AES keys stored in a dump file.
That worked, we got two 128-bits AES keys : 

```
Found AES-128 key schedule at offset 0x3655164:
3d 68 2f 1a 3d 5e 04 20 8e 23 29 75 bd e6 c5 9b
Found AES-128 key schedule at offset 0x106b9260:
29 60 1b 94 90 87 c9 fa a6 de f2 91 c4 8d b9 c7
```

At this point, I thought that I only had to find for the IV, which is 16 bytes (128-bits) long.

By chance, both our `deesktop.ini` and `dessktop.ini` were 16-bytes long files:

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW/dump]
└─$ xxd deesktop.ini && xxd dessktop.ini
00000000: e609 874a 80d1 b27e fd46 cc0a 131a 4dcd  ...J...~.F....M.

00000000: 1b54 ee42 0bd5 b839 6e15 fc9f e010 55f8  .T.B...9n.....U.

┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ wc -c dessktop.ini && wc -c deesktop.ini
16 dessktop.ini
16 deesktop.ini
```

Or, if we take a look closely at the content of both our files, none of them corresponds to the keys we previously found with aesfinder.

Before trying some dumb combinaisons, we need to figure out the purpose of the last file: `generator.exe`
I ran it with win, in 32-bit config, under linux, in an empty directory:

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW/test]
└─$ ls
generator.exe

┌──(nozzy㉿shoggoth)-[~/Documents/ECW/test]
└─$ wine generator.exe
Key and IV written to files.

┌──(nozzy㉿shoggoth)-[~/Documents/ECW/test]
└─$ 
ls
deesktop.ini  dessktop.ini  generator.exe
```

This is what we wanted: to confirm that both `.ini` files are the Key and the IV. We do not have to use the keys found with aesfinder.

Decrypting is the last step:

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ openssl enc -aes-128-cbc -d -in file.txt.enc -out file.txt -K '1b54ee420bd5b8396e15fc9fe01055f8' -iv 'e609874a80d1b27efd46cc0a131a4dcd'
bad decrypt
4037C9F7C87F0000:error:1C800064:Provider routines:ossl_cipher_unpadblock:bad decrypt:../providers/implementations/ciphers/ciphercommon_block.c:124:
```
{{< admonition type=danger title="nopad" open=true >}}
Error with the pad? What if we remove it with `-nopad`?
{{< /admonition >}}

```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ openssl enc -aes-128-cbc -d -in file.txt.enc -out file.txt -K '1b54ee420bd5b8396e15fc9fe01055f8' -iv 'e609874a80d1b27efd46cc0a131a4dcd' -nopad

┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ cat file.txt
=�;K   ��Al��
                0flag{82a30fadcfc07d634fbed1bffe4a2aa1}
```

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `flag{82a30fadcfc07d634fbed1bffe4a2aa1}`
Key : `dessktop.ini` -> `1b54ee420bd5b8396e15fc9fe01055f8`
IV : `deesktop.ini` -> `e609874a80d1b27efd46cc0a131a4dcd`
{{< /admonition >}}

{{< admonition type=question title="And aesfinder ?" open=true >}}
aesfinder could have found other keys in the dump. However, he couldn't manage to find ours, which are correctly visible in the hex dump of `task.raw`:
```
┌──(nozzy㉿shoggoth)-[~/Documents/ECW]
└─$ xxd task.raw | grep -C1 '1b54 ee42 0bd5'
01977030: 736b 746f 702e 696e 690a 0302 94a1 57b6  sktop.ini.....W.
01977040: fba4 d901 1b54 ee42 0bd5 b839 6e15 fc9f  .....T.B...9n...
01977050: e010 55f8 1d77 5651 0305 0400 0000 0000  ..U..wVQ........
```
{{< /admonition >}}

{{< admonition note "TL;DR" true >}}
- suspect ``.ini`` and ``.rar`` files used respectively by ``notepad.exe`` and ``WinRar.exe``
- dumping and extracting files
- ``generator.exe`` creates to files: 
  - the Key `dessktop.ini` 
  - the IV `deesktop.ini` 
- decrypt ``file.txt.enc`` with dumped `dessktop.ini` and `deesktop.ini`
{{< /admonition >}}
