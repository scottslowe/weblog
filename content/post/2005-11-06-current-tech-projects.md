---
author: slowe
categories: Information
comments: false
date: 2005-11-06T14:19:15Z
slug: current-tech-projects
tags:
- BSD
- Exchange
- Linux
- Newsgroups
- Postfix
- SpamAssassin
- Collaboration
- Messaging
title: Current Tech Projects
url: /2005/11/06/current-tech-projects/
wordpress_id: 113
---

Every now and then, I like to post out here a list of my current "tech projects." These are the things that I'm working on for my own network, things that I may or may not start recommending to or supporting for customers.

Here's my current list:

* _InterNetNews (INN):_ I had an installation of INN up and running a short while back, but had to resort to an ugly hack with `stunnel` in order to make SSL work from a newsreader. To get a clean build, I've decided I'll just start from scratch with a clean installation. I'll be using [CentOS](http://www.centos.org/) 4.1 again as I work on transitioning all my Linux-based servers to a newer Linux distribution, and I'll be compiling INN from source instead of using a package.

* _[OpenBSD](http://www.openbsd.org/)-based antispam gateway:_ I've got an antispam gateway running right now (uses Red Hat Linux, [Postfix](http://www.postfix.org/) 2.1, SpamAssassin, Postgrey, Razor, DCC, and ClamAV), but I want to try building one using OpenBSD 3.8 (just recently released) and newer builds of Postfix, SpamAssassin, and Amavisd-New. In particular, I'm interested in the advanced integration of newer versions of Postfix and Amavisd-New.

* _XC Connect:_ I've also mentioned XC Connect before as well, but a previous installation proved to be unstable, and the Apache integration was less than stellar. In fact, the integration was nonexistent. I'm going to try a clean build of CentOS 4.1 and XC Connect to see if that will correct the stability and integration problems.

I also need to wrap up the documentation for a few completed items, such as the Cisco VPN integration with Active Directory. Mac OS X integration with Active Directory is also on the "to do" list, but it will have to wait a little while---I'll need to find another Mac to experiment with instead of using my own PowerBook.
