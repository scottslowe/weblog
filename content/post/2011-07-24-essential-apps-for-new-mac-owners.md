---
author: slowe
categories: Information
comments: true
date: 2011-07-24T10:16:46Z
slug: essential-apps-for-new-mac-owners
tags:
- Apple
- macOS
- Productivity
title: Essential Apps for New Mac Owners
url: /2011/07/24/essential-apps-for-new-mac-owners/
wordpress_id: 2356
---

A few of my colleagues are switching from Windows to Mac OS X thanks to the recent release of the new MacBook Air models. As a Mac user for more than 8 years now (since well before the Intel switch), I thought it might be handy to post a list of what could be considered some essential apps for new Mac users.

With that in mind, here goes...

## File Transfer: Cyberduck

On Windows, many people use [Filezilla](http://filezilla-project.org/) for their file transfer needs. Filezilla does have a Mac OS X version, but I've never used it; instead, new Mac users might prefer [Cyberduck](http://cyberduck.ch/), a free and open source file transfer application that supports FTP, SFTP, WebDAV, Amazon S3, Windows Azure, and Google Storage. It's not just an FTP client anymore! The nice thing about Cyberduck is that it leverages many of the features that drew you to Mac OS X in the first place: Spotlight, Quick Look, Bonjour, and Keychain.

Cyberduck is versatile, but for flat-out raw speed you'll want to have a look at [Interarchy](http://nolobe.com/interarchy/). It's not free, but it is powerful, and supports many of the same features as Cyberduck. [Transmit](http://www.panic.com/transmit/), from Panic, is another option. Interarchy is my tool of choice.

## Instant Messaging: Adium

Without a doubt, [Adium](http://adium.im/) is the king of the Mac OS X instant messaging world. With incredible protocol support (including Google Talk, Facebook Chat, MSN, AIM, MobileMe, Yahoo Messenger, ICQ, Bonjour/iChat, Twitter, IRC, MySpaceIM, Lotus Sametime, and Novell Groupwise), support for encryption (via [OTR](http://www.cypherpunks.ca/otr/)), integration with the Mac OS X Address Book, and a polished user interface, it's hard to beat. Oh, did I mention it's free and open source?

If you need integration into a corporate Microsoft Communicator-type environment, Microsoft has a Mac version of Communicator that will fill that need. For all other IM needs, use Adium.

## Diagramming: OmniGraffle Professional

Need to create network diagrams (or any type of diagram, really)? Try [OmniGraffle Professional](http://www.omnigroup.com/products/omnigraffle/). OmniGraffle Pro imports and exports Visio files (not just Visio drawings but also Visio stencils and templates), supports shared layers, custom data, and numerous other features. This is one app you'll want to evaluate if you have a need for building diagrams of any sort. It's not free and not open source, but still worth it in my opinion.

You can, of course, still run Microsoft Visio via a virtualization solution (see below).

## Application Firewall: Little Snitch

Just because Mac OS X hasn't yet seen as much malware and other stuff as other platforms doesn't mean it isn't coming. So be prepared: use an outbound application-level firewall like [Little Snitch](http://www.obdev.at/products/littlesnitch/index.html). Little Snitch will let you know about any outbound traffic that an application tries to initiate, and will let you approve or deny the traffic. Combine this with Mac OS X's built-in inbound application-level firewall (enabled in the Security section of System Preferences) and the BSD-level `ipfw` firewall (which you'll have to configure manually) and you've got the ability to keep network traffic into and out of your Mac locked up tight.

## Traditional Productivity: Microsoft Office

Apple's [iWork](http://www.apple.com/iwork/) suite (Pages, Numbers, and Keynote) are handy, but they haven't quite caught up to [Microsoft Office](http://www.microsoft.com/mac). If you need to exchange documents with other people using Microsoft Office (and let's face it, who doesn't?), this is your best choice (OpenOffice and LibreOffice come to mind). Is it the only choice? No, certainly not; there are numerous alternatives. But this choice saves you time sorting out conversion issues, giving more time to get real work done. Heads-up: I haven't upgraded to Lion yet, and I'm hearing that there are some potential compatibility issues between Office 2011 and Lion.

## Twitter Client: Twitterrific

While the 4._x_ branch of [Twitterrific](http://twitterrific.com/mac) dropped some features I personally considered essential---namely, tweet filtering support and AppleScript support---I still find it to be a great Twitter client. I also find it handy to use the same client on my Mac, my iPhone, and my iPad.

## Transition Support: VMware Fusion

Regardless of the new apps you might adopt, as a new Mac user you're bound to find things that you still need to do in Windows. For those times, [VMware Fusion](http://www.vmware.com/products/fusion/overview.html) is the way to go. Yes, there are other options (Parallels Desktop), but I've been using Fusion since the very earliest "Friends and Family" pre-beta releases in 2006 and have never experienced even the first problem with it.

## SSH Client: OpenSSH, built-in!

As a former Windows user, you had to download and install an SSH client (typically [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/)). No longer! Mac OS X comes with [OpenSSH](http://www.openssh.com/) preinstalled, and all you need to do is open up Terminal (found in Applications > Utilities), use the `ssh` command, and you're good to go.

There are plenty of other great apps that I use and support--[Unison](http://www.panic.com/unison/), [Colloquy](http://colloquy.info/), [Typinator](http://www.ergonis.com/products/typinator/), [Yojimbo](http://www.barebones.com/products/yojimbo/), [Handbrake](http://handbrake.fr/), [MarsEdit](http://www.red-sweater.com/marsedit/), [OmniFocus](http://www.omnigroup.com/products/omnifocus/), and [Skim](http://skim-app.sourceforge.net/), among others---but the seven applications listed above will certainly get you started.

Are there other apps that veteran Mac users would consider essential for a new Mac user? Please speak up in the comments!
