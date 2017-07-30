---
author: slowe
categories: Tutorial
comments: true
date: 2009-03-13T13:04:44Z
slug: next-gen-stuff-enabling-cdp-in-esxesxi
tags:
- Cisco
- ESXi
- Networking
- Virtualization
- VMware
- vSphere
title: 'Next-Gen Stuff: Enabling CDP in ESX/ESXi'
url: /2009/03/13/next-gen-stuff-enabling-cdp-in-esxesxi/
wordpress_id: 1225
---

Now that the veil appears to have been lifted on discussing features and functionality in the next-generation of ESX/ESXi, I thought I'd start out with a brief how-to on enabling Cisco Discovery Protocol (CDP) on ESX/ESXi, both with vNetwork Standard Switches (vSwitches) and vNetwork Distributed Switches (dvSwitches).

Let's start out with an easy one: enabling CDP on ESX for a vSwitch. There is no GUI for enabling or disabling CDP for a vSwitch (yet), so it's off to the CLI. You've probably seen this command before:

	esxcfg-vswitch --set-cdp both vSwitch0

Replace "vSwitch0" in that command with the appropriate information for your environment. That sets CDP to both listen (receive CDP transmissions) and announce (send CDP transmissions). I recommend using both, although I have not currently found a way to explore the CDP information that ESX is gathering by listening to announcements. If anyone has any information there, I'd love to hear it.

The next one is a bit harder: enabling CDP on ESXi for a vSwitch. Of course, since we are using ESXi there is no Service Console, so this time we'll have to rely upon the next-generation equivalent of VIMA.

The command from next-gen VIMA looks like this:

	vicfg-vswitch --server <vcenter.domain.com> -h <esxi.domain.com> -B both vSwitch0

Unless you've set some environment variables, you'll be prompted for username and password. I substituted the "-h" for "-vihost" and "-B" for "-set-cdp". Again, you'll need to replace vCenter Server name, ESXi host name, and vSwitch name with the appropriate information for your environment. I did find that using IP addresses didn't seem to work well; I had to use fully-qualified domain names instead. That's probably just an oddity.

The last scenario is enabling CDP on a dvSwitch. The process for this is the same for both ESX and ESXi. You'll need to use the next-generation VI Client for this, and then go to the Edit Settings screen for the dvSwitch. Once there select Advanced, and you'll see the option to set the CDP behavior. Click Both from the drop-down list and you should be good to go.

There's lots more networking goodness in here; stay tuned for more articles in the near future.
