---
author: slowe
categories: Rant
comments: true
date: 2012-12-08T22:03:18Z
slug: why-a-cli-for-dropbox-on-mac-os-x
tags:
- Automation
- CLI
- macOS
- Storage
title: Why a CLI for Dropbox on Mac OS X
url: /2012/12/08/why-a-cli-for-dropbox-on-mac-os-x/
wordpress_id: 3003
---

I was thinking about a command-line interface (CLI) for [Dropbox](http://www.dropbox.com/), and how I personally would take advantage of such a feature. So, after failing to find any indication that the Mac OS X Dropbox client contained a CLI, tonight on Twitter I made [this comment](https://twitter.com/scott_lowe/status/277598393314971648):

>Too bad the #Mac version of @Dropbox doesn't have a CLI.

Shortly thereafter, I received [this response](https://twitter.com/markmeulemans/status/277599284399054848):

>@scott_lowe @Dropbox What would you do with it if you did?

I posted a response (which you can see if you follow either of the Twitter links above), but I realized that my response really needed a bit more background.

Like many people in IT today, I'm pretty mobile. I have a home office, but I also travel a fair amount. My laptop, a 2011 13" MacBook Pro with 8GB of RAM and a 512GB SSD, is my primary computer. The problem is this: when I'm in my home office, I want my laptop to be configured a certain way, but when I'm traveling, I need it configured a different way. For example, when I'm in my home office, I want Synergy running so that I can connect to [the Synergy server on my Mac Pro workstation][1]. When I'm not in the home office, Synergy should not be running. So how do I get the computer to automatically reconfigure itself? The answer is quite simple, actually: an app called [ControlPlane](http://www.controlplaneapp.com/).

ControlPlane is a handy little application that performs a set of actions based on a context. A context is defined as a set of conditions, like (as in my situation) being connected to my 24" Apple Cinema Display and being connected via Ethernet to a network using my home network's IP addressing scheme. If all those conditions are met, then it's quite likely I'm in my home office---meaning I'm in that particular context---and ControlPlane should perform a set of actions to reconfigure my laptop. Similarly, if those conditions aren't true, then it's quite likely I'm not in my home office---meaning I'm in a roaming or traveling context---and therefore my computer should be configured a different way. Handy, right?

To put some specifics on this idea, then, here's how I use ControlPlane:

* I have two contexts, one called Docked and one called Roaming. Docked is _only_ for when I'm connected to my 24" Apple Cinema Display in my actual home office, wired up via Ethernet (not wireless), and have an IP address off my home network's subnet. When those conditions are true, I'm "docked" and I need Synergy running so that I can share keyboard and mouse between my Mac Pro workstation and my laptop.

* Any other time, I'm not "docked" and should be in the Roaming context. In the Roaming context, Synergy should not be running.

* When I enter the Docked context, ControlPlane should launch Synergy, if it's not already running, and then issue a [Growl](http://growl.info) notification that the computer is entering the Docked context.

* When I leave the Docked context (meaning I'm entering the Roaming context), then ControlPlane should kill Synergy (if it's running), and post a Growl notification.

ControlPlane is capable of much, much more, but (for now) this is sufficient. At some point in the future, I might have it mount network drives (or maybe [my EncFS filesystem][2]).

I said all that to finally come back to the comment that started all this: if Dropbox had a CLI (or AppleScript support, but that's probably too much to ask for), then I could use ControlPlane to automate/manipulate the behavior of Dropbox as part of my contexts. For example, I could define another context---say, Disconnected---in which there are no active network interfaces. In that context, I'd like Dropbox to pause syncing. Then, when I enter another context, either Roaming or Docked, then Dropbox should continue syncing. However, without some sort of non-GUI access to Dropbox, this isn't possible (to my knowledge).

Anyway, that's what I was thinking. Courteous comments (or questions) are always invited and encouraged, so feel free to speak out below.

[1]: {{< relref "2012-04-30-updated-launchd-property-list-file-for-synergy.md" >}}
[2]: {{< relref "2012-11-23-using-encfs-with-dropbox-and-boxcryptor.md" >}}
