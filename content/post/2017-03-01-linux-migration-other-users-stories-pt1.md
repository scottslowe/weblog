---
author: slowe
categories: Information
comments: true
date: 2017-03-01T00:00:00Z
tags:
- Linux
- Windows
- Ubuntu
title: 'The Linux Migration: Other Users'' Stories, Part 1'
url: /2017/03/01/linux-migration-other-users-stories-pt1/
---

Shortly after I announced [my intention to migrate to Linux as my primary desktop OS][xref-1], a number of other folks contacted me and said they had made the same choice or they had been encouraged by my decision to also try it themselves. It seems that there is a fair amount of pent-up interest---at least in the IT community---to embrace Linux as a primary desktop OS. Given the level of interest, I thought it might be helpful for readers to hear from others who are also switching to Linux as their primary desktop OS, and so this post kicks off a series of posts where I'll share other users' stories about their Linux migration.

In this first post of the series, you'll get a chance to hear from Roddy Strachan. I've structured the information in a "question-and-answer" format to make it a bit easier to follow.

**Q: Why did you switch to Linux?**

I was a heavy Windows user due to corporate requirements. It was just easy to run Windows.  I never ran the standard corporate build, but instead ran my own managed version of Windows 10; this worked well. I switched because I wanted to experiment with Linux as the host OS, and run Windows as a VM. I'd experimented with Linux in VMs, but thought I would put a bit more time into the experiment and run it full-time. I spend most of my time in terminals/shells for work purposes, so other than using a Mac---which I would love to do, but I just can't justify the cost at the moment---Linux is the next best choice.

**Q: Which distribution of Linux did you choose?**

I'm fairly open to trying anything, over the years I've used all the major distributions (RHEL, CentOS, Fedora, Arch Linux, Debian, Ubuntu, and Mint). Although I run Arch Linux on a small ARM-based server, I ruled it out due to too much tinkering required to get it going in a desktop scenario. I wanted something that would allow me to be productive right away (I wanted things to "just work"). I tried Fedora 25 but ran into some issues with Wayland and my virtualization software, so I switched to Ubuntu 16.04.1 LTS. Most everything worked straight away (more on that in a moment).

**Q: What sort of hardware are you using?**

I'm running Linux on a Toshiba Portege Z30, which has an i7-4510U CPU (2 GHz core frequency), 16 GB of RAM, and a 512GB SSD. From a hardware perspective, this laptop is nice and fast and has all the functionality I need. It also has a built-in cellular modem, which is really handy when I'm out of the office. I'm using a Toshiba Dynadock docking station, which provides multi-monitor support and an additional network interface card (NIC), all over USB3.

**Q: What sort of challenges did you encounter, and how did you overcome them?**

By and large, Ubuntu picked up all the hardware. I installed the Dynadock drivers and multi-monitor support worked, and the cellular modem was picked up during installation. My Microsoft Bluetooth mouse didn't work for some odd reason, but after a bit of research it turns out I had to modify some Bluetooth daemon settings due to the lower energy use of this device. After modifying the settings, the device is picked up every time I turn it on.

I did run into an issue with the NIC supplied by the Dynadock. For some reason it wouldn't become the default NIC for the laptop when I was plugged into the dock. As a simple fix I decreased the metric for the Dyandock NIC to be lower than the onboard NIC, and it's now working fine.

Otherwise, it was just a matter of installing a few additional packages to tune the OS better for my needs, such as installing the "touchpad-indicator" package so I could customize touchpad behavior.

The only "problem" I still have is that I like to turn off devices like the  Wi-Fi or the cellular modem when I'm not using them. This causes an issue where I have to restart the Network Manager service before I can activate those interfaces again. I'm not sure if this is a bug or not, but I can live with restarting Network Manager for now.

**Q: What benefits have you seen after the switch?**

By moving my Windows 10 installation into a VM (which works perfectly for work-related things), I've been able to minimize some issues and delays with work proxy servers. This has made me more productive. Additionally, when I apply patches to the Windows 10 VM, I don't have to take the entire laptop down.

I've modified my working style so that I have my Windows 10 work VM on one monitor and Linux as my primary OS on the other monitor. After a few days, it feels quite natural.

**Q: Anything else you'd like others to know?**

Switching to Linux as your primary desktop OS won't be for everyone, but if you're like me and stick with it for a few good weeks you'll never look back!



[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
