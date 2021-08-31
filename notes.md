## Plan

 - Intro
 - Disclaimer: pas d'outil interactifs (éditeurs, REPL...)
 - Intro suite 
 - Respecter les conventions
 - Options vs sous-commandes
 - Commandes fourre-tout

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

