---
highlighter: shiki
info: |
  ## Construire un outil de CLI userfriendly

  <br>

  [Sources](https://github.com/nlepage/build-userfriendly-cli-tool-talk)
title: Construire un outil de CLI userfriendly
layout: intro
---

# Construire un outil de CLI userfriendly

## Inventons la **C**ommand **L**ine e**X**perience !

<div class="mt-10">
  <img src="/nicolas-lepage.jpg" class="rounded-full shadow-md inline w-25 mx-4" />
  Nicolas Lepage - <a href="https://twitter.com/njblepage">@njblepage</a>
</div>

<!--
- Consultant / Formateur Z Nantes
 - Principalement Dev fullstack JS
 - Go par passion sur projets OS

Durant cette présentation :
 - Comprendre/chercher pourquoi outil CLI est userfriendly ?
 - S'appuyer notamment sur des exemples existants, trouver bonnes/mauvaises pratiques
 - Fin, comme s'appuyer écosystème Go construire notre propre outil CLI
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
Pas juste noms 1 lettre

Options longues:
 - tjrs précédées 2 tirets
 - peuvent même plusieurs mots séparés tirets

Exemple grep, recherche :
 - ▶ ignorant casse
 - ▶ inversant le résultat (uniquement lignes ne correspondents pas)
 - ▶ compte (nombre lignes)

▶ nombre lignes ne contenant pas needle dans haystack.txt en ignorant la casse

Beaucoup plus clair, on comprend mieux ce qu'on fait

Si écrit script pour automatiser qqchose, bonne idée utiliser options longues...
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
 - précédés d'une seul tiret
 - 1 lettre

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

Options parlant, permet modifier comportement outil.

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
Afficher status dépot ▶

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

Image avec chats et phrases haut et/out bas image
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
<img src="/linux.svg" class="inline w-50">

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
Changer de chat
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

---
layout: section
---

# Configurable

---
layout: section
---

# Configurer mes dossiers d'images

---

# Configurer mes dossiers d'images

```sh {all|5|8-10}
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

---
src: black.md
---
---
layout: statement
---

# Un peu d'interactivité

---
layout: center
---

<img src="/catption-demo-3.gif" class="w-200">

---

FIXME ATTENTION vérifier que l'entrée et la sortie standard sont liées à un terminal

---

# Quoi d'autre ?

<v:clicks>

 - Conseiller / guider l'utilisateur
 - Logs sympas (couleur, émojis, ...)
 - FIXME rappelle toi

</v:clicks>

---

FIXME Ecosystème golang

---

FIXME conclusion mon ps aux
