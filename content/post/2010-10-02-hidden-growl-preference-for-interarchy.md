---
author: slowe
categories: Information
comments: true
date: 2010-10-02T22:54:49Z
slug: hidden-growl-preference-for-interarchy
tags:
- macOS
- FTP
title: Hidden Growl Preference for Interarchy
url: /2010/10/02/hidden-growl-preference-for-interarchy/
wordpress_id: 2129
---

[Interarchy](http://nolobe.com/interarchy/) is a well-known FTP/SFTP client for Mac OS X that I've used off and on for a few years. It's been mostly off in recent times as I tended to use [Cyberduck](http://cyberduck.ch/), but since Cyberduck's AppleScript support got whacked in a recent update I've been re-evaluating Interarchy again.

This hidden preference for Interarchy is one that I actually found out about from the Interarchy developer quite some time ago, but apparently never published anything about it. Interarchy has extensive support for [Growl](http://growl.info/), the ubiquitous Mac OS X notification system, but is configured by default to only post Growl notifications when in the background. To force Interarchy to post Growl notifications even when in the foreground, use this Terminal command to set a hidden preference:

	defaults write com.nolobe.interarchy PostGrowlNotificationsInForeground -bool TRUE

Once you run that command, Interarchy will post Growl notifications whether in the foreground or the background, and you can customize which notifications you'd like to see from the Growl preference pane in System Preferences, as you'd expect.
