---
author: slowe
categories: Tutorial
comments: true
date: 2009-01-02T12:36:06Z
slug: ubuntu-and-mac-os-x-integration
tags:
- Apache
- Apple
- Interoperability
- Linux
- macOS
- Music
- Samba
- Ubuntu
title: Ubuntu and Mac OS X Integration
url: /2009/01/02/ubuntu-and-mac-os-x-integration/
wordpress_id: 1118
---

One of my projects over the Christmas holiday has been to rebuild the home network. You'd think I'd want to avoid that sort of thing since I've been on vacation from work for the past two weeks, but working on a home network is a different sort of beast than working on a network for a company. There are different challenges to be addressed.

My primary goals for this "home network rebuild" were the following:

1. Rebuild the home server with a newer version of Linux, and possibly switch to a different distribution.

2. Continue to provide DNS, DHCP, HTTP, and HTTP proxying/content filtering services to the home network.

3. Continue to provide file sharing services via Server Message Block/Common Internet File System (SMB/CIFS) for Windows-based systems on the home network.

4. Continue to have a shared music library available via Digital Audio Access Protocol (DAAP, aka iTunes) available to all systems on the home network.

5. Provide file sharing services to Macs on the network via AppleTalk Filing Protocol (AFP) over TCP.

Ideally, I also wanted to enable Time Machine backups from my Mac laptop to the home server.

After doing a fair amount of research, I settled on the use of [Ubuntu](http://www.ubuntu.com/) 8.04 LTS ("Hardy Heron") for the server build. I didn't go with Ubuntu 8.10 ("Intrepid Ibex") simply because a) I already had 8.04.1 downloaded and burned to a CD; and b) Hardy Heron is an LTS release, so I should have better support over the long term.

I won't bore readers with the details of the rebuild, but it took about a day or two to get a larger hard drive installed, Ubuntu installed and configured, and services running like DHCP (including some static reservations for certain computers, like my laptop and my iPhone), DNS (using [MaraDNS](http://www.maradns.org/), much easier to figure out than BIND), [Apache](http://httpd.apache.org/), and [Squid](http://www.squid-cache.org/) with [SquidGuard](http://www.google.com/url?q=http://www.squidguard.org/&sa=X&oi=revisions_result&resnum=1&ct=result&cd=1&usg=AFQjCNH4w9W4Lq6vGuMF80X7BgDwIQq16A). At this point, I'd completed tasks #1 and #2.

On to task #3. This was pretty simple and straightforward and easily accomplished via [Samba](http://www.samba.org/), with nothing really unique to document here. The one interesting thing that I did find was a way to map the long usernames that Mac OS X uses (like "Bob Jones") to a short username (like "bjones"). I used this command in the `/etc/samba/smb.conf` file:

	username map = /etc/samba/usermap.conf

In this file, I simply placed lines that mapped the long usernames to the short usernames. Since Mac OS X defaults to the long username when connecting to the server, this allows me to simply type in a password and connect. I searched for hours trying to find a way to have Mac OS X supply my current password to the Samba server so that I wouldn't get prompted, but could not find any information. If anyone knows the trick, I'd love to hear about it. After configuring a few shares, setting Linux permissions and the umask, and then testing from both my Mac laptop and a Windows laptop, task #3 was finished.

Task #4, providing an iTunes-compatible music server, was also really straightforward and easy. For this, I again selected [Firefly Media Server](http://www.fireflymediaserver.org/), formerly mt-daapd, which I'd used before with great success. Again, nothing unusual or unique to document here, except for the potential interaction with Avahi (more on that later).

The final task was installing [Netatalk](http://netatalk.sourceforge.net/) to provide AFP over TCP file sharing services for Macs on the network. Fortunately for me, one of the sites I'd been using to help in my project pointed me to [this blog post](http://gpz500.wordpress.com/2008/09/27/lairone-al-servizio-del-leopardo/), which had a prebuilt package for Netatalk that included the necessary SSL support that Mac OS X requires. That saved me the trouble of compiling Netatalk from source. Following the steps in [the Kremalicious article](http://www.kremalicious.com/2008/06/ubuntu-as-mac-file-server-and-time-machine-volume/) as well as information from [this guide](http://www.zaphu.com/2008/04/29/ubuntu-guide-configure-a-netatalk-file-server-based-on-apple-filing-protocol-afp/), I configured Netatalk to present a volume to use for Time Machine backups. It was at this point that I noticed a strange interaction with Avahi.

[Avahi](http://www.avahi.org/) is a multicast DNS (what Apple calls Bonjour) server for Linux. It's responsible for advertising services to multicast DNS-enabled systems, such as other Linux systems running Avahi or Macs. I'd installed Avahi earlier and used some service definitions [from this article](http://holyarmy.org/benjamin/2008/01/advertising-linux-services-via-avahibonjour/) and [this blog post](http://www.zaphu.com/2008/04/29/ubuntu-guide-configure-avahi-to-broadcast-services-via-bonjour-to-mac-os-x/) to advertise Samba and HTTP. In addition, after installing Firefly, I'd noticed that Firefly starting advertising its presence automatically through Avahi with no service definition required.

Upon installing Netatalk, I also noticed that Netatalk started advertising automatically via Avahi as well, but using the IP address of the server. In order to be able to control how Netatalk advertises via Avahi, I had to change this line in `/etc/avahi/avahi-daemon.conf`:

	enable-dbus=no

The suggestion for this change came from [this thread on the Ubuntu Forums](http://ubuntuforums.org/archive/index.php/t-347019.html). Upon making the change and restarting Avahi, the odd Netatalk entry went away, but so did Firefly! To advertise both Netatalk and Firefly, I added a couple of files to `/etc/avahi/services`:

**afpd.service:**

```xml
<?xml version="1.0" standalone='no'?><!--*-nxml-*-->  
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">  
<service-group>  
<name replace-wildcards="yes">Intrepid Time Machine</name>  
<service>  
<type>_afpovertcp._tcp</type>  
<port>548</port>  
</service>  
<service>  
<type>_device-info._tcp</type>  
<port>0</port>  
<txt-record>model=AirPort</txt-record>  
</service>  
</service-group>
```

**daapd.service:**

```xml
<?xml version="1.0" standalone='no'?><!--*-nxml-*-->  
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">  
<service-group>  
<name replace-wildcards="yes">Home Music Server</name>  
<service>  
<type>_daap._tcp</type>  
<port>3689</port>  
</service>  
</service-group>
```

After placing these two files into `/etc/avahi/services`, the new services starting advertising immediately. By the way, you'll note the extra "device-info" entry in `afpd.service`; that sets the icon that will be used by Macs when they discover this service. I made mine look like a Time Capsule by using the setting listed above.

During this work with Avahi, I uncovered a couple of interesting things:

* I found that restarting the Avahi daemon is actually more problematic than just leaving it alone; in order to make it start advertising services again after a restart, you'll have to open one of the files in `/etc/avahi/services` and then close it again. No changes are necessary to the file, but opening it will kickstart Avahi into service advertisement.

* Advertising SMB/CIFS and AFP together with the same name caused my Mac to ignore the SMB/CIFS services and only use AFP. I had to separate SMB/CIFS and AFP into different entries. Since I was using AFP really only for Time Machine backups and SMB/CIFS for everything else, it wasn't really a big deal.

* Advertising SMB/CIFS and RFB (Screen Sharing, as [outlined here](http://www.zaphu.com/2008/04/29/ubuntu-guide-configure-vinagre-to-share-the-screen-with-mac-os-x/)) works fine together.

At this point, task #5 was pretty much complete. I had originally envisioned providing file sharing services to the same locations via both AFP and SMB/CIFS, but in the end---partially because of the odd issue with AFP and SMB/CIFS being advertised together as described above---settled for using AFP only for Time Machine and SMB/CIFS for everything else.

Along the way, I also configured screen sharing [as outlined here](http://www.zaphu.com/2008/04/29/ubuntu-guide-configure-vinagre-to-share-the-screen-with-mac-os-x/), and it seems to work just fine. I have to leave an account logged in to the Ubuntu server, but I can just lock the screen when I'm not logged in remotely.

The last step was to enable Time Machine backups to the Ubuntu server via AFP. First, the hack to enable non-Time Capsule network backups (this should be all on one line):

	defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1

I was then able to select the Ubuntu-hosted AFP volume for Time Machine backups. Attempting to run a Time Machine, backup, however, reported an error about being "unable to create the disk image". Fortunately, a number of different articles pointed to the use of `hdiutil` to create the disk image, and that seemed to fix the problem. Time Machine is now backing up to the AFP volume, although I suspect I still have a few issues to work through (for example, it looks as though I have to keep the Time Machine AFP volume mounted in order for automatic backups to run).

So, when everything is said and done, I was able to achieve all my stated goals. The only outstanding issue that I haven't yet figured out yet centers on automatic logins; for both AFP and SMB/CIFS, I get prompted for a password when connecting, even though I keep my password synchronized (manually) between my Mac and the Ubuntu server. Any tips on how to resolve that would certainly be appreciated.

Along the way, I found the following sites to be quite helpful. I'm sure there are others that I used along the way, and I apologize if I've failed to extend credit where credit is due.

[Limit size of Time Machine backups on Time Capsule](http://www.macosxhints.com/article.php?story=20080519051720677)  

[Set up Time Machine on a NAS in three easy steps](http://www.macosxhints.com/article.php?story=20080420211034137)  

[Make Ubuntu a Perfect Mac File Server and Time Machine Volume](http://www.kremalicious.com/2008/06/ubuntu-as-mac-file-server-and-time-machine-volume/)  

[Five Guides on How to Integrate Ubuntu into a Mac OS X Network](http://www.zaphu.com/2008/04/30/five-guides-on-how-to-integrate-ubuntu-into-a-mac-os-x-network/)  

[Time Machine Wireless Backups without Time Capsule](http://adamcohenrose.blogspot.com/2008/02/time-machine-wireless-backup-without.html)
