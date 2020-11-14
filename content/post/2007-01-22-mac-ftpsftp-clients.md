---
author: slowe
categories: Rant
comments: true
date: 2007-01-22T21:02:26Z
slug: mac-ftpsftp-clients
tags:
- Encryption
- FTP
- macOS
- SFTP
title: Mac FTP/SFTP Clients
url: /2007/01/22/mac-ftpsftp-clients/
wordpress_id: 404
---

I'd gotten turned on to [Cyberduck](http://cyberduck.ch/) as my primary FTP/SFTP client after really getting into [Growl](http://growl.info/), the global notification system for [Mac OS X](http://www.apple.com/macosx/). The application I was using at the time, [Fugu](http://rsug.itd.umich.edu/software/fugu/), didn't have Growl support. Cyberduck did, so I switched, and I've been using Cyberduck ever since.

I like the Cyberduck interface; it seems to make sense to me and I've never really run into any major compatibility issues (seems like I ran into one minor problem after an upgrade of [OpenSSH](http://www.openssh.org/) on one of my servers, but that problem was quickly resolved as I recall). The Growl support is, of course, excellent, and Cyberduck also offers a veritable laundry list of features---integrated support for Spotlight, a Dashboard widget, Keychain support, multiple windows, etc. It even comes as a Universal binary. (The features are far too many to list here; refer to the Cyberduck web site for complete information.)

Sound like a great application? It is---if you don't need to transfer large files. Since I started out just using Cyberduck to move some small web pages back and forth to my web server, these were mostly small files and I didn't really notice any performance hit. Sure, it seemed a bit slower than command-line SFTP or SCP and it seemed to be a bit of a memory hog, but I figured it was just GUI overhead and thought no more about it. For what I was doing at the time, it worked fine.

Recently, though, I've been needing to transfer much larger files to and from some SFTP servers on my local LAN. How large? ISO images ranging from 300MB to 600MB, sometimes multiple ISO images at a time. Generally, the file transfers will complete, but they are just plain slow. Almost painfully slow. So slow, in fact, that I've been driven to looking at alternatives.

I'm currently evaluating [Interarchy](http://www.interarchy.com/). While the interface is a bit quirky (although I suppose that is due to being predisposed to an interface like Cyberduck's), the performance is _astounding_. I can transfer multiple ISO images in minutes, not hours as with Cyberduck. It's almost unbelievable.

I have yet to decide whether I'll just buy Interarchy or if I'll evaluate two other potential candidates, [Transmit](http://www.panic.com/transmit/) and [Fetch](http://www.fetchsoftworks.com/). Both applications have gotten good reviews, but---being the UI stickler that I am---neither of them sports as modern a UI as Interarchy (I really like the unified toolbar look).

My primary complaint with Interarchy is the price. Sixty bucks seems a bit high for this type of application; both Transmit and Fetch (other options to replace Cyberduck) charge about half that. Of course, the other applications don't offer the same set of functionality that Interarchy offers, either. But will I actually use that functionality? Amazon S3 support is great, but will I really use Amazon S3? I don't have a WebDAV server, so is it worth paying for WebDAV support? Is it worth paying for network tools that duplicate functionality already in the base operating system?

What do you think? If you are a Fetch, Transmit, Interarchy, Fugu, or even Cyberduck user, please post in the comments and tell me what you think.
