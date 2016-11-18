+++
tags = [
  "hugo",
]
categories = [
  "blog",
  "how-to-use",
]
date = "2016-11-17T18:04:28-05:00"
title = "Blogger avec hugo"

+++

Mon aventure de commencer à bloguer en utilisant Hugo et résumé des aspects importants et à ne pas oublier lorsqu'on l'utilise.

<!--more--> 

# Aperçue

Ce post est revu des différentes capacités de Hugo, je pense qu'il est utile pour savoir comment bien utiliser Hugo ainsi que 
des petits détails utiles (par exemple je ne savais pas que Hugo offrait une table des matières intégrés)


<!-- toc -->


# Début de l'aventure

Au moment où j'ai décidé de commencer à vouloir bloger, pour plusieurs raisons (voir autre post), j'ai d'abord du trouver une plateforme qui 
rapidement me permettrait de commencer à écrire. J'avais quelque critère:

* Compatible avec gh-pages: donc un site web statique (sans serveur)
* Facile à configurer et setup
* Plusieurs options de customization


Après quelque recherche je devais choisir entre Jykyll qui à un support officiel avec gh-pages et hugo qui semblait avoir le hype dans les voiles.
Puisque jekyll demandait ruby et j'étais tros paresseux pour setup cela, j'ai choisi hugo qui contient un executable: easy peasy!


Après plusieurs heures à regarder les différents thèmes possible: mon objectif pour le blog n'était pas d'être un spécialiste du css, je voulais 
profiter le travail d'un autre. Finalement j'ai du changer plusieurs fois: ce qui demande toujours quelque configuration... hurhghghg! 

Bref considérant cette partie volatile et suptile au changement je me suis mis en quête de comprendre les concepts de base de Hugo qui sont réutilisables
peu importe le thème. Alors voilà pourquoi j'écris ce post!

Tout d'abord, je compte sur le fait que vous avez au moins lue le [tutoriel officiel](https://gohugo.io/overview/quickstart/)




# Les bases

Ce qu'il faut savoir pour profiter à 80% des fonctionnalités de Hugo


## Organisation et sections

[Documentation officiel](https://gohugo.io/content/organization/)

Hugo ne supporte pas seulement le contenu de "post", il peut supporté d'autres types comme des quotes. Ces différents types ont le nom de sections et
on les retrouves au premier niveau sous le dossier content. L'organisation de notre contenu est donc très visuel ce qui est agréable. Bien sur tout 
est modifiable avec le [Front Matter](#front-matter)

    .
    └── content
        ├── post // section #1
        |   ├── firstpost.md   // <- http://[...].com/post/firstpost/
        |   ├── happy
        |   |   └── ness.md  // <- http://[...].com/post/happy/ness/
        |   └── secondpost.md  // <- http://[...].com/post/secondpost/
        └── quote // section #2
            ├── first.md       // <- http://[...].com/quote/first/
            └── second.md      // <- http://[...].com/quote/second/


### Types

> FMORR: Ok, c'est bien beau avoir des sections et différents types de contenu mais qu'est ce que ca change? 

Ayant des types on peut créer des [archetype](#archetype) et [templates](#template)

## Front Matter

> FMORR : Hummm, ca semble être le début d'une mauvaise blague. Dans tous les cas, ma position est que tout matter!!!

> PM: ... est ce que tu te trouves drôles FMORR, je pensais qu'avec l'âge viendrait la sagesse....

Bref de plaisanterie! Front Matter veut tout simplement dire qu'i lest possible d'ajouter du meta-data à notre contenu.

Il supporte différent format:
* TOML: identifié avec '+++' qui utilise des =
* YAML: identifié avec '---' qui utilise des :
* JSON: qui est tout simplement un object JSON


> FMORR: Ok! À quoi ca sert maintenant?

Les pairs variables-valeurs sont accesible dans les templates. Par exemple on voudrait que le titre du post soit écrit en taille 40 dans la page du post
et en taille 10 sur la page qui montres tous les posts disponibles

(todo: ajouter photo)

### Variables intéressantes

* title: Titre du contenu (duh!) 
* description: Description du contenu
* date: Date utilisé pour trier les posts
* taxonomies: [Voir prochaines sections](#taxonomies)
* draft: Si brouillon normalement on ne veut pas le rendre visible (à moins d'avoir des bas standards et que de seulement montrer de quoi qu'on est fier est pas dans nos valeurs)


### Résumé

Par défaut hugo, prend les 70 premiers mots du post et les mets dans la variable summary. Ce que je trouve donne souvent des résultats peu désirable: les premiers mots d'un texte
sont rarement le parfait résumé qu'on en ferais. Heureusement, il est possible de le spécifier et en plus il permet d'utiliser le formatage HTML!

Il suffit d'ajouter le résumé à la fin du Front Matter et lorsqu'on a finit de l'écrire on ajoute: `<!--more-->`

```md
---
title : "Legit post"
tags : ["serious", "potato", "pls take me au sérieux"] 
categories : ["demo"]
date : "2016-11-17" 
---

Résumé du post: bla bla bla bla potato bla bla bla bla

<!--more--> 

Reste du texte
[...]
```


## Archetype

> FMORR : Damn si ca se rapproche tant soit peu d'un archnemesis, hugo vient monter d'une coche en terme de coolitude

> PM: ... hélas non: le arche devant le mot type signifie seulement que c'est le modèle du type. Mais FMI la différence le mot arch devant ennemie provient du grec "arkhos" qui
> signifie "le plus important", et la différence entre un ennemie et un némésis et une référence à la déesse grec de la juste colère et de la rétribution céleste qui est utilisé
> par antonomase pour signifier la force vengeresse. On pourrait dire que plusieurs peuvent vaincre leur ennemie mais peu peuvent échapper leur nemesis! [^1]

Bref.... il permet de déclarer les variables de front matter par défaut pour un type de contenue. 

    .
    └── archetypes
        ├── default.md // Le modèle utilisé si pas de modèle plus spécialisé pour un type
        ├── post.md // Le modèle utilisé pour le type de contenu "post"


> FMORR: Comment utiliser ces valeurs par défaut?

Il suffit d'utiliser la commande new de hugo:

```Io
$ hugo new [type conteny]/[path]/my-new-content.md
```


# Les thèmes

Pour accélérer le démarage d'un blog il n'y a pas mieux que de partir d'un thème: il va tout styler ton site pour qu'il soit responsive, beau, ...

Un des plus grand problème est qu'il y a telement de [choix](http://themes.gohugo.io/)! Et que chacun demande une petite configuration initial.

## Pour utiliser un thème

> PM: Dans tousles showcase il le rapelle mais juste au cas où!

1. Télécharger le thème désiré

```Io
$ cd themes
$ git clone URL_TO_THEME
```

2. Indiquer à Hugo quel thème utilisé (puisqu'on peut en télécharger plusieurs)

* On peut utiliser la ligne de commande

```Io
$ hugo -t ThemeName
```

* Ou le set nous même dans le fichier config

```
theme: "ThemeName"
```


## Tweek le thème

Il m'est rapidement arrivé que je voulais tweeker un peu le thème. Il serait une mauvaise idée de le modifier directement dans le dossier thème puisque que s'il y a une mise
à jour nos modifications vont être effacé!

Le principe est donc de remplacer les templates, fichiers statiques ou archetypes à partir du répertoire de base (pas de theme). Si même nom de fichier dans même sous-arboresence
Hugo va d'abord sélectionner le notre.

Par exemple:

```
/layouts/_default/single.html
```

Va remplacer:

```
/themes/themename/layouts/_default/single.html
```


# Template

> PM : C'est les template qui permet de constuire simplement un site web complexe et riche en ressource.

Il existe 4 types important de template:
* Single: Pour visualiser un type de contenu
* List: Pour visualiser plusieurs morceau de contenus (ex: montrer tous les posts)
* Homepage: Code pour la Home Page du site (...)
* Partial: Permet de séparer des morceaux de code et les ré-utiliser à de multiples endroits


# Taxonomies

Pour faire simple, il s'agit une façon de classifié le contenu. Par exemples tags, categories, series sont des taxonomies.



# Les extras

Pour les utilisations plus avancés et les configurations plus poussés.

## Comments

## Shortcodes

## Table of contents


## Markdown

Le markdown à pour but de simplifier l'écriture d'un post: il permet de définir comment afficher l'information

Hugo utilise [Blackfriday](https://github.com/russross/blackfriday)



# Annexe


> PM: Rappel toi FMORR que la [documentation de hugo](https://gohugo.io/overview/introduction/) est lui même un blog hugo dont on peut regarder le [code source](https://github.com/spf13/hugo/tree/master/docs)


*[antonomase] : Figure de style qui consiste à utilisé un nom propre est utilisé comme un nom commun: par exemple un atlas provient en fait de Atlas un géant de la mythologie grecque qui devait porter le monde sur ses épaules
[^1]: [Difference ennemie, némésis et arch](http://www.dailywritingtips.com/on-the-use-of-nemesis/)