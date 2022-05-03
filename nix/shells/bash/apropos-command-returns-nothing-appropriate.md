---
title: "Error: man -k (apropos) returns 'nothing appropriate'"
keywords: Bash Commands, Linux
summary: "Why you're getting this error, and how to fix it"
sidebar: nix_bash_sidebar
permalink: /bash-man-k-or-apropos-returns-nothing-appropriate
folder: docker
tags: [Bash Commands, Linux]
---

Because it is virtually impossible to remember every single Bash command, the `man -k` or `apropos` command serves as one of the most useful commands available.

You may already be aware that `man -k` is equivalent to `apropos`; therefore, I will be using `apropos` throughout the remainder of this demonstration.

In the example below, I'm using `apropos` to look up commands related to networking, but I get an error returned to `stdout` below.

```code
$ apropos network
network: nothing appropriate.
```

It is likely that the reason this is occurring is the `man` database caches on the system may not be created, initialized, or up-to-date. So, to remediate the problem that's causing this error, I elevate my permissions as required and run the following command:

```code
$ sudo mandb
Processing manual pages under /usr/share/man...
...
```

After completion, I run the `apropos` command again:

```code
$ apropos network

ctstat (8)           - unified linux network statistics
...
```

And, as you can see, now it works.
