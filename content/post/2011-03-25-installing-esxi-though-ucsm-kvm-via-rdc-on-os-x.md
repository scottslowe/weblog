---
author: slowe
categories: Explanation
comments: true
date: 2011-03-25T20:05:35Z
slug: installing-esxi-though-ucsm-kvm-via-rdc-on-os-x
tags:
- Cisco
- ESXi
- macOS
- UCS
- Virtualization
- VMware
- vSphere
title: Installing ESXi though UCSM KVM via RDC on OS X
url: /2011/03/25/installing-esxi-though-ucsm-kvm-via-rdc-on-os-x/
wordpress_id: 2249
---

How's that for acronyms?

In all seriousness, though, as I was installing VMware ESXi this evening onto some remote Cisco UCS blades, I ran into some interesting keymapping issues and I thought it might be handy to document what worked for me in the event others run into this issue as well.

So here's the scenario: I'm running Mac OS X 10.6.7 on my MacBook Pro, and using VMware View 4.6 to connect to a remote Windows XP Professional desktop. Within that Windows XP Professional session, I'm running Cisco UCS Manager 1.4(1i) and loading up the KVM console to access the UCS blades. From there, I'm installing VMware ESXi onto the blades from a mapped ISO file.

What I found is that the following keystrokes worked correctly to pass through these various layers to the ESXi install process:

* For the F2 key (necessary to log in to the ESXi DCUI), use Ctrl+F2 (in some places) or Cmd+F2 (in other places).

* For the F5 key (to refresh various displays), the F5 key alone works.

* For the F11 key (to confirm installation at various points during the ESXi install process), use Cmd+F11.

* For the F12 key (used at the DCUI to shutdown/reboot), use Cmd+F12.

There are a couple of factors that might affect this behavior:

* In the Keyboard section of System Preferences, I have "Use F1, F2, etc., keys as standard function keys" selected; this means that I have to use the Fn key to access any "special" features of the function keys (like increasing volume or adjusting screen brightness). I haven't tested what impact this has on this key mapping behavior.

* The Mac keyboard shortcuts in the preferences of the Microsoft Remote Desktop Connection do not appear to conflict with any of the keystrokes listed above, so it doesn't appear that this is part of the issue.

If I find more information, or if I figure out why the keystrokes are mapping the way they are I'll post an update to this article. In the meantime, if you happen to need to install VMware ESXi into a Cisco UCS blade via the UCSM KVM through VMware View from a Mac OS X endpoint, now you know how to make the keyboard shortcuts work.

Courteous comments are always welcome---speak up and contribute to the discussion!
