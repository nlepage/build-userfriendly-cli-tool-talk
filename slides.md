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
Bien présenter les 2 axes du talk :
 - Faire un outil userfriendly
 - S'appuyer sur l'écosystème Go pour ça
-->

---
layout: fact
---

# ⚠ Disclaimer ⚠

<!--
Pas d'outil interactifs (éditeurs, REPL...)
-->

---
layout: full
class: bg-black
---

---

``` {all|1}
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
Pas intuitif.

Options à une lettre sans tiret, pas très conventionnel...

Mais alors justement, qu'est-ce qui est conventionnel ?
Qu'est-ce qu'on s'attend à avoir ?
-->

---
layout: section
---

# Options

---

# Options longues

``` {all|2|3|4|all}
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

---

# Options courtes

``` {all|1|3|all}
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

---
layout: fact
---

# Écosystème <img src="/Go-Logo_Blue.png" class="inline w-60" />

---
layout: statement
class: align-middle
---

<h1 class="flex gap-30 items-end justify-center">
  <img alt="cobra" src="/cobra.png" class="inline w-60" />
  <div>urfave/cli</div>
</h1>

---

# <img alt="cobra" src="/cobra.png" class="inline w-40" />

```go {all|1,2,10|1,3,10|1,4-6,10|1,7-10|12-17}
var rootCmd = &cobra.Command{
  Use:   "hugo",
  Short: "Hugo is a very fast static site generator",
  Long: `A Fast and Flexible Static Site Generator built with
                love by spf13 and friends in Go.
                Complete documentation is available at http://hugo.spf13.com`,
  Run: func(cmd *cobra.Command, args []string) {
    // Do Stuff Here
  },
}

func main() {
  if err := rootCmd.Execute(); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

<style>
  code {
    @apply text-sm
  }
</style>

---

# <img alt="cobra" src="/cobra.png" class="inline w-40" />

```go
var (
  author string
  verbose bool
)

func init() {
  rootCmd.Flags().StringVar(&author, "author", "YOUR NAME", "Author name for copyright attribution")
  rootCmd.Flags().BoolVarP(&verbose, "verbose", "v", false, "verbose output")
}
```

---

```
$ hugo --help
A Fast and Flexible Static Site Generator built with
love by spf13 and friends in Go.
Complete documentation is available at http://hugo.spf13.com

Usage:
  hugo [flags]

Flags:
      --author string   Author name for copyright attribution (default "YOUR NAME")
  -h, --help            help for hugo
  -v, --verbose         verbose output
```
