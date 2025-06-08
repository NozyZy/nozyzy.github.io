# HeadShot


## Category

> Forensics

## Description

Un groupe de terroriste exfiltre des données via un jeu vidéo. Ils doivent utiliser leur sauvegarde pour cela. A partir de cette sauvegarde, trouvez les 4 parties du flag. Il n'est pas important d'installer le jeu.

## Files

[headShot.tar](https://ctfd.flag4all.sh/files/7379772026f7dae22376571d239b5239/headShot.tar.gz?token=eyJ1c2VyX2lkIjoxMTIsInRlYW1faWQiOm51bGwsImZpbGVfaWQiOjMxOH0.ZTUOPw.6FXd_EmJfzC42znDu4PMCy7AEp8)

## Difficulty

**Medium** - 497 points

- Author: **Penthium2 (BZHack)**
---

{{< image src="/HeadShot/screen.png" caption="Screenshot" title="Screenshot" alt="Screenshot" >}}

## Write up

The file consists of some minecraft backup files. To go fast, I just ``file`` all files to know exactly what was going on for each of them, and it appears that the `.dat` files were **gzip archives**.
And that are the files that will interest us first. Each of ``level.dat`` and `playerdata/5435ace1-00d5-437c-862f-77d8bacfa86e.dat` had the same parts of the flag:

```bash
$ strings level
...
pages
Welcome,
All parts of the  FLAG should be somewhere, everywhere, hidden !
Look in all you think from your HEAD !
In a box,Book,everry thing,.. JUST SEARCH
And of cours Have fun !!
Slot
...
Name
{"text":"_eAsilY_but_"}
Slot
minecraft:netherite_sword
Count
RepairCost
Damage
display
Name
{"text":"For3ns1c_St4r7"}
Enchantments
```

We got : ``_eAsilY_but_``, `For3ns1c_St4r7` and a hint about *HEAD*.

Maybe the file ``datapacks/AllMobHeads_V5.5+1.16.zip`` will be useful? spoiler: yes

It refers to a public mod. Once extracted, we have a bunch of directories and ``.json`` files, and `grep` does not give anything.

But it is a public mod, what if it was altered? Download the [original file](https://www.curseforge.com/minecraft/customization/all-mob-heads-fr/files/3077877) and compare.

{{< image src="/HeadShot/headmod.png" caption="Both \"True\" and fake files" title="Both \"True\" and fake files" alt="Both \"True\" and fake files" >}}

Then our best ``diff`` command:
```bash
$ diff -Nrq AllMobHeads_V5.5+1.16 TrueAllMobHeads_V5.5+1.16
Files AllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/creeper.json and TrueAllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/creeper.json differ
Files AllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/ee/974.json and TrueAllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/ee/974.json differ
Files AllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/ee/venom.json and TrueAllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/ee/venom.json differ
Files AllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/pig.json and TrueAllMobHeads_V5.5+1.16/data/minecraft/loot_tables/entities/pig.json differ
```

![tiens tiens](https://c.tenor.com/gVHHuzDLos8AAAAC/tenor.gif)

We got 4 json files that differ. Check one of them:

{{< image src="/HeadShot/diff.png" caption="Difference !" alt="Difference !" >}}

The differences appear the same way for the 4 files. They all give, once base64 decoded:

- ``{textures:{SKIN:{url:_w1Th_tr0lls}``
- https://soundcloud.com/mad-core/darktek-ta-geule
- ``{"textures":{"SKIN":{"url":"http://textures.minecraft.net/texture/G0_Som3t1me_d33per"}}}``
- [flag](https://www.youtube.com/watch?v=dQw4w9WgXcQ)

We gor all our 4 flag parts:
- ``_w1Th_tr0lls``
- ``G0_Som3t1me_d33per``
- ``For3ns1c_St4r7``
- ``_eAsilY_but_``

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{For3ns1c_St4r7_eAsilY_but_G0_Som3t1me_d33per_w1Th_tr0lls}`
{{< /admonition >}}


