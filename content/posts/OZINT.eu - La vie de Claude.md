---
title: "OZINT.eu   La Vie De Claude"
date: 2023-10-22T14:32:55+02:00
draft: false
tags: ['OSINT']
categories:
  - 'Writeups'
  - 'Flag4All'
---

# Flag4All - OZINT.eu La Vie De Claude
---

## Category

> OSINT

### Files

{{< image src="/Claude/enonce.1.email.jpg" caption="énoncé" title="énoncé" alt="énoncé" >}}

## Part 1/2

### Description

Qui sont les deux auteurs du documentaire ? (on demande les noms de famille dans l'ordre alphabétique)

Format de la réponse : FLAG{dupont_durand}

### Difficulty

**EASY** - 25 points

- Author: **[OZINT](ozint.euu)**
---

{{< image src="/Claude/screen.png" caption="Screenshot" title="Screenshot" alt="Screenshot" >}}

### Write up

We could try to look for her userame @claudedlm, but it will be only useful for the next part.

Nope, for this one, only google the words of the documentary: **documentaire 2010 esclaves ile**

The first links talks about ___'Les esclaves oubliés de Tromelin'___, a 2010 documentary about slaves on an island.

{{< image src="/Claude/authors.png" caption="Authors" title="Authors" alt="Authors" >}}

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{ragobert_roblin}`
{{< /admonition >}}

## Part 2/2

### Description

Le fils de Claude s'interroge sur un bien immobilier qu'il a vu cette année, saurez-vous répondre à ses questions ?

Format de la réponse : FLAG{143_245800}

### Difficulty

**Medium** - 481 points

- Author: **[OZINT](ozint.euu)**
---

{{< image src="/Claude/screen2.png" caption="screen" title="screen" alt="screen" >}}

### Write up

#### Social networks

It's time for **Social Intelligence**. In her email, claude has written her **@claudelm**. Just try a tool like [WhatsMyName](https://whatsmyname.app/) for username lookups.
We then find her **Mastodon** account: [here](https://mastodon.social/@claudedlm)

{{< admonition type=note title="Fediverse" open=true >}}
In her email, she talks about 'Fediverse'. Search for it, and you'll learn that it talks about decentralized social networks, and gives Mastodon as an exemple. That was another hint.
[Article](https://www.radiofrance.fr/franceinter/podcasts/veille-sanitaire/veille-sanitaire-du-vendredi-08-septembre-2023-8005700)
{{< /admonition >}}

Her very first post confirms that we are on the good person (about old documents). The [next post](https://mastodon.social/@claudedlm/110509127588935440) is what we need to find her son:
{{< image src="/Claude/edf.png" caption="EDF" title="EDF" alt="EDF" >}}


This post teaches us that:
- Her name is "**De La Mergerie**"
- Actually, she divorced recently, and the name of her ex-husband is "**Vainsseau des taies**".

Now that we have her ex-lastname, we can search for it on... *Facebook for example?*

Bingo, [her profile](https://www.facebook.com/profile.php?id=100093107636294)

{{< image src="/Claude/son.png" caption="Claude's son" title="Claude's son" alt="Claude's son" >}}

Here is [Gabriel](https://www.facebook.com/profile.php?id=100093519045761), her son, that liked the picture and gives us directly his profile.

There is this post about a city and a sold house:

{{< image src="/Claude/post.png" caption="Castle's pic" title="Castle's pic" alt="Castle's pic" >}}

He talks about a trip he did in the city with the **castle**,
that near this **pylon** there is a **pharmacy** in front of a **residential area**, across a **large road**,
where he saw a **house** being sold.

1. Where is this [castle](/Claude/castle.jpg) ? -> Google Lens: **King's Castle, Limerick, Ireland**
2. Where is that pylon? -> Kinda hard part. Much easier to take all four elements all at once (pylon, pharmacy, large road and residential area)

I was searching for those elements:
- green corner with sidewalk: still in the city but not the center
- orange brick wall
- Trees
- semi-big pylon

{{< image src="/Claude/pylon.png" caption="Pylon" title="Pylon" alt="Pylon" >}}

By *wandering on street view*, this is the only zone that approximately matched,
and I was able to find the same type of pylon on the edge of the city.

{{< image src="/Claude/pylons.png" caption="Search zone" title="Search zone" alt="Search zone" >}}

Then I found that this zone could match: pharmacy, residential area across the large road. This gotta be in **Coolraine Heights** 

{{< image src="/Claude/zone.png" caption="Match zone" title="Match zone" alt="Match zone" >}}

By searching **coolraine heights sold house** on Google, this [site](https://proper.ie/county/Limerick/area/Coolraine%20Heights) looked promising.
We were looking for a house sold between **25/02/2023 and 05/03/2023**.

We got our matching house!
{{< image src="/Claude/house.png" caption="Match!" title="Match!" alt="Match!" >}}

Another search with the address will easily give us the area: **111 m²**

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{111_290000}`
{{< /admonition >}}
