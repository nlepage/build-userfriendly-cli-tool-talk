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

---
layout: fact
---

# ⚠ Disclaimer ⚠

---
layout: full
class: bg-black
---


---
---

```sh {all|1}
$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 165992 11536 ?        Ss   09:58   0:01 /sbin/init
root           2  0.0  0.0      0     0 ?        S    09:58   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   09:58   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   09:58   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   09:58   0:00 [kworker/0:0H-events_highpri]
root           8  0.0  0.0      0     0 ?        I<   09:58   0:00 [mm_percpu_wq]
...
```

<v-clicks>

**a** : Les process de tout le monde

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
