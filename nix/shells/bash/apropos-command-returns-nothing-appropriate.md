---
title: "Error: man -k (apropos) returns 'nothing appropriate'"
authors: Shawn Hoffman
editors: 
summary: "You don't remember the name of a Bash command you need, so you use man -k or apropos to search for it by keyword. Instead of the output you expected, you get the error: 'nothing appropriate'. Here's why this error usually occurs, and how you can fix it."
tags: [Bash Commands]
keywords: Bash Commands
references: https://www.mankier.com/1/apropos.mandoc, https://www.mankier.com/8/mandb
permalink: /bash-man-k-or-apropos-returns-nothing-appropriate

sidebar: nix_bash_sidebar
folder: docker
---

Because it is virtually impossible to remember every single Bash command, the `man -k` or `apropos` command serves as one of the most useful commands available.

You may already be aware that `man -k` is equivalent to `apropos`; therefore, I will be using `apropos` throughout the remainder of this demonstration.

In the example below, I'm using `apropos` to look up commands related to networking, but I get an error returned to `stdout` below.

{% highlight shell linenos %}
$ apropos network
network: nothing appropriate.
{% endhighlight %}

It is likely that the reason this is occurring is the `man` database caches on the system may not be created, initialized, or up-to-date. So, to remediate the problem that's causing this error, I elevate my permissions as required and run the following command:

{% highlight shell linenos %}
$ sudo mandb
Processing manual pages under /usr/share/man...
...
{% endhighlight %}

I could also throw in the `-c` option to create the database caches from scratch, rather than updating them.

After completion, I run the `apropos network` command again and get the resulting output:

{% highlight output linenos %}
ctstat (8)           - unified linux network statistics
dhclient-script (8)  - DHCP client network configuration script
ethtool (8)          - query or control network driver and hardware settings
ifenslave (8)        - Attach and detach slave network devices to a bonding device.
ifstat (8)           - handy utility to read network interface statistics
...
{% endhighlight %}

And, as you can see, it now works.

Furthermore, for ease of administration it may be a good idea to have `mandb` running periodically off of a cron job depending on your installation.
