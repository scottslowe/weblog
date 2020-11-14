---
author: slowe
categories: Information
comments: true
date: 2008-01-20T16:43:14Z
slug: leopard-upgrade
tags:
- Apple
- Blogging
- macOS
title: Leopard Upgrade
url: /2008/01/20/leopard-upgrade/
wordpress_id: 609
---

So I upgraded my laptop this past weekend to Mac OS X version 10.5, aka "Leopard". I've been reasonably pleased with the upgrade so far.

I keep most of my applications up to date, so I didn't have too many applications that weren't already Leopard compatible. That's an advantage of being a slightly later adopter as opposed to being one of those guys waiting in line when the new OS was released. In addition, I gain the benefit of the Mac OS X 10.5.1 update, which addressed a number of issues with the initial Leopard release.

So far I've only run into a couple of issues, both of them very minor:

* Mail.app 3.1 complains about the self-signed SSL certificate that my hosting provider uses with IMAP-TLS and SMTP-TLS. This occurred with Tiger as well, but some instructions I'd seen before the upgrade indicated that I might be able to bypass those warnings by setting the certificate to "Always Trust". This doesn't seem to work. Admittedly, a very minor issue.

* My blogging application, [ecto](http://infinite-sushi.com/software/ecto/), was supposedly not Leopard compatible with the version I was using (version 2.4.2, Intel build). (I left the older version installed side-by-side with the newer version and the old version seems to run fine, though.) So I switched to a beta build of ecto3, which is a complete rewrite of the blogging application, and I've run into a few little issues there. Those are directly related to the ecto3 upgrade, though, and not necessarily to the Leopard upgrade itself.

One of the first "tweaks" I reached for was the tweak to return the menu bar to a more opaque status. There are a number of sites out there providing instructions; here's the Terminal command I used:

	sudo defaults write /System/Library/LaunchDaemons/com.apple.WindowServer 'EnvironmentVariables' -dict 'CI_NO_BACKGROUND_IMAGE' 0.62

The command worked like a champ, and my menu bar was restored to some sense of normalcy. I initially also switched the Dock to a 2-D smoked glass look, but then switched back to the default 3-D appearance. I figured I'd give the new Dock appearance a chance before just banishing it to the ether.

I haven't been back to the office or at a customer's site since the upgrade, obviously, so I don't have any feedback yet on interoperability with Windows-based networks, Kerberos support, etc. I do need to look up the information on Leopard's built-in support for SSH keys, since I relied upon SSHKeyChain before the Leopard upgrade. If anyone has any pointers on that one, please let me know.

One _huge_ missing piece so far are the Leopard-compatible versions of MailTags and Mail Act-On. I have the beta versions of both, but I'm a bit hesitant to use them---I don't want to take any chances with my mail, if you know what I mean. Anyone out there using the beta versions of these on Leopard and have some feedback for me? Are they safe yet, or should I wait just a bit longer yet?

Spaces is pretty cool; it's nice to have virtual desktops back with Mac OS X again. I'd [used a pretty fair number][1] of virtual desktop applications on my Mac, eventually settling on VirtueDesktops (then just called Virtue) and then discontinuing my use of virtual desktops after my Tiger upgrade. VirtueDesktops went through various stages of support and non-support during the Tiger upgrade and the migration to the Intel platform, eventually [ending development][2] due to the expected introduction of Spaces. While Spaces doesn't have all the features that VirtueDesktops had, it is at least fully supported. In addition, the former developer of VirtueDesktops is working on something called [Hyperspaces](http://tonyarnold.com/entries/hyperspaces-dont-tell-anyone-ive-shown-you-this/), which will---as the name suggests---extend Spaces to include features that VirtueDesktops used to have. In any case, Spaces seems to work fine so far.

I'll post more information as I continue to get accustomed to Leopard; in the meantime, I'd love to hear any feedback from other Leopard users on your experiences. Feel free to put your feedback in the comments below!

[1]: {{< relref "2005-09-30-virtual-desktops-on-mac-os-x.md" >}}
[2]: {{< relref "2007-03-18-virtuedesktops-to-cease-development.md" >}}
