---
author: slowe
categories: Explanation
comments: false
date: 2005-12-01T21:52:39Z
slug: updating-kubuntu-to-kde-35
tags:
- KDE
- Linux
- OSS
- Ubuntu
title: Updating Kubuntu to KDE 3.5
url: /2005/12/01/updating-kubuntu-to-kde-35/
wordpress_id: 132
---

[Kubuntu](http://kubuntu.org/) 5.10, the [KDE](http://www.kde.org/)-based variant of [Ubuntu](http://www.ubuntulinux.org/), normally comes with KDE 3.4.3. With KDE 3.5.0 just recently released, however, I wanted to update Kubuntu to use the latest version of KDE. Here's what I had to do to make it work.

First, from [this page](http://kubuntu.org/announcements/kde-35.php), I downloaded and added the appropriate GPG key. Once that was done, I used `apt-get update` to download the latest package information and `apt-get upgrade` to upgrade packages. That upgraded most of KDE, but there were a few packages that still refused to upgrade (they were reported by apt-get as "held back").

After poking around with apt-get for a while, I discovered that there was one missing file---libgnokii2---that couldn't be found in the apt-get sources. I manually reviewed the apt-get sources and finally found this package in the universe section of the main Ubuntu apt-get source. I edited `/etc/apt/sources.list` to enable the other sections, then manually installed the missing file with these commands:

    apt-get clean  
    apt-get update  
    apt-get install libgnokii2

I then edited `/etc/apt/sources.list` again to disable the other sources and upgraded the remainder of the KDE packages:

    apt-get clean  
    apt-get update  
    apt-get upgrade

This last round of `apt-get` removed a few obsolete packages and then updated the remainder of the KDE packages.

I don't know if this is the "correct" way of doing things, but it seemed to be the only way to make it work as anticipated. If anyone has a better way, please let me know.
