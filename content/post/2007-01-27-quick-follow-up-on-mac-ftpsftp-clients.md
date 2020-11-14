---
author: slowe
categories: Review
comments: true
date: 2007-01-27T10:56:01Z
slug: quick-follow-up-on-mac-ftpsftp-clients
tags:
- ActiveDirectory
- FTP
- Kerberos
- macOS
- Networking
- SFTP
title: Quick Follow Up on Mac FTP/SFTP Clients
url: /2007/01/27/quick-follow-up-on-mac-ftpsftp-clients/
wordpress_id: 407
---

Unable to cope with the slow performance of [Cyberduck](http://cyberduck.ch/) any longer (even though I love everything else about the application), I started looking at other Mac FTP/SFTP clients. Since performance was the key driving factor causing me to seek a new client, I thought it would probably be important to perform some informal performance comparisons between the major candidates--[Interarchy](http://www.interarchy.com/), [Transmit](http://www.panic.com/transmit/), and [Fetch](http://www.fetchsoftworks.com/) (and, for reference, Cyberduck as well).

First, the parameters of the test:

* The source system was, of course, a Mac. Specifically, a [MacBook Pro](http://www.apple.com/macbookpro/) running Mac OS X 10.4.8 with all available and applicable updates installed.

* The destination system was a server running ESX Server 3.0.1. It looks like ESX Server 3.0.1 uses OpenSSH 3.6.1p2.

* I transferred an ISO of Windows Server 2003 R2 x64; the file was about 592MB in size. I deleted the file from the destination server after each transfer.

The results of the test were as follows:

Transmit 3.5.5: 39 seconds  
Fetch 5.2: 39 seconds  
Interarchy 8.2.2: 29 seconds  
Cyberduck 2.7.2: 9 minutes 53 seconds

I hadn't truly realized just how much slower Cyberduck was until I ran this test. The difference is quite dramatic. As you can see, the test results between Transmit, Fetch, and Interarchy were pretty close; Interarchy edged the others out but only by a small margin.

Given that the performance is pretty much the same between the three replacement candidates comes down to aesthetics and features. I love the Kerberos support in Fetch (I'm a Kerberos fan---in case you hadn't noticed by all the articles I write about leveraging the Kerberos support in Active Directory for cross-platform integration), but really don't like the interface. Likewise, Transmit is nice, has a Quicksilver plug-in and a Dashboard widget, Keychain support, Spotlight integration, etc., but something about the interface doesn't seem to fit. I'm not sure what it is. This is not a knock against Transmit or [Panic](http://www.panic.com/); I paid for and use [Unison](http://www.panic.com/unison/), Panic's Usenet/NNTP client.

Interarchy's interface has its quirks, but I suppose I'm getting used to them now because I don't notice them as much. I just found and installed the Interarchy Quicksilver plug-in yesterday, which solves one complaint, but I still wish Interarchy would expand its Growl support.

Each of the apps has its advantages and disadvantages, as you can see.

What would be the ideal solution? The ideal solution would be a way to tune Cyberduck's performance so that it was in the same ballpark as the other applications (not necessarily faster, just in the same general area). I don't suppose anyone has any performance tuning tips for Cyberduck?
