---
title: "Gishadb"
subtitle: "Flag4All - Gishadb"
date: 2023-10-22T03:13:42+02:00
draft: false
tags: ['Forensics', 'Realistic']
categories:
  - 'Writeups'
  - 'Flag4All'
---

## Category

> Realistic

## Description

Ce serveur écoute sur 2 ports ! 443 et 222. Trouvez un moyen d'obtenir le flag.

This server listen on two ports: 443 and 222. Find a way to get the flag.

## Link

https://gishadb.flag4all.sh

## Difficulty

**EASY** - 481 points

- Author: **Penthium2 (BZHack)**
---

{{< image src="/Gishadb/screen.png" caption="Screenshot" title="Screenshot" alt="Screenshot" >}}

## Write up

### .git dump

I highly recommend you to have [DotGit](https://github.com/davtur19/DotGit) extension installed for CTF and web audits.
It automatically checks for you if a **.git directory is exposed**. And that's what happened here when I opened the page. This extension also comes with an extraction tool.

{{< image src="/Gishadb/dotgit.png" caption="Dump it !" title="Dump it !" alt="Dump it !" >}}

After the whole extraction is done, we can first take a look at the git logs with ``git log``:

{{< image src="/Gishadb/commits.png" caption="commits" title="commits" alt="commits" >}}

Then I used ``git show <commit id>`` to show the content of each commit. And it didn't disappoint me:

{{< admonition type=info title="1. The main page content (not useful) AND db credentials (ofc useful)" open=false >}}
```diff
commit 02ef7e50a0bb9954d7819f8fd7c7b2c593db0d73
Author: penthium2 <penthium2@hotmail.fr>
Date:   Thu May 25 14:44:03 2023 +0200

    ipremier commit

diff --git a/index.php b/index.php
new file mode 100644
index 0000000..20e4fa0
--- /dev/null
+++ b/index.php
@@ -0,0 +1,13 @@
+<H1>Je suis un maitre du PHP et SQL</h1>
+<?php
+$cnx = new PDO('mysql:host=127.0.0.1;dbname=app;charset=utf8', 'app', 'db_Super_P4$$');
+
+// Partie "Requête"
+
+echo "<p>dans ma table <b>HAND</b></p>";
+$query = 'SELECT * FROM `hand` where id=1';
+$resultSet = $cnx->query($query);
+$element = $resultSet->fetch();
+echo "<p> voici le $element[1] de $element[2] et son id : $element[0]<p>";
+?>
+<h2> mais quel est la valeur de l'id 3 ?? </h2>
```
{{</admonition>}}

{{< admonition type=info title="2. \"Fixing\" the credentials leak (but they did not change)" open=false >}}

```diff
commit 6dbcaf8c22bbe74aa2562ac60f58602d8592347a
Author: penthium2 <penthium2@hotmail.fr>
Date:   Thu May 25 14:50:32 2023 +0200

    sécurisation

diff --git a/config.php b/config.php
new file mode 100644
index 0000000..35d2322
--- /dev/null
+++ b/config.php
@@ -0,0 +1,3 @@
+<?php
+$cnx = new PDO('mysql:host=127.0.0.1;dbname=app;charset=utf8', 'app', 'db_Super_P4$$');
+?>
diff --git a/index.php b/index.php
index 20e4fa0..5cd8a7d 100644
--- a/index.php
+++ b/index.php
@@ -1,7 +1,7 @@
 <H1>Je suis un maitre du PHP et SQL</h1>
 <?php
-$cnx = new PDO('mysql:host=127.0.0.1;dbname=app;charset=utf8', 'app', 'db_Super_P4$$');
-
+// ajout d'un iclude pour la s<C3>curit<C3>
+include("config.php");
 // Partie "Requête"

 echo "<p>dans ma table <b>HAND</b></p>";
```
{{</admonition>}}

{{< admonition type=info title="3. some SSH credentials! (very useful)" open=false >}}
```diff
commit 13c0faef33bf466602913bc3918094cfefea072f
Author: penthium2 <penthium2@hotmail.fr>
Date:   Thu May 25 14:52:44 2023 +0200

    ajout info ssh pour preprod

diff --git a/info_login b/info_login
new file mode 100644
index 0000000..d904e9b
--- /dev/null
+++ b/info_login
@@ -0,0 +1,2 @@
+loginssh : gishadb@192.168.124.1
+pass : iop
```
{{</admonition>}}

{{< admonition type=info title="4. Removing SSH creds (they di not change too)" open=false >}}
```diff
commit cfdccf9472e0b985abc372f90fa64352d744b5ad (HEAD -> master)
Author: penthium2 <penthium2@hotmail.fr>
Date:   Thu May 25 14:53:28 2023 +0200

    suppresion info preprod

diff --git a/info_login b/info_login
deleted file mode 100644
index d904e9b..0000000
--- a/info_login
+++ /dev/null
@@ -1,2 +0,0 @@
-loginssh : gishadb@192.168.124.1
-pass : iop
```
{{</admonition>}}

We now have:
- Database credentials
- SSH credentials

### Database dump

Then plan now is to connect to the server, and access the local database (not accessible remotely). However, a little thing will bother us after connecting to the port **222**:

![Fuck](/Gishadb/fuckssh.gif)

This program is impossible to leave, just have to close the terminal.

But anyway, we will go through an **SSH tunnel**, using ``-L`` flag,
that allows us to forward one port of the server to our machine.

We'll look for MySql port, **3306**
Using ``-N`` will also not prompt the green binary disaster.

Command is: ``ssh -N gishadb@gishadb.flag4all.sh -p 222 -L 3306:localhost:3306``

We can now, in another terminal, connect to the remote MySql server using ``app:db_Super_P4$$``:

```bash
$ mysql -u app -p -h 127.0.0.1
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 100238
Server version: 10.5.21-MariaDB-0+deb11u1 Debian 11
```

Now, need to show the databases, its tables, and then its content : 

```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| app                |
| information_schema |
+--------------------+

MariaDB [(none)]> use app
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [app]> show tables;
+---------------+
| Tables_in_app |
+---------------+
| hand          |
| users         |
+---------------+

MariaDB [app]> select * from hand;
+----+-----------------------------+----------+
| id | finger                      | ring     |
+----+-----------------------------+----------+
|  1 | pouce                       | Penthium |
|  2 | index                       | Penthium |
|  3 | FLAG{REDACTED}              | Penthium |
|  4 | anulaire                    | Penthium |
|  5 | oriculaire                  | Penthium |
+----+-----------------------------+----------+
```

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{TAK3_Care_Of_SSH_POWA}`
{{< /admonition >}}
