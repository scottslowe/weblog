---
author: slowe
categories: Tutorial
comments: true
date: 2011-12-02T11:28:32Z
slug: fixing-repeating-itunes-firewall-prompt
tags:
- macOS
- Security
title: Fixing Repeating iTunes Firewall Prompt
url: /2011/12/02/fixing-repeating-itunes-firewall-prompt/
wordpress_id: 2490
---

As part of a comprehensive security strategy for my Mac---I don't believe that Macs are automatically invulnerable to security threats---I use multiple host-based firewalls, including the Mac OS X application-level firewall found in System Preferences (this is in addition to [Little Snitch](http://www.obdev.at/products/littlesnitch/index.html) and `ipfw`). However, recently I started noticing that every time I launched iTunes, it would prompt me for whether or not I wanted to allow iTunes to accept incoming network connections.

It wasn't as much that iTunes was prompting me, as I knew why the application wanted to accept connections and had already taken steps at the BSD layer (using `ipfw` to limit connections on the appropriate ports). Rather, it was the fact that it kept prompting me, over and over. So, I did a bit of searching, and found the answer. I don't recall exactly where I found the answer, so I can't provide attribution here (sorry), but here's the information.

First, use the `codesign` utility (a handy little tool to verify code signatures) to see if iTunes has been modified/damaged/corrupted in some way:

    codesign -v --verbose /Applications/iTunes.app

If iTunes is fine, then you'll get a response like this:

    /Applications/iTunes.app: valid on disk
    /Applications/iTunes.app: satisfies its Designated Requirement

However, if you're experiencing the same problem that I was experiencing, you'll probably get a response different than the one above. Your message will say something to the effect that iTunes is invalid on disk and is missing a required component (or something along those lines; I didn't capture a screenshot with the exact message---sorry).

If that's the case, here's the fix:

1. Download the iTunes installation package from Apple again. You'll find it at [http://www.apple.com/itunes](http://www.apple.com/itunes) or similar.

2. Delete iTunes from the /Applications folder.

3. Reboot (it might be sufficient to just logout and log back in again).

4. Re-install iTunes.

After you've reinstalled iTunes, run `codesign` again to ensure that iTunes is reporting OK. Then, set the settings in the System Preferences Firewall GUI, and you should be good to go!
