---
title: "You are mine!"
date: 2023-11-25T05:38:54+01:00
draft: false
tags: ['Forensics']
categories:
  - 'Writeups'
  - 'Cybernight 2023'
---

# Cybernight 2023 - You are mine!
---

## Category

> Forensics

## Description

> J'ai réussi à extraire la RAM d'un administrateur de Youaremine !
>
> Aidez-moi à me loguer, je dois modifier quelque chose dans le panneau d'administration !
> 
> Trouvez le nom d'utilisateur de l'administrateur sur le site web.
> 
> Le flag sera à soumettre sous la forme MALICE{nom_utilisateur}

## Files

dump.ram

## Difficulty

**HARD** - 150 points

- Author: **Sysdream**
---

![img.png](img.png)

## Write up

### The very hot fox

We got a dump of a Win 10 machine, and we are looking for web artifacts.

I am going to use [volatility3](https://github.com/volatilityfoundation/volatility3) for this WU, it could possibly be done using volatility2.

using the command ``windows.pstree.PsTree``, we see that **firefox** is running.
```bash
$ vol3 -f dump.ram windows.pstree.PsTree
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime

4       0       System  0xfa80018ab040  80      622     N/A     False   2018-03-21 15:27:49.000000      N/A
* 248   4       smss.exe        0xfa80020f5990  2       29      N/A     False   2018-03-21 15:27:49.000000      N/A
...
1864    1824    explorer.exe    0xfa80032a47f0  36      930     1       False   2018-03-21 15:27:57.000000      N/A
* 296   1864    VBoxTray.exe    0xfa8003352920  13      154     1       False   2018-03-21 15:27:57.000000      N/A
* 2012  1864    DumpIt.exe      0xfa8001be0920  6       92      1       False   2018-03-21 15:37:28.000000      N/A
* 2340  1864    firefox.exe     0xfa80021932e0  56      778     1       True    2018-03-21 15:37:04.000000      N/A
** 2024 2340    firefox.exe     0xfa80019bfb30  24      288     1       True    2018-03-21 15:37:08.000000      N/A
** 2784 2340    firefox.exe     0xfa8001b9c060  23      278     1       True    2018-03-21 15:37:09.000000      N/A
** 1676 2340    firefox.exe     0xfa8001919060  23      300     1       True    2018-03-21 15:37:06.000000      N/A
```

We might probably look into the files firefox stores in the computer, we'll maybe find some cookies or session data.

Using the ``windows.filescan.FileScan`` command, we can filter all the files containing **firefox** in its path. I already know that it stores lot of data in `.db` and `.sqlite` files, both sqlite database format.

We got that result:

```bash
$ cat files.txt| grep -i firefox
0x574c9c80      Volume2\Users\user\AppData\Local\Mozilla\Firefox\Profiles\6k1fhany.defa 216
0x5759adf0      \Users\user\AppData\Local\Mozilla\Firefox\Profiles\6k1fhany.default\s   216
...       216
0x7fcd9070      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\addons.json       216
0x7fcdee60      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\webappsstore.sqlite       216
0x7fce2c10      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\extensions.json   216
0x7fce5070      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\webappsstore.sqlite-wal   216
0x7fce52d0      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\places.sqlite     216
0x7fce5420      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\places.sqlite-shm 216
0x7fce5570      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\places.sqlite-wal 216
0x7fce56c0      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\webappsstore.sqlite-wal   216
0x7fce5810      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\webappsstore.sqlite       216
0x7fce5f20      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\webappsstore.sqlite-shm   216
0x7fce7070      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\favicons.sqlite   216
0x7fce7dc0      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\favicons.sqlite-wal       216
...
0x7fd97210      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\places.sqlite     216
...
0x7fee9a90      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\gmp-widevinecdm\1.4.8.1008\manifest.json  216
0x7feea480      \Users\user\AppData\Roaming\Mozilla\Firefox\Profiles\6k1fhany.default\cookies.sqlite    216
```

I filtered using only the important files.

### My secret username

We can now use the command to dump these files, which is ``vol3 -f dump.ram windows.dumpfiles.DumpFiles --physaddr <addr>``

The first useful is ``places.sqlite`` which stores the history of the websites visited, which can be opend using `sqlite3` or a tool like **DataGrip**

```sql
$ sqlite3
SQLite version 3.43.2 2023-10-10 12:14:04
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .open file.0x7fd97210.0xfa8001ae6ae0.SharedCacheMap.places.sqlite.vacb
sqlite> .open file.0x7fd97210.0xfa8001ae6ae0.SharedCacheMap.places.sqlite.vacb
sqlite> .tables;
Error: unknown command or invalid arguments:  "tables;". Enter ".help" for help
sqlite> .tables
moz_anno_attributes    moz_historyvisits      moz_keywords
moz_annos              moz_hosts              moz_places
moz_bookmarks          moz_inputhistory
moz_bookmarks_deleted  moz_items_annos
sqlite> select * from mos_places;
Parse error: no such table: mos_places
sqlite> select * from moz_places;
1|https://support.mozilla.org/fr/products/firefox||gro.allizom.troppus.|0|0|0|137||xVbLHMURrI44|1|47357880692093||
..
11|http://172.30.30.105:8001/|Login Form|501.03.03.271.|2|0|1|4000|1521640712387000|QXFyrROE90Ww|0|125510699223928||
12|http://172.30.30.105:8001/login|405 Method Not Allowed|501.03.03.271.|1|0|0|0|1521639810249000|VdgiStx2JmZh|0|125510885845080||
13|https://www.mozilla.org/fr/firefox/59.0.1/whatsnew/?oldversion=58.0.2|Découvrez les nouveautés de Firefox|gro.allizom.www.|1|0|0|100|1521640706447000|uPwmEOps9vcr|0|47356943968719|Plus rapide et plus réactif, tout en défendant votre vie privée. Jetez un œil aux dernières évolutions du navigateur qui vous protège.|https://www.mozilla.org/media/img/firefox/template/page-image.4b108ed0b8d8.png
14|http://youaremine.ctf:8001/|Login Form|ftc.enimerauoy.|1|0|1|2000|1521645923509000|slQFReqGxzFm|0|125511387997629||
sqlite>
```

{{< admonition note "Smart request" >}}
```sql
sqlite> select * from moz_places where url like '%youaremine%';
14|http://youaremine.ctf:8001/|Login Form|ftc.enimerauoy.|1|0|1|2000|1521645923509000|slQFReqGxzFm|0|125511387997629||
```
{{</ admonition >}}

We can now gather information about the website we are looking for :
- url : **http://youaremine.ctf:8001**
- ip : **172.30.30.105**
- numbers (?) : **501.03.03.271.**
- another url (?) : **ftc.enimerauoy**

We could possibly look into the ``cookies.sqlite``, but this does not contain any information about this website.

However, according to the [Mozzila documentation](https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data#:~:text=a%20fresh%20copy-,Profiles%20-%20Where%20Firefox%20stores%20your%20bookmarks%2C%20passwords%20and%20other%20user,special%20folder%20called%20a%20profile), we could search into:
- ``key4.db`` which stores the encryption key for the ``logins.json``, where are stored the encrypted format of users, passwords. We couldn't manage to find ``logins.json`` in the memory, I guess this was not supposed to be recoverable.
- ``webappsstore.sqlite`` which is related to the storing of web application data, using the DOm and not cookies.

![img_1.png](img_1.png)

This file was recoverable at the address ``0x7fce5810``. See above.

{{< admonition info "Doubled file" >}}
The file was found twice by volatility. The first one was empty.
Furthermore, dumping it gives two files: a ``.dat``, which is also empty, and a ``.vacb``, which was the right one to use.
{{</ admonition >}}

We could now look into it:

```sql
$ sqlite3
SQLite version 3.43.2 2023-10-10 12:14:04
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .open file.0x7fce5810.0xfa8002195010.SharedCacheMap.webappsstore.sqlite.vacb
sqlite> .tables
webappsstore2
sqlite> select * from webappsstore2;
|moc.xoferif.stnuocca.:https:443|moc.xoferif.stnuocca.:https:443|__fxa_storage.experiment.q3FormChanges|"{\"enrolled\":true}"
...
|501.03.03.271.:http:8001|501.03.03.271.:http:8001|state|{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6ImFyY3RpY21vbmtleSJ9.grDw5SlOEWCrN1NC0oapTFJZnAD1KfdpDlwUyYmNiYpVTL4_KvHtzdsFkjCt_25Mc282-VS80jUu2iRfS9XT7UxiYdWkktCFly2kPprQUgI9ICH21Kjgnh-dubdnnvdWKOcSUSjKWO7oyA4tvKMGWZiBXLbYc-8_iftoO_vI02Pii2X1VXFlWUMNvVKVDQVlo4KIfW1XdaZpuaiXGwX-FdOXocPS3Fx-gin-m-GNLNdjxSIFczFD6gESINPh0-LnV1K2mO-7xPJFLwcprAzFpu5cZAyDyhVuvVeFEMrn6Kmjl8xlrEKwMbXleoxoPqGCi601QIGkGbUxOZdHywzWvw"}
|ftc.enimerauoy.:http:8001|ftc.enimerauoy.:http:8001|state|{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6ImFyY3RpY21vbmtleSIsImV4cCI6MTUyMTY0OTU4M30.LTDRrIx40dtgOnAAJdvR85LLIJktqJ_T8fa7L2hjq9gK36P5RyiBhLjCv4KwpmddDuFXUOJgVNvJexDZOvdBDmLbEvBTmirn1BVaEaqYrpXEgowRP4Os_GAJ1fwvkyoYwyoeipsB5Yko98dKKvHO1wY6n37C0nz-cHtYrPk9R2AtCILYhYkl_X4axDZeol5I_iS-NBijxCPBMqp1VHRvd5xj2-8ig3MTogqtlx5aat5AUV7yoHdknXmnnYKUH0ktlKMgxIxyQRi0wzLqB6wL5Nb-fLzAL6f0oT-JCbe6-6YqP_Ro-IpJ8y3DSn7G-umcrzvrTw4PhTG62kTOBA5Kiw"}
```

{{< admonition note "Smart request" >}}
```sql
sqlite> select * from webappsstore2 where originKey like '%8001%';
originAttributes|originKey|scope|key|value
|501.03.03.271.:http:8001|501.03.03.271.:http:8001|state|{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6ImFyY3RpY21vbmtleSJ9.grDw5SlOEWCrN1NC0oapTFJZnAD1KfdpDlwUyYmNiYpVTL4_KvHtzdsFkjCt_25Mc282-VS80jUu2iRfS9XT7UxiYdWkktCFly2kPprQUgI9ICH21Kjgnh-dubdnnvdWKOcSUSjKWO7oyA4tvKMGWZiBXLbYc-8_iftoO_vI02Pii2X1VXFlWUMNvVKVDQVlo4KIfW1XdaZpuaiXGwX-FdOXocPS3Fx-gin-m-GNLNdjxSIFczFD6gESINPh0-LnV1K2mO-7xPJFLwcprAzFpu5cZAyDyhVuvVeFEMrn6Kmjl8xlrEKwMbXleoxoPqGCi601QIGkGbUxOZdHywzWvw"}
|ftc.enimerauoy.:http:8001|ftc.enimerauoy.:http:8001|state|{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1c2VybmFtZSI6ImFyY3RpY21vbmtleSIsImV4cCI6MTUyMTY0OTU4M30.LTDRrIx40dtgOnAAJdvR85LLIJktqJ_T8fa7L2hjq9gK36P5RyiBhLjCv4KwpmddDuFXUOJgVNvJexDZOvdBDmLbEvBTmirn1BVaEaqYrpXEgowRP4Os_GAJ1fwvkyoYwyoeipsB5Yko98dKKvHO1wY6n37C0nz-cHtYrPk9R2AtCILYhYkl_X4axDZeol5I_iS-NBijxCPBMqp1VHRvd5xj2-8ig3MTogqtlx5aat5AUV7yoHdknXmnnYKUH0ktlKMgxIxyQRi0wzLqB6wL5Nb-fLzAL6f0oT-JCbe6-6YqP_Ro-IpJ8y3DSn7G-umcrzvrTw4PhTG62kTOBA5Kiw"}
```
{{</ admonition >}}

We can recognize the indicators we spotted above (port used, string and numbers) that confirm this is about our targeted website.

The token is a JWT base64 encoded, a token used for authentification and data exchange between client and server. Decoding it resolves in this:

```json
{
    "username": "arcticmonkey",
    "exp": 1521649583
}
```


{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `MALICE{arcticmonkey}`
{{< /admonition >}}
