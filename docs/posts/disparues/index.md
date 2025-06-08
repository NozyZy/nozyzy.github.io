# Disparue(s)


## Category

> OSINT

## Description

Ce CTF est particulier, car il s'agit d'une seule et grande enquête en OSINT.

Ici, je partage tous les challenges que mon équipe et moi avons résolus, par ordre de catégories.

- Author: **Oscar Zulu**
---

## Dead Man Walking

> Vous avez été sollicité(e) par la famille desespérée de Ellen Gravist, qui n'a plus de contact avec elle depuis plusieurs semaines. En rupture avec ses proches, elle ne donnait que peu de nouvelles mais montrait toutefois des signes de vie.
>
> Ils sont incapables de vous donner plus de détails sur les circonstances, ce qui explique pourquoi les forces de l'ordre n'ont pas considéré la disparition comme inquiétante...
>
> Le fichier PDF joint rassemble le peu d'informations que la famille a pu vous donner.
>
> Après leur avoir précisé que vous ne pourrez rien leur communiquer sans l'accord d'Ellen (si vous la retrouvez), mais que vous informerez celle-ci de leur inquiétude, vous acceptez la mission
>
> Bonne chance !
>
> **Dans quelle école Ellen a t-elle fait sa formation lors de sa reconversion ?**
>
> format du flag : ``udemy``
>
> ***20 points***

{{< image src="img_1.png" >}}

Début de l'histoire !

On recherche une Ellen, et son école. Son compte [linkedin](https://www.linkedin.com/in/ellen-gravist-jacoulet-a9924528a/) est l'endroit parfait pour ça.

{{< admonition success "Dead Man Walking" false >}}
:triangular_flag_on_post: `LiveMentor`
{{< /admonition >}}


## Hard Target

### Fresh

> Afin de constituer le profil, vous poursuivez vos recherches. Quel est l'intitulé de son nouveau métier, clairement indiqué dans son titre de profil ?
>
> Format du flag : ``employée de libre service``
>
> ***10 points***

Sur son compte [linkedin](https://www.linkedin.com/in/ellen-gravist-jacoulet-a9924528a/) de même.

{{< admonition success "Fresh" false >}}
:triangular_flag_on_post: `Web Content Writer`
{{< /admonition >}}


### Killing Zoe

> Vous commencez à avoir une idée de qui est Ellen, et certaines lectures sont assez claires sur son état d’esprit au moment de sa disparition… Mais elle a clairement été aidée. Quel est le prénom de la personne qui lui permet de prendre un nouveau départ dans sa vie ?
>
> Format du flag : ``antonin``
>
> ***20 points***

Ellen partage des articles de son compte [Medium](https://medium.com/@ellen.gravist).

Ce n'est pas dans l'un d'entre eux que l'on trouvera la réponse, bien que l'on apprenne l'état psychologique dans lequel se trouve Ellen (sa conversion pro et son rapport à la mort)

Sur cette page par contre, elle donne le lien de son [Wattpad](https://www.wattpad.com/user/EllenGravist), dans lequel on trouve son journal. Au cours des 5 chapitres, elle fait la rencontre de **Marianne**. Cette rencontre va bousculer sa vie.

{{< admonition success "Killing Zoe" false >}}
:triangular_flag_on_post: `Marianne`
{{< /admonition >}}


### Kiss of Death

> Vous avez trouvé son journal, qu’elle rédige avec un certain talent, mais il semble que la piste s’arrête là… Il vous faut maintenant poursuivre votre enquête pour savoir ce qu’il est advenu d’elle.
>
> De quel peintre du XVIIe siècle a-t-elle partagé une oeuvre ?
>
> Format du flag : ``Jean Jacques Picasso``
>
> ***50 points***

Les lectures de son journal nous apprennent deux choses essentielles ici :
- Elle a changé de nom, pour devenir **Perséphonia Aidoneus**
- Elle a un récent "blog sur cette vieille plateforme à \[son\] nouveau nom"

Celui-là m'a donné du fil à retordre pour pas grand-chose. En herchant des vieux sites de blog on tombe sur Skyblog (qui a fermé la veille de l'ouverture de son compte wattpad), Myspace ou encore tumblr.

Il se trouve qu'en cherchant son nom sur Tumblr, on tombe sur son [profil](https://www.tumblr.com/persephonia-aidoneus).

Dessus, elle partage une peinture de Peter Paul Rubens, datée de 1630-35.

{{< admonition success "Kiss of Death" false >}}
:triangular_flag_on_post: `Peter Paul Rubens`
{{< /admonition >}}

### Bad Company

> Avant de continuer, vous prenez quelques minutes pour faire un point et relire tous les renseignements que vous avez trouvés. Il semble que votre enquête prenne la direction d’une secte dans laquelle Ellen aurait été recrutée. Quel est le nom de cette secte ?
>
> Format du flag : ``Les oncles d'elise``
>
> ***10 points***

Le nom est cit sur son Medium et dans son journal : **Les Enfants d'Hades**

{{< admonition success "Bad Company" false >}}
:triangular_flag_on_post: `Les Enfants d'Hades`
{{< /admonition >}}


### Nothing to Lose

> Vous souhaitez en avoir le cœur net. Sans doute pourrez-vous trouver un email parmi les informations disponibles.
>
> Format du flag : ``nom.prenom@untrucavantle.com``
>
> ***70 points***

Un compte tumblr permet d'archiver des posts. Mais on peut retrouver ces posts en cliquant sur {{< fa-icon solid ellipsis-h >}}.

Un de ces posts est une photo nue floutée, probablement la photo dont elle parle dans son journal, prise par un certain G.

En dessous cette photo, on peut y trouver son mail, avec une incitation à prendre contact avec elle.

{{< admonition success "Nothing to Lose" false >}}
:triangular_flag_on_post: `persephonia.aidoneus@proton.me`
{{< /admonition >}}

## City of Industry

### Clear and Present Danger

> Comme toute organisation moderne, la secte doit avoir un canal d’information officiel. Il faudrait l'identifier.
>
> Format du flag : ``informations.com``
>
> ***50 points***

Sous un de ses posts, on retrouve le hashtag ``#enfantsdhades.com``.

Il s'agit bien de leur site web.

{{< admonition success "Clear and Present Danger" false >}}
:triangular_flag_on_post: `enfantsdhades.com`
{{< /admonition >}}

### Judge Dredd

> Obtenez le pseudo du responsable de la sécurité.
>
> exemple : ``mickey``
>
> ***150 points***

Il y a deux façons de faire.

Ma première trouvaille a été ce commentaire, laissé sous le premier post d'Ellen :

{{< image src="img_2.png" >}}

La page tumblr de cette personne nous renvoie vers son [blog](https://kronosantihades.wordpress.com), où il dénonce les activités de la secte.

Son dernier poste parle d'un certain dénommé **H.**, responsable de la sécurité. Notre cible.

{{< image src="img_3.png" caption="H. Responsable Sécurité" title="H. Responsable Sécurité" alt="H. Responsable Sécurité" >}}

Or si l'on clique sur le nom du post, on ouvre le post en entier. Ce dernier porte l'url : ``https://kronosantihades.wordpress.com/2023/09/05/heracles-sans-doute-le-pire-de-tous/``

On en déduit qu'**Heracles** est son pseudo.

Mais puisque je ne suis pas quelqu'un de simple, j'ai d'abord trouvé la seconde solution.

En regardant le ``/robots.txt`` du site de la secte, une page en particulier retient notre attention :

```
User-Agent: *
Sitemap: https://enfantsdhades.com/sitemap.xml
Disallow: /CHANGELOG.txt
...
Disallow: /vfrrghertevb34535lkh2Ohh00hLhkl0iujd39hhkbekzjiceluiquivoitcaestfort909878987
Allow: /safebrowsing/diagnostic
...
Disallow: /nullze/
```

La page ``https://enfantsdhades.com/vfrrghertevb34535lkh2Ohh00hLhkl0iujd39hhkbekzjiceluiquivoitcaestfort909878987/`` permet un directory listing, donc de se ballader dans les sous-dossiers :
{{< image src="img_4.png" >}}


À l'intérieur de ``/org`` se trouve comme seul fichier un ``organigramme.pdf``, contenant ces informations :

{{< image src="img_6.png" >}}

Nous retrouvons bien le pseudo de notre responsable sécurité.

{{< admonition success "Judge Dredd" false >}}
:triangular_flag_on_post: `enfantsdhades.com`
{{< /admonition >}}


### Albino Alligator

>Le responsable de la sécurité doit savoir beaucoup de choses ! Il faut à tout prix l'identifier ! Trouvez sa véritable identité.
>
> format du flag : ``godefroy salsepareille`` (prénom nom)
>
> ***200 points***

Pour celui-là, il fallait avoir trouvé l'organigramme.

Ce dernier nous apprend que chaque membre de la direction possède un mail de la forme ``nom@enfantsdhadescom``, donc celui d'Heracles est ``heracles@enfantsdhades.com
``

À l'aide d'un outil de recherche de ompte en ligne à partir d'un email, tel qu'[Epieos](https://epieos.com/), on peut trouver que notre bonhomme possède un compte [Trello](https://trello.com).

{{< image src="img_7.png" >}}

Or, l'obtention des détails du compte nécessite un compte payant à Epieos. Mais il y a moyen de faire sans.

En se créant un compte sur Trello, on peut ajouter des personnes par leur adresse mail à notre espace de travail. Or là, il fallait avoir une astuce technique.

Puisque Trello auto-détecte le compte en saisissant uniquement l'adresse mail, le client effectue donc une requête pour récupérer les infos de ce compte :

{{< image src="img_8.png" >}}

On peut alors trouver dans les requêtes web envoyées, la réponse au fetch de ce compte :

{{< image src="img_9.png" >}}

On apprend, grâce à cela, le nom du compte Trello : `heracles34`.

En s'inspirant du lien de mon compte public, je peux donc trouver celui d'Heracles grâce à son pseudo : https://trello.com/u/heracles34

Ce dernier nous donne accès à son tableau, avec des tâches concernant la secte. L'une d'elle le concerne directement, puisqu'elle contient un lien [Proton Drive](https://drive.proton.me/urls/C5AHVF5V04#S6gzfi8xkgcY), avec un document administratif à son nom dedans :

{{< image src="img_10.png" >}}

Heracles s'appelle en réalité **Guillaume Tacheleron**

{{< admonition success "Albino Alligator" false >}}
:triangular_flag_on_post: `Guillaume Tacheleron`
{{< /admonition >}}

## Guilty as Sin

### Psycho

> En vous basant sur ce que vous avez trouvé jusqu’ici, vous devriez pouvoir ajouter un nouveau chapitre à votre dossier. Quel pseudo utilise le chef de la secte ?
>
> format du flag : ``poseidon``
>
> ***30 points***

Tout en haut de l'organigramme, on retrouve **Sarapis**

{{< admonition success "Psycho" false >}}
:triangular_flag_on_post: `Sarapis`
{{< /admonition >}}

## Faster Pussycat! Kill! Kill!

### Captives

> Au vue des informations que vous avez trouvées, nul doute qu’Ellen n’est pas la seule victime. Pourrez-vous découvrir les pseudonymes des autres jeunes femmes sous influence ?
>
> Format du Flag : ``alexia siri`` (ordre alphabetique)
>
> ***100 points***

L'étape pour y arriver, bien qu'assez détachée du challenge à mon sens, eu été **d'envoyer un mail à persephonia.aidoneus@proton.me**.

Voici la réponse que nous obtenons :

```
Es tu prêt à comprendre que notre corps n'est qu'un vaisseau destiné a servir les plaisirs et les souffrances de notre âme ?
ul23ocgxwgj7wi455ncpiegylrzqmisd7uwh2hxjsmxgy2wjgax35tad.onion

NB : Les éléments créés dans le cadre de cette enquête et dans ce mail sont purement fictifs et strictement interdits aux mineurs.
```

Un lien tor, qui amène sur une page qui parle des 3 nouvelles recrues qui passeront bientôt, et certainement, la cérémonie pour devenir des filles d'Hades.

Les trois noms sont : **Persephone**, **Demeter** et **Macarie**

{{< admonition success "Captives" false >}}
:triangular_flag_on_post: `demeter macarie`
{{< /admonition >}}

### Heavenly Creatures

> Vous avez découvert un site internet terrifiant… comme vous l’imaginiez, ces jeunes filles ne sont pas protégées par la secte, mais au contraire manipulées pour devenir les proies d’hommes prêts à payer pour abuser d’elles. Bien décidé à les sortir de ce piège, vous continuez votre enquête. Quel est l’auteur des photos du site ?
>
> Format du flag : ``alain deloin``
>
> ***50 points***

Remonter jusqu'au photographe.. On a déjà vu qu'il se faisait surnommer **G.** Et si le Gary, Responsable Communication, de l'organigramme était ce fameux photographe ?

Pour en être sûr, on peut regarder les métadonnées d'une des trois photos des jeunes filles trouvées sur le lien tor, à l'aide de la commande ``èxiftool`` par exemple :

{{< image src="img_11.png" >}}

Notre photographe n'est autre que **Gary Durbourg**

{{< admonition success "Heavenly Creatures" false >}}
:triangular_flag_on_post: `Gary Durbourg`
{{< /admonition >}}

### Donnie Brasco

> Son pseudonyme ne vous aide pas beaucoup, mais il pourra vous servir à éventuellement trouver sa véritable identité !
>
> Format du flag : ``michel sardine``
>
> ***100 points***

Rien de plus simple qu'une petite recherche google ici :

{{< image src="img_12.png" >}}

On reconnait deux mots-clés qui nous parlent. Voici donc son profil [Behance](https://www.behance.net/garydurbourg13100?tracking_source=search_users|Gary%20Durbourg), dans lequel il dit dans sa bio

> À PROPOS DE MOI
>
> De mon vrai nom Gérard Dubourguin, je suis photographe de métier depuis plus de 20 ans.
> blabla

{{< admonition success "Donnie Brasco" false >}}
:triangular_flag_on_post: `Gérard Dubourguin`
{{< /admonition >}}

## Last Man Standing

### Tales from the Hood

> Comme toute secte, les Enfants d’Hades doivent avoir des détracteurs ! Sous quel pseudonyme pourrez-vous en retrouver un ?
>
> Format du flag : ``Jason Staham``
>
> ***40 points***

De retour sur le blog de **Kronos Titan**, la personne voulant dénoncer les actes de la secte.

Le voilà notre détracteur, le "Last Man Standing"

{{< admonition success "Tales from the Hood" false >}}
:triangular_flag_on_post: `Kronos Titan`
{{< /admonition >}}

### True Romance

> Vous commencez à en apprendre plus sur le fonctionnement de la secte, et sur comment Ellen a été recrutée et manipulée.
>
> Ils sont à priori bien organisés !
>
> Quel rôle occupe Marianne dans la secte ?
>
> Format du flag : ``surveillant``
>
> ***20 points***

Ellen en a parlé dans son journal, mais Kronos aussi : Marianne a joué le rôle de **marraine** pour Ellen

{{< admonition success "True Romance" false >}}
:triangular_flag_on_post: `marraine`
{{< /admonition >}}

### Touch of Evil

> Afin d’être le plus efficace possible, il vous faut continuer à obtenir le plus d’informations possible sur les Enfants d’Hades, et en particulier sur les évènements qui pourraient vous permettre de retrouver Ellen. Quel nom est donné aux soirées de cette secte ?
>
> Format du flag : ``Mariages de la planète bleue``
>
> ***20 points***

Permis les article de Kronos, [celui-là](https://kronosantihades.wordpress.com/2023/08/28/les-soirees-de-la-lune-noire-comment-les-enfants-dhades-se-financent-en-exploitant-leurs-jeunes-recrues/) parle de cette cérémonie sacrée où se retrouvent les membres : la **Céremonie de la lune noire**

{{< admonition success "Touch of Evil" false >}}
:triangular_flag_on_post: `Céremonie de la lune noire`
{{< /admonition >}}

### Basic Instinct

> Pour reprendre contact avec Ellen, il vous faudra essayer de savoir les étapes subies pendant son intégration dans la secte. Quelle est la troisième étape initiale dans le processus de recrutement des Enfants d'Hades ?
>
> format du flag : ``classification des vérités``
>
> ***40 points***

Toujours sur le blog de Kronos, [cet article](https://kronosantihades.wordpress.com/2023/08/28/le-piege-des-tenebres-comment-les-enfants-dhades-manipulent-et-recrutent-des-jeunes-femmes-en-difficulte/) nous explique la manière de recruter de la secte.

Entre deux paragraphes, Kronos donne le lien du [guide de recrutement](https://kronosantihades.wordpress.com/2023/08/28/le-piege-des-tenebres-comment-les-enfants-dhades-manipulent-et-recrutent-des-jeunes-femmes-en-difficulte/) des enfants, où sont détaillées les étapes.

Celle que nous recherchons s'appelle la **verification des antécédents**.

{{< admonition success "Basic Instinct" false >}}
:triangular_flag_on_post: `verification des antécédents`
{{< /admonition >}}

### The Glass Shield

> Bien entendu, l'idéal serait de pouvoir identifier cette source précieuse pour votre enquête. Il doit être possible de trouver plus d'informations sur lui, afin de trouver son prénom et le lien qu'il a avec l'actuel chef de la secte.
>
> format du flag : ``Hector oncle``
>
> ***100 points***

Honte de sa position, Kronos a supprimé un article de son blog où il se confessait par rapport à son lien intime à la secte.

On est en capacité de retrouver cet article grâce à [Web archive](https://web.archive.org/web/20230903085211/https://kronosantihades.wordpress.com/2023/08/24/la-veritable-origine-des-enfants-dhades-mes-confessions/#more-45).

Cet article nous apprend que Kronos s'appelle **Georges**, qu'il est le fondateur des Enfants d'Hades, mais que son **fils** a perverti son organisation, jusqu'à en prendre la tête et la transformer en secte malsaine et dangereuse.

{{< admonition success "The Glass Shield" false >}}
:triangular_flag_on_post: `Georges pere `
{{< /admonition >}}

## Swordfish

### Wargames

> Un membre de la secte possède surement son propre serveur informatique. Pourriez vous vous y connecter ?
>
> Format du flag : ``EDH{Ceci_Est_Un_Magnifique_Flag}``
>
> ***50 points***

Parmi les membres de la direction présent sur l'organigramme, si nous envoyions un mail au Responsable Evénement Zagreux (``zagreus@enfantsdhades.com``), ce dernier vous répond cela :

```
Il ne me reste plus beaucoup de temps...
Si tu souhaites vraiment de me contacter, connecte toi ici :

SSH zagreus@90.84.171.140
SRm5fowKzC9D6fw9

Je ne sais pas combien de temps ce serveur sera encore disponible.

Zagreus
```

On peut alors se connecter en SSH à ce serveur, ce qui donne un flot de caractères imbitable.

Aucune action n'est faisable, mais il nous donne le flag.

{{< admonition success "Wargames" false >}}
:triangular_flag_on_post: `EDH{CHilDren_Of_EasTeR_Egg}`
{{< /admonition >}}

---

#### *Ensuite, mon équipe et moi n'avons pas poursuivi nos recherches...*

