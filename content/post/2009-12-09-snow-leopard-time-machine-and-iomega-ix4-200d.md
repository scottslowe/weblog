---
author: slowe
categories: Tutorial
comments: true
date: 2009-12-09T23:10:08Z
slug: snow-leopard-time-machine-and-iomega-ix4-200d
tags:
- Backup
- EMC
- macOS
- Storage
title: Snow Leopard, Time Machine, and Iomega ix4-200d
url: /2009/12/09/snow-leopard-time-machine-and-iomega-ix4-200d/
wordpress_id: 1766
---

Chad Sakac of EMC (visit [his weblog here](http://virtualgeek.typepad.com/)) recently sent me an Iomega ix4-200d storage unit. Considering that I didn't need it for my VMware lab---which runs both EMC and NetApp storage arrays---I pressed it into service at home. I can't tell you how useful it's been, especially the built-in Time Machine support. Making my three-year-old MacBook Pro running Leopard (Mac OS X 10.5) work with the Iomega's Time Machine implementation was a piece of cake: point it at the ix4-200d and you're all set. It's been great.

Recently, though, I picked up a new MacBook Pro running Snow Leopard. Unfortunately, due to some "under the covers" changes from Leopard to Snow Leopard, making your Snow Leopard-based Mac use the ix4-200d for Time Machine backups isn't quite so straightforward. Thankfully, [due to these instructions](http://www.insanelymac.com/forum/index.php?showtopic=184462), I was able to make it work without too much effort. Here's how.

First, you'll need to create a sparse disk image using the following command from Terminal.app:

	hdiutil create -size 500G -fs HFS+J -volname 'Time Machine' -type SPARSEBUNDLE <filename>.sparsebundle

Leopard required that the name of the sparse disk image be a concatenation of the computer's name and the MAC address of the Ethernet interface (en0) on the system. (By the way, you have to use the MAC address of en0 even if you are performing wireless backups over en1.) It appears that Snow Leopard does not have this restriction. I followed it anyway, just in case. `hdiutil` will create it in whatever directory you are in when you run the command. If you aren't in the TimeMachine directory on your ix4-200d (which would have a path of `/Volumes/TimeMachine` on your local Mac), then you'll need to copy the sparse disk image later. That's OK.

Second, you'll need to create a file named `com.apple.TimeMachine.MachineID.plist` and copy that into the sparse disk image. The contents of this file should look like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">  
<plist version="1.0">  
<dict>  
<key>com.apple.backupd.HostUUID</key>  
<string>_**System UUID Here**_</string>  
</dict>  
</plist>
```

You'll need to put your system's UUID here; you can get this value from System Profiler. Once you've created this file with the appropriate values, copy it into the sparse disk image you created. I did this with the Finder and it works fine, but if you want to use Terminal.app you can use this command:

	cp com.apple.TimeMachine.MachineID.plist <filename>.sparsebundle

Finally, copy---if necessary---the sparse disk image from wherever you created it to the TimeMachine share on the Iomega ix4-200d. Make sure you've mounted the TimeMachine share via AFP and then copy the sparsebundle over using this command:

	cp -pfr <filename>.sparsebundle /Volumes/TimeMachine/<filename>.sparsebundle

After you've completed all these steps, you can go into the Time Machine preferences and activate Time Machine against the ix4-200d. It worked seamlessly for me on Mac OS X 10.6.2, but your mileage may vary.

I also found [this site](http://sputteringdigitized.blogspot.com/2009/09/snow-leopard-and-time-machine-over-nfs.html) and [this site](http://www.markdeepwell.com/2009/11/using-ubuntu-for-time-machine-in-snow-leopard/) helpful in confirming the necessary contents of the `com.apple.TimeMachine.MachineID.plist` file, which is really the key to making it work in Snow Leopard.

I hope someone finds this useful!
