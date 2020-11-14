---
author: slowe
categories: Tutorial
comments: true
date: 2010-07-21T19:55:09Z
slug: changing-rdp-settings-in-vmware-view-open-client-for-mac
tags:
- macOS
- VDI
- Virtualization
- VMware
title: Changing RDP Settings in VMware View Open Client for Mac
url: /2010/07/21/changing-rdp-settings-in-vmware-view-open-client-for-mac/
wordpress_id: 2007
---

I've been using the [VMware View Open Client](http://code.google.com/p/vmware-view-open-client/) for Mac OS X for a few weeks now, ever since I was approved to participate in EMC's pilot VDI program. The one thing that I don't like about the View Open Client is how it leverages the Mac-native Remote Desktop Connection application to make connections to the View hosted desktops. It does so in a way that makes it impossible to customize the behavior of the RDP session---or so I thought.

It turns out there there is a way, after all, to customize the behavior of the RDP session that the View Open Client leverages. The View Open Client has an .RDP file, containing all the saved settings for RDP connections generated through the View Open Client, embedded inside the client itself. And because applications on Mac OS X are nothing more than specially-treated folders, it's possible to crack open the client and actually customize the session parameters.

Here's how:

1. Open the Applications folder on your Mac (or wherever you installed the VMware View Open Client).

2. Right-click (or Ctrl-Click) on the VMware View Open Client and select Show Package Contents.

3. Open the Contents folder, then open the Resources folder.

4. In the Resources folder, you'll see a file named `vmware-view.rdp`. This is the template the View Open Client uses to generate new RDP connections. By modifying this file, you can modify the behavior of the RDP sessions that View creates.

5. Open the `vmware-view.rdp` file in a text editor and make any desired changes. When you attempt to save the changes, you will most likely be prompted for authentication (because you are modifying the contents of an application in the Applications folder).

That's it! It's actually a lot easier than it might seem. For example, I didn't like the fact that the EMC corporate VDI connection played that stupid Windows logon sound, so I modified the `vmware-view.rdp` file to change the value of the `AudioRedirectionMode` parameter so that it wouldn't play music when I logged into a VDI image. All I had to do was change the integer value of the `AudioRedirectionMode` parameter to two, like this:

```xml
<key>AudioRedirectionMode</key>  
<integer>2</integer>
```

Voila! No more sounds being sent across my RDP connection. I haven't yet found a comprehensive breakdown of all the parameters, although [this page is a good start](http://www.coe.uncc.edu/mosaic/remote_desk/RDP%20File%20Settings.htm).

The key drawback to this mechanism is, of course, that you can't selectively apply these settings to different VDI connections. For example, I might not want to bring sound across for my corporate VDI session, but what if I'm connecting to a partner's VDI environment and I want sound for that session? By modifying the template `vmware-view.rdp` file inside the View Open Client itself, the changes you make apply to **all** sessions. Perhaps a future revision of the View Open Client will give us some per-session granular control over these settings? (Hey, I can dream!)

Have a better way of accomplishing this? Let me know in the comments! Courteous and professional comments are welcome. 
