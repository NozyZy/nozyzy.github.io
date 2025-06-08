# J'ai pas ROTé


## Category

> Cryptography

## Description

> votre collègue, adepte de blagues plus que de cryptographie, vous envoie la phrase suivante dans la conversation d'équipe: "j'vous promets les gars, cette fois, j'ai pas ROTé: 871 1157 858 1014 1599 1287 507 663 1508 1261 1365 1508 1235 1456 676 1495 1235 637 1235 1287 663 1495 1261 1482 1625"

## Difficulty

**EASY**

- Author: **Lmeaou**
---

## Write up

Ce nom me rappelle grandement celui de l'an dernier "Burp" où il fallait comprendre que c'était du ROT13 (Brup, rot, vous l'avez ?)

Cette fois, c'est un poil trop explicite, mais on tente toutes les combinaisons de ROT avec cette chaine.

{{< image src="/jai_pas_ROTe/image-20221212112423942.png" caption="Sus WiFi" title="Sus WiFi" alt="Sus WiFi" >}}

Ca ne marche **EVIDEMMENT** pas. 


Donc à un moment j'accepte de suivre ma première idée "c'est pas du ROT, mais un genre de code ASCII"

Ce n'est pas de l'ASCII pur, car ASCII(871) = ?, donc on est loin du CYBN

OR, on voit que l'écart entre 871 et 1157 est positif, et que l'écart entre 871 et 858 est négatif et très faible

{{< admonition question "ASCII décalé" >}}
Conclusion, ça peut très bien entre un ASCII avec facteur de décalage, puisque l'écart entre C et Y est grand, et entre C et B est petit
{{</ admonition >}}

On va donc vérifier ça :

```py
flag = "871 1157 858 1014 1599 1287 507 663 1508 1261 1365 1508 1235 1456 676 1495 1235 637 1235 1287 663 1495 1261 1482 1625"
f = list(map(int,flag.split())) # transforme en list d'entiers

diff = ord('Y') - ord('C') # différence initiale entre Y et C en ASCII
di = f[1] - f[0] # différence apperçue ici

d = di / diff   # ratio (faustin)
print(d)        # on vérifie que le ratio est entier (13.0)

# pour chaque nombre, on divise par le ratio, et affiche la lettre correspondante
for s in f:
	print(chr(int(s//d)), end="")
```

{{< admonition type=success title="Flag" open=false >}}
:triangular_flag_on_post: `CYBN{c'3tait_p4s_1_c3sar}`
{{< /admonition >}}

