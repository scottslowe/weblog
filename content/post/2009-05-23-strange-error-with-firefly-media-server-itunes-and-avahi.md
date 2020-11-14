---
author: slowe
categories: Explanation
comments: true
date: 2009-05-23T20:10:21Z
slug: strange-error-with-firefly-media-server-itunes-and-avahi
tags:
- Linux
- macOS
- Music
- Networking
- OSS
- Ubuntu
- Windows
title: Strange Error with Firefly Media Server, iTunes, and Avahi
url: /2009/05/23/strange-error-with-firefly-media-server-itunes-and-avahi/
wordpress_id: 1375
---

Over the 2008-2009 holiday season, I rebuilt my home network. I included the notes and information from my home network rebuild in [an article that described the Mac OS X-Ubuntu integration][1] resulting from the rebuild. Since that time, I've added a larger hard drive to the home server to make more room for Time Machine backups, movies, music, and other files. Things seemed to be working very well. Until the other day...

My wife made an offhand comment that she couldn't access the shared music library from her laptop. I tested the connection and, sure enough, every time I clicked the shared library icon it simply disappeared. No error, no warning, no entries in any log files...it just disappeared. I searched the Windows event logs, and I searched the log files on the Ubuntu server downstairs. Neither computer had any entries whatsoever that provided any insight as to why this one computer would not connect to the shared music library.

Being the geeky troubleshooter that I am, I attempted to replicate the problem on some of the other computers on the network. My MacBook Pro worked fine. Three other Windows laptops on the network, running the same version of Windows (Windows XP Professional) and the same Service Pack revision, also worked fine. The problem seemed to be isolated to her computer. Perhaps it was only when she was on the wireless network...nope, the same problem regardless of the network connection.

I upgraded iTunes to the latest version. That didn't work. I disabled the Windows Firewall on her computer. That didn't work. I made sure that no traffic was being blocked by the firewall on the Ubuntu server; no traffic was being blocked. In other words, that didn't work. I was about to give up and just write it off as one of those strange aberrations that couldn't be resolved and chalk it up to Windows.

Then I stumbled onto [this site](http://www.vleeuwen.net/2009/05/setup-firefly-to-serve-itunes). I'd already created a daapd.service file for Avahi to use previously, but this site described some additional entries in the daapd.service file that I didn't have. I made some edits, based on the information on the site, and here's the daapd.service file I had for Avahi:

```xml
<?xml version="1.0" standalone='no'?><!--*-nxml-*-->  
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">  
<service-group>  
<name replace-wildcards="yes">Home Media Server</name>  
<service>  
<type>_daap._tcp</type>  
<port>3689</port>  
<txt-record>txtvers=1</txt-record>  
<txt-record>iTSh Version=131073</txt-record>  
<txt-record>Version=196610</txt-record>  
</service>  
</service-group>
```

After changing the daapd.service file to the version listed above, I restarted Avahi. Upon the shared media server re-appearing in iTunes, I clicked on it and...drum roll please...it worked! The previous version I had been using did not have the `txt-record` entries, and I really have no idea why adding the `txt-record` entries suddenly made my wife's iTunes connect properly. I suppose it doesn't matter why it works, it just matters that I **FIXED IT!** (ePlus engineers who attended our NSM this year will get this joke.)

Still, in the event you're running into the same issue---a Windows installation of iTunes that fails to connect to a shared music library running on Firefly Media Server---then perhaps updating your Avahi configuration will correct the problem.

[1]: {{< relref "2009-01-02-ubuntu-and-mac-os-x-integration.md" >}}
