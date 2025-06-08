# Traque Au Scammer


## Category

> OSINT

## Description

> Un escroc donne du fil à retordre à la C3N, notamment car ils n'ont plus de budget pour engager un expert OSINT. Heureusement que vous êtes là avec un peu de temps libre ce week-end ;) Une victime auditionnée leur a fourni l'image que l'escroc envoi après avoir chaque escroquerie. Ses méfaits se concentrent principalement sur du phising puis du vol de crypto-monnaie. Le C3N compte sur vous pour retrouver son adresse et son prénom.
>
> Format du flag : AMSI{adresse_prenom}
>
> Exemple : AMSI{Montpellier-12-rue-Claret_Roger}

## Files

[FCKY.png](FCKY.png)

## Difficulty

**MEDIUM** - 500 points

- Author: **Flavio - flav-sns**
---
{{< image src="img.png" caption="Description" title="Description" alt="Description" >}}

## Write up

### Little punk

On commence avec une photo assez générique d'un meme, le contenu ne nous aidera pas à identifier le scammer.

En revanche, ce petit malin à signer son image de **son nom** :
{{< image src="img_1.png" caption="Exiftool" title="Exiftool" alt="Exiftool" >}}

Petit coup de recherche d'username avec [whatsmyname](https://whatsmyname.app/), et on trouve que ce malin possède un compte **[github](https://github.com/Punk4ch1en)** :
{{< image src="img_3.png" caption="whatsmyname" title="whatsmyname" alt="whatsmyname" >}}

### Le scammer de crypto

On peut alors se balader sur ses repos, et ses commits. Evidemment que l'on trouve quelque chose d'intéressant dans ce [commit](https://github.com/Punk4ch1en/Spinning_cat/commit/4ea912e855c603c82223b444e6cf9e494c4b22bd) :
{{< image src="img_4.png" caption="Le commit" title="Le commit" alt="Le commit" >}}

On possède alors sa "drain address", qui est une adresse crypto utilisée pour voler les gens : ``0xd9A1C3788D81257612E2581A6ea0aDa244853a91``

Le site que je recommande, et qui m'a été utile est [etherscan](https://etherscan.io), on trouve sur [la page de son adresse](https://etherscan.io/address/0xd9A1C3788D81257612E2581A6ea0aDa244853a91) certaines infos :
{{< image src="img_5.png" caption="Etherscan" title="Etherscan" alt="Etherscan" >}}

On a donc bien un rappel qu'il s'agit de scam, alors on peut rechercher des Database de report d'adresse crypto.
{{< image src="img_6.png" caption="ChainAbuse" title="ChainAbuse" alt="ChainAbuse" >}}

On n'y trouve aucun report (avant la fin du CTF, il y en a eu depuis par les joueurs).
{{< image src="img_7.png" caption="No report" title="No report" alt="No report" >}}

La solution se cache dans le fai que notre adresse est liée à une autre, qui l'a fourni en argent :
{{< image src="img_8.png" caption="Feed address" title="Feed address" alt="Feed address" >}}

On recherche alors cette adresse ``0xDCddc9287e59B5DF08d17148a078bD181313EAcC`` sur [CoinAbuse](https://www.chainabuse.com/address/0xDCddc9287e59B5DF08d17148a078bD181313EAcC)
{{< image src="img_9.png" caption="Report" title="Report" alt="Report" >}}

### Sa petite vie sur internet

On a donc la source de la personne qui a report : [https://x.com/LDfmf4883](https://x.com/LDfmf4883)

Le compte Twitter est assez vide : pas de post, pas d'archive, pas de liste, rien.
Seulement des **abonnements**.

Regardez qui voilà :
{{< image src="img_10.png" caption="Abonnés twitter" title="Abonnés twitter" alt="Abonnés twitter" >}}

Le retour de notre ami le scammer, et il se plaint sur son [compte twitter](https://x.com/cdjknd43084) d'avoir perdu son adresse crypto entre autres.

Par contre, on retrouve un petit commentaire sympa sur [un de ses posts](https://x.com/cdjknd43084/status/1860959517585899896) :
{{< image src="img_11.png" caption="Post scam" title="Post scam" alt="Post scam" >}}

Cette personne s'appelle alors **Philipe**, et possédait un petit compte perso, qui est évidemment supprimé désormais :
{{< image src="img_12.png" caption="Compte supprimé" title="Compte supprimé" alt="Compte supprimé" >}}

En revanche, une **archive** existe, mais pas sur archive.org, uniquement sur [archive.ph](https://archive.ph/tvNKB) (archive today) :
{{< image src="img_13.png" caption="Archive" title="Archive" alt="Archive" >}}

On a donc une image, prise à priori devant chez lui, avec des infos importantes :
- Boutique "Mag' Occaz"
- Une fileuse de verre
- Une cordonnerie, à l'adresse où habite le scammer

![photo](https://archive.ph/tvNKB/f6eb1b708ad0df0f7a3644331bb84e18019a1dc4.jpg)

Alors ensuite, pour trouver le lieu, je ne vous raconte pas par quoi je suis passé. \
... \
Bon ok je vous dis :
- [pappers.fr](https://papper.fr) → trouvé aucune boutique
- pages facebook → pareil
- reverse image (google, bing, yandex) → pas trouvé perso (askip on pouvait)
- overpass turbo → aucun résultat probant
- mots clé sur Google et Google Maps → boff boff

Comment j'ai trouvé ?

{{< image src="img_14.png" caption="Google Images" title="Google Images" alt="Google Images" >}}

Lien de l'article : [https://www.lamontagne.fr/saint-flour-15100/actualites/le-nouveau-reseau-electrique-de-la-rue-marchande-inaugure_11425042/](https://www.lamontagne.fr/saint-flour-15100/actualites/le-nouveau-reseau-electrique-de-la-rue-marchande-inaugure_11425042/)

On apprend que ça se passe dans la ville de **Saint-Flour** !

On peut alors chercher la fileuse de verre, le Mag' Occaz, ou direct la cordonnerie, mais on finira bien par la trouver :
{{< image src="img_15.png" caption="Street View" title="Street View" alt="Street View" >}}

{{< image src="img_16.png" caption="Adresse" title="Adresse" alt="Adresse" >}}

Nous voici chez le cordonnier, au **34 rue Marchande, à Saint-Flour**

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `AMSI{Saint-Flour-34-rue-Marchande_Philippe}`
{{< /admonition >}}

