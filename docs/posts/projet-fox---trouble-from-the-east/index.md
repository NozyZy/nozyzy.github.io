# Projet FOX   Trouble From the East


# Flag4All - Projet FOX Trouble From the East
---

## Category

> OSINT

## Description

Les services de contre-espionnage ont appris que 3 pilotes de la Bundeswehr ont été recrutés par S. B., un homme d'affaires chinois et fils d'un officier de l'Armée Populaire de Libération. Afin d'être payés, nous savons qu'ils ont chacun établi une société écran. Vous avez pour tâche de retrouver l'identité de chacune de ces entreprises.

Format du flag : FLAG{nom de l'entreprise/nom de l'entreprise/nom de l'entreprise} (par ordre alphabétique)

## Difficulty

**Medium** - 448 points

- Author: **[Projet FOX](https://projetfox.com/)**
---

{{< image src="/Trouble/screen.png" caption="Screen" title="Screen" alt="Screen" >}}


## Write up

### The Story

This challenges needs a lot of understanding of the story beneath. The beginning is tough, because we do not have clear information. Just go and Google dork: ``"Bundeswehr" "son of a People's Liberation Army officer"``

We then find this [article](https://www.spiegel.de/international/germany/suspicious-activity-what-are-german-fighter-pilots-doing-in-china-a-25ac852d-887d-454b-8d73-02a595c83c32) on fist link

To recap:
- Su Bin, a chinese businessman, has employed three ex-Bundeswehr pilots for its own business in China.
- The pilots are accused of spying for China.
- This story comes from 2013.
- This was written by two german journals: [spiegel.de](https://www.spiegel.de/international/germany/suspicious-activity-what-are-german-fighter-pilots-doing-in-china-a-25ac852d-887d-454b-8d73-02a595c83c32) and [zdf.de](https://www.zdf.de/nachrichten/politik/china-bundeswehr-piloten-kampfflieger-ausbildung-100.html)

The three men are not fully identified, but they are called:
- Alexander "Limey" H.
- Dirk J. (Tornado pilot)
- Peter S.

In both articles, we learn that proofs have been found in the **Panama papers**, a massive offshore data leaks.

We could probably find their companies in those data.

### Panama Papers

The first article specifies that it is in the **Panama Papers - Mossack Fonseca** that the leak provides,
and that their companies were based in the **Seychelles**.

This [link](https://offshoreleaks.icij.org/search?c=&cat=1&d=pp&j=&q=Dirk+J) is where this database can be found. Here with filters to find our Dirk.
I couldn't apply more filters, because the site refuses to expand the menus, so I had to Ctrl+F a lot in those results.

I was searching for:
- **Dirk J**
- **Mossack Fonseca** leak
- Juridiction: **Seychelles**
- Linked to **Hong Kong**
- From **2013**

**[Dirk Janssen](https://offshoreleaks.icij.org/nodes/12189428)** and its company **[GATOR CONSULTING LTD](https://offshoreleaks.icij.org/nodes/10028179)** fully matched the first.

Same research for **Alexander H** leads us to **[Alexander Hoening](https://offshoreleaks.icij.org/nodes/12210273)** and **[Phamivity Consult Ltd](https://offshoreleaks.icij.org/nodes/10191954)**

{{< admonition type=note title="Article" open=true >}}
Its company was also in this [article](https://www.spiegel.de/international/germany/suspicious-activity-what-are-german-fighter-pilots-doing-in-china-a-25ac852d-887d-454b-8d73-02a595c83c32). It may have helped for the research. I didn't see it first lol
{{< /admonition >}}

**Peter S.** gave too many results. So I had to Google dork, using his previous camarades pages: ``"peter" "PANAMA PAPERS - MOSSACK FONSECA" "Hong Kong" "26-AUG-2013"``

This gives us only one, and good result: **[Peter Helmut Schmitt](https://offshoreleaks.icij.org/nodes/12189467)** and its company **[FlyHi Consulting LTD](https://offshoreleaks.icij.org/nodes/10032671)**

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `FLAG{flyhi consulting ltd/gator consulting ltd/phamivity consult ltd}`
{{< /admonition >}}

