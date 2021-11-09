---
highlighter: shiki
info: |
  ## Construire un outil de CLI userfriendly

  <br>

  [Sources](https://github.com/nlepage/build-userfriendly-cli-tool-talk)
title: Construire un outil de CLI userfriendly
routerMode: hash
download: true
layout: intro
---

# Construire un outil de CLI userfriendly

## Inventons la **C**ommand **L**ine e**X**perience !

<div class="mt-10">
  <img src="/nicolas-lepage.jpg" class="rounded-full shadow-md inline w-25 mx-4" />
  Nicolas Lepage - <a href="https://twitter.com/njblepage">@njblepage</a>
  <img src="/osxp.png" class="inline w-30 ml-16" />
  <img src="/zenika.png" class="inline w-30 ml-16" />
</div>

<!--
 - Merci orga, merci à vous
 - Consultant / Formateur Z Nantes
 - Principalement Dev fullstack JS
 - Go par passion sur projets OS

Durant cette présentation :
 - Comprendre/chercher pourquoi outil CLI est userfriendly ?
 - S'appuyer notamment sur des exemples existants, trouver bonnes/mauvaises pratiques
 - Trouver d'autres idées pour être plus userfriendly
-->

---
layout: fact
---

# ⚠ Disclaimer ⚠

<!--
Ne seront pas abordés outil hautement interactifs (éditeurs, REPL...)

Domaine à part entière.
-->

---
layout: full
class: bg-black
---

<!--
Premières expériences ligne commande, Développeur Java grosse DSI...
-->

---

```sh {all|1}
$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 165992 11536 ?        Ss   12:00   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    12:00   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   12:00   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   12:00   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   12:00   0:00 [kworker/0:0H-events_highpri]
root           8  0.0  0.0      0     0 ?        I<   12:00   0:00 [mm_percpu_wq]
...
```

<v-clicks>

**a** : Les process de tout les utilisateurs

**x** : Même les process sans tty

**u** : Format orienté utilisateur
 
</v-clicks>

<style>
  code {
    @apply text-sm
  }

  p:first-of-type {
    @apply mt-18
  }

  p {
    @apply text-center text-2xl
  }
</style>

<!--
Jusqu'aujourd'hui (talk) continuer utiliser petite commande ▶ sans savoir ce que signifiait 3 lettres aux

Moins puisse dire, pas intuitif, difficile deviner.

▶ a ▶ u ▶ x

Options à une lettre sans tiret, pas très conventionnel... (selon doc options sauce BSD)

Bonne manière outil moins déroutant plus intuitif ▶
-->

---
layout: section
---

# Conventions

<!--
utilisées par les outils les plus répandus
-->

---
layout: section
---

# Options

<!--
donc conventions niveau options ligne commande...

options: flags passe aux outils pour modifier leur comportement

Déjà préférable d'avoir systématiquement options longues ▶
-->

---

# Options longues

```sh {all|2|3|4|all}
$ grep \
    --ignore-case \
    --invert-match \
    --count \
    needle haystack.txt
21
```

<style>
  code {
    @apply text-lg
  }
</style>

<!--
- mots précédés 2 tirets
 - peuvent même plusieurs mots séparés tirets

Exemple grep, recherche :
 - ▶ ignorant casse
 - ▶ inversant le résultat (uniquement lignes ne correspondents pas)
 - ▶ compte (nombre lignes)

▶ nombre lignes ne contenant pas needle dans haystack.txt en ignorant la casse

Beaucoup plus clair, on comprend mieux ce qu'on fait (bien pour script, automatisation)
-->

---

# Options courtes

```sh {all|1|3}
$ grep -i -v -c needle haystack.txt
21
$ grep -ivc needle haystack.txt
21
```

<style>
  code {
    @apply text-lg
  }
</style>

<!--
Options courtes correspondant aux options longues

Pas obligatoire... options les plus utilisées

Option courte:
 - 1 lettre précédée d'une seul tiret

▶ Simplement remplacer options longues (pas entrain d'écrire script, simplement dans terminal, pour aller plus vite)

▶ Possible de grouper derrière 1 seul tiret

Dernière convention
-->

---

# Options usuelles

```
$ grep --version
grep (GNU grep) 3.6
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Mike Haertel and others; see
<https://git.sv.gnu.org/cgit/grep.git/tree/AUTHORS>.
```

<style>
  code {
    @apply text-sm
  }
</style>

---

# Options usuelles

```
$ grep --help
Usage: grep [OPTION]... PATTERNS [FILE]...
Search for PATTERNS in each FILE.
Example: grep -i 'hello world' menu.h main.c
PATTERNS can contain multiple patterns separated by newlines.

Pattern selection and interpretation:
  -E, --extended-regexp     PATTERNS are extended regular expressions
  -F, --fixed-strings       PATTERNS are strings
...
```

<style>
  code {
    @apply text-sm
  }
</style>

<!--
Avec en général variant courte -h

Quand même quelques critères à respecter...

Garantit d'avoir outils auront mérite pas dérouter utilisateurs.

Options permettent modifier comportement outil.

Piège à éviter, exemple ▶
-->

---
layout: fact
---

# <img alt="pacman" src="/pacman.png" class="inline w-30" /> pacman

<!--
Gestionnaire paquet arch linux...

A la particularité de ▶
-->

---

# <img alt="pacman" src="/pacman.png" class="inline w-10" /> pacman

```sh {all|2|10}
$ pacman -h
usage:  pacman <operation> [...]
operations:
    pacman {-h --help}
    pacman {-V --version}
    pacman {-D --database} <options> <package(s)>
    pacman {-F --files}    [options] [file(s)]
    pacman {-Q --query}    [options] [package(s)]
    pacman {-R --remove}   [options] <package(s)>
    pacman {-S --sync}     [options] [package(s)]
    pacman {-T --deptest}  [options] [package(s)]
    pacman {-U --upgrade}  [options] <file(s)>

use 'pacman {-h --help}' with an operation for available options
```

<!--
Utilise flags (préfixé par - --) comme  ▶ opérations...

Exemple installer package ▶ -S ou --sync

Problème : distinction entre opération et option

Uniquement par casse flag court

Distinction disparait flag long

Porte à confusion

Normalement flags pas obligatoire, ici opération une et une seule...

Pour outil capable faire différente chose, bonne manière d'éviter ça, faire comme ▶
-->

---
layout: fact
---

# <img alt="git" src="/git.png" class="inline w-50" />

<!--
présente plus...

git a système de ▶
-->

---

# Commandes <img alt="git" src="/git.png" class="inline w-20" />

```sh {all|1|10}
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
	modified:   slides.md

no changes added to commit
$
$ git commit -am "Add slides about git"
[main 536e1f3] Add slides about git
 1 file changed, 25 insertions(+), 2 deletions(-)
```

<style>
  code {
    @apply text-sm
  }
</style>

<!--
Afficher état dépot ▶

Créer nouveau commit ▶

Options -am...

Distinction claire commande, options donne infos à commande (pas obligatoire)

Peut pousser plus loin comme docker, groupe de commandes ▶
-->

---

# Groupes de commandes

```
$ docker image

Usage:  docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.
```

<!--
docker image: pas une commande mais groupe

Peut aller plus loin, google cloud, gcloud hiérarchie groupes commande

a parlé de git, ses commandes, status, commit, git aussi défauts...
-->

---
layout: statement
---

# git checkout ?

<style>
  code {
    @apply text-5xl
  }
</style>

---
layout: statement
---

## Switch branches or restore working tree files

<!--
Problème: fait 2 choses

Nom pas clair (sans parler confusion svn checkout)

Pattern à éviter, commande fourre-tout

Commande:
 - nom clair : permet comprendre ce qu'elle fait
 - but précis : sinon à découper

a été corrigé...

Vous avais pas dit, moi même... s'apelle ▶
-->

---
layout: fact
---

# 🐱 catption

<!--
Permet créer rapidement meme cats

Image avec chats et phrases haut et/ou bas image
-->

---

# 🐱 catption

```
$ catption -h
Cat caption generator tool

Usage:
  catption [flags] <input file>

Flags:
  -b, --bottom string    Bottom text
      --fontSize float   Font in points (default 96)
  -h, --help             help for catption
      --margin float     Top/bottom text margin (default 20)
  -o, --out string       Output file (default "out.jpg")
  -s, --size float       Output image size (default 1024)
  -t, --top string       Top text
  -v, --version          version for catption
```

<style>
  code {
    @apply text-sm
  }
</style>

<!--
Bien respecter avait dit...

Options longues, options courtes, pas de commande (petit outil, ne sait faire qu'une chose)

le voici en action  ▶
-->

---
layout: center
---

<img src="/catption-demo.gif" class="w-200">

<!--
...

Comment aller plus loin ? Plus sympa, plus facile à utiliser ?
-->

---
layout: section
---

# Intégration avec l'OS

<img src="/windows.svg" class="inline w-50">
<img src="/linux.svg" class="inline w-40">
<img src="/pomme.png" class="inline w-30">

<!--
OS ont beaucoup fonctionnalités

Intéressant s'appuyer dessus.

Outil sorte cadre ligne commande.

Pour catption par exemple...
-->

---
layout: section
---

# Ouvrir le fichier généré

<!--
Plus avoir à taper commande en plus
-->

---

# Ouvrir le fichier généré

 - Windows : start
 - macOS : open
 - Linux : xdg-open

<style>
  ul {
    @apply text-2xl
  }
</style>

<!--
exemple windows commande start

prend chemin fichier, ouvre avec programme par défaut prévu pour type fichier

équivalents...

fonctionne pour tout type d'URL (exemple HTTP)
-->

---
layout: center
---

<img src="/catption-demo-2.gif" class="w-200">

<!--
Déjà plus sympa

Utilise ma commande, vois tout suite résultat

Par contre pas perdre vue, outil ligne commande aussi utilisé automatiser choses, écrire scripts

Important pouvoir désactiver ce type fonctionnalité
-->

---

# (Ne pas) Ouvrir le fichier généré

```sh {all|12}
$ catption -h
Cat caption generator CLI

Usage:
  catption [flags] <input file>

Flags:
  -b, --bottom string    Bottom text
      --fontSize float   Font in points (default 96)
  -h, --help             help for catption
      --margin float     Top/bottom text margin (default 20)
      --open             Open file with system viewer (default true)
  -o, --out string       Output file (default "out.jpg")
  -s, --size float       Output image size (default 1024)
  -t, --top string       Top text
  -v, --version          version for catption
```

<style>
  code {
    @apply text-sm
  }
</style>

<!--
pouvoir ne pas ouvrir...

moyen simple  ▶ ajouter option

autre fonctionnalité assez similaire: intégration presse-papier

pour aller plus loin  ▶
-->

---
layout: section
---

# Configurable

<!--
par exemple comme git (nom, email, ssh), commande git config

Rentre aussi un peu catégorie intégration OS, stocker config endroit spécifique selon système (exemple linux)

pour catption  ▶
-->

---
layout: section
---

# Configurer mes dossiers d'images de chats

<!--
Pas envie avoir à écrire fichier config à la main...

Nouveau groupe commande dir
-->

---

# Configurer mes dossiers d'images de chats

```sh {all|8-10}
$ catption help dir
Manages input files directories

Usage:
  catption dir [command]

Available Commands:
  add         Adds an input files directory
  list        Lists input files directories
  remove      Removes an input files directory

Flags:
  -h, --help   help for dir

Use "catption dir [command] --help" for more information about a command.
```

<style>
  code {
    @apply text-sm
  }
</style>

<!--
▶ 3 petites commandes permettent configurer mes répertoires chats...

top: plus à mettre chemin complet fichier, juste nom, catption va trouver...

Autre manière aider utilisateur, cette fois restant plus cadre ligne commande...
-->

---
layout: statement
---

# Un peu d'interactivité

<!--
pas faire outil interactif

mais un peu interactivité à petite dose dans cas bien spécifiques

exemple catption, appelle commande avec juste nom fichier, me suis tromper, oublier options

dans ce cas, propose saisir texte
-->

---
layout: center
---

<img src="/catption-demo-3.gif" class="w-200">

<!--
Attention, encore fois, pas perdre vue outil ligne commande sert automatisation...

Pour éviter problème, vérifier que entrée/sortie standard sont attachées terminal, si non pas d'interactif

arrive bientôt fin...
-->

---

# Idées en plus

<v-clicks>

 - Conseiller / guider l'utilisateur
 - Autocomplétion personalisée
 - Logs sympas (couleur, émojis, ...)

</v-clicks>

<style>
  ul {
    @apply text-2xl
  }
</style>

<!--
▶ conseiller guider

git fait très bien, notamment git status, en fonction état dépôt git propose commandes...

aussi did you mean...

▶ autocomplétion personnalisée

▶ logs sympas

là aussi qqchose à déclencher si sortie standard attachée terminal

si non plutôt logs destinés fichier log, donc sans couleur, format interprétable par autre programme
-->

---

# 🐱 [github.com/nlepage/catption](https://github.com/nlepage/catption)

## <a class="!border-none" href="https://github.com/spf13/cobra#readme"><img alt="cobra" src="/cobra.png" class="w-40 inline mb-4"></a><a href="https://github.com/spf13/viper#readme" class="!border-none"><img alt="viper" src="/viper.png" class="w-40 inline ml-10"></a>

## [github.com/mattn/go-isatty](https://github.com/mattn/go-isatty)

## [Packages os et os/exec de la bibliothèque standard](https://pkg.go.dev/os)

<style>
  h1 {
    @apply mb-20
  }

  h2 {
    @apply mb-10
  }
</style>

---

# Autres langages

 - NodeJS : [commander](https://www.npmjs.com/package/commander)
 - Java : [picocli](https://picocli.info/)
 - Python : [click](https://click.palletsprojects.com/en/latest/)
 - Et bien d'autres

<style>
  ul {
    @apply text-2xl
  }
</style>

---

# Références

 - https://man.archlinux.org/man/ps.1.en
 - https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html
 - https://man.archlinux.org/man/grep.1.en
 - https://man.archlinux.org/man/pacman.8.en
 - https://git-scm.com/docs
 - https://docs.docker.com/engine/reference/commandline/image/
 - https://www.freedesktop.org/wiki/Software/xdg-utils/

---
layout: center
---

## Slides : [nlepage.github.io/build-userfriendly-cli-tool-talk](https://nlepage.github.io/build-userfriendly-cli-tool-talk/)

<!--
Pour conclure vous donner mon ps aux idéal

process list --all-users --filter java
-->
