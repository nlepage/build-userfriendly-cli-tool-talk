## Abstract

La ligne de commande c'est austère... et on est nombreux à préférer l'éviter si possible.<br>
Cependant de nombreux outils ne sont disponibles que par ce biais, et on se sent parfois bien démuni quand on veut les utiliser... **mais ça ne devrait pas être le cas !**

Le design pour un outil de ligne de commande, comme pour toute application, peut changer radicalement l'expérience utilisateur en bien ou en mal.<br>
Mais qu'est-ce qui fait **un bon outil de CLI** : simplicité ? respect des conventions ? intuitivité ? personnalisation ? intégration avec l'environnement ? auto-complétion ? ~une interface graphique ?~

**Inventons la Command Line eXperience !**

L'éco-système Go autour des CLIs est très développé, voyons comment en tirer partie pour construire rapidement **un outil facile et agréable à utiliser**.

## Infos

Ce talk s'articulera autour de deux axes principaux :
1. Au travers d'exemples très connus (ps, git, docker...), mettre en avant ce qui fait qu'un outil de ligne de commande peut être simple à utiliser sans même avoir lu le manuel (ou bien l'inverse) !
2. Comment s'appuyer sur l'écosystème Golang pour construire un outil de ligne de commande userfriendly.

Le plan du talk n'est pas encore vraiment défini, mais voici quelques exemples :
 - Options vs sous-commandes
 - Commande fourre-tout vs commande avec un but clair et précis
 - Intégration avec l'OS et ses fonctionnalités

Les outils interactifs tels que les éditeurs de textes ou REPL ne seront pas abordés dans ce talk, sinon le sujet serait trop vaste...

Le public visé :
 - En particulier toute personne développant, ayant déjà ou souhaitant développer un outil de CLI
 - Mais aussi toute personne utilisant au moins occasionnellement la ligne de commande

## Plan

 - Intro
 - Disclaimer: pas d'outil interactifs (éditeurs, REPL...)
 - Intro suite 
 - Respecter les conventions (options, help, etc)
 - Options vs sous-commandes
 - Commandes fourre-tout
 - Introduction catption ?
 - xdg-open (fichier, ou URL)...
 - config (répertoires...)
 - advices (fichier non trouvé, exemple ajout répertoire)
 - interactivité (saisie du bottom/top ?)
 - logs sympa (émojis, couleur...)

## Idées

 - Personnalisation: Enregistrement de config (git config), viper ou autre, plus stdlib
 - Arguments positionnelles, ln
 - Utiliser catption en fil rouge ? Plutôt à la fin
 - Intégration avec système: xdg-open etc., attention à prévoir une option pour désactiver
 - Guider l'utilisateur, git status
 - Commande d'installation d'autocomplétion
 - Coloration emojis
 - Un peu d'interactivité, isatty
 - Analytics de la CLI ?

## Références

https://www.gresearch.co.uk/article/in-praise-of-dry-run/

### Intro suite

`ps aux`

a = lift user only filter
u = user oriented format
x = lift tty filter

BSD-style...

Pas intuitif, pas très conventionnelle (option à une lettre sans tiret)

### Respecter les conventions

Conventions des options:
 - nom long avec deux tirets (explicite, rapide à comprendre)
 - nom court avec un tiret et une lettre, en majuscule pour inverser

S'appuyer sur une librairie faite pour ça:
 - Simplement décrire les options
 - Valider
 - Documenter
 - Voire même auto-compléter
 - Ou bien du "did you mean?"

Cobra ou autre...

Respecter les options connues -h -f -r -v

### Options vs sous-commandes

Enchaîner sur --help et --version, qui sont en faites des sous-commandes...

Commande permettant d'effectuer différentes opérations.

git, docker, apt...

Un mauvais exemple: pacman...

Permet:
 - D'avoir des options spécifiques aux sous-commandes (plus de sens)
 - Une documentation plus structurée

Bonus:
 - Avoir une commande par défaut ou simplement afficher l'aide
 - Sous-sous-commande docker container et docker image

### Commandes fourre-tout

À quoi sert "git checkout" ?

(On ne parle même pas de la confusion avec svn checkout...)

> git-checkout - Switch branches or restore working tree files

Éviter les commandes fourre-tout : contre-intuitif, pas sympa pour les débutants

Préférer diviser la commande: git switch et git restore

Parlent d'elles mêmes, ont un but clair et précis.

