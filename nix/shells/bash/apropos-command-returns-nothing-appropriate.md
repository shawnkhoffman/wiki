---
title: "Error: man -k (apropos) returns 'nothing appropriate'"
keywords: Bash Commands
summary: "You don't remember the name of a Bash command you need, so you use man -k or apropos to search for it by keyword. Instead of the output you expected, you get the error: 'nothing appropriate'. Here's why this error usually occurs, and how you can fix it."
sidebar: nix_bash_sidebar
permalink: /bash-man-k-or-apropos-returns-nothing-appropriate
folder: docker
tags: [Bash Commands]
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

I could also throw in the `-c` option to create the database caches from scratch, rather than updating them.

After completion, I run the `apropos` command again:

```code
$ apropos network

ctstat (8)           - unified linux network statistics
dhclient-script (8)  - DHCP client network configuration script
ethtool (8)          - query or control network driver and hardware settings
ifenslave (8)        - Attach and detach slave network devices to a bonding device.
ifstat (8)           - handy utility to read network interface statistics
...
```

And, as you can see, now it works.

---

Authored by: Shawn Hoffman

<br>

**References:**
- [ManKier. apropos(1)](https://www.mankier.com/1/apropos.mandoc)
- [ManKier. mandb(8)](https://www.mankier.com/8/mandb)