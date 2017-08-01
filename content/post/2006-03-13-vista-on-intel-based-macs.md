---
author: slowe
categories: Information
comments: false
date: 2006-03-13T10:45:14Z
slug: vista-on-intel-based-macs
tags:
- Apple
- Macintosh
- Microsoft
- Vista
- Windows
title: Vista on Intel-Based Macs
url: /2006/03/13/vista-on-intel-based-macs/
wordpress_id: 199
---

Apparently, there are some rather heated arguments occurring in certain circles about the inability to boot [Microsoft Windows Vista](http://www.microsoft.com/windowsvista/) on the new Intel-based Macs (as a side note, I seriously do not like the term "Mactel," so I'll instead be referring to them as "Intel-based Macs"). While I can see both sides of the debate, I personally feel that holding [Apple](http://www.apple.com/) to blame for not being able to run Windows is a bit of a stretch.

I reviewed portions of a rather lengthy thread in the comp.sys.mac.system Usenet newsgroup regarding this issue, and it basically comes down to the fact that (according to reports) Vista will only support [EFI](http://www.intel.com/technology/efi/) (Extensible Firmware Image) on 64-bit platforms. Of course, the new [MacBook Pro](http://www.apple.com/macbookpro/) and [iMac](http://www.apple.com/imac/) use EFI, but these are 32-bit platforms. As a result, Vista won't boot. Also according to reports, adding BIOS support (to allow non-EFI-aware operating systems to run) is as simple as adding a module to EFI to emulate BIOS. So, a number of people are holding Apple to blame for failing to load the BIOS emulation module in EFI on the Intel-based Macs, saying that it intentionally prevented Windows Vista from booting on the new Macs.

Well, I _suppose_ you could put it that way. Then again, Apple did say that they weren't going to do anything to make sure that Windows ran on the new Intel-based Macs, nor were they going to support it on their hardware. Is Apple intentionally preventing Vista from running on their hardware, or are they just doing what they said?

As to the argument that Apple is missing potential sales by not making the Intel-based Macs capable of booting Windows Vista, I can't say that I agree with that one. Yes, it _might_ be possible to increase value for Apple shareholders by spending extra time and effort to make their hardware more compatible with Windows. But why burden down your hardware with legacy technology that your own core products don't really need? [Mac OS X](http://www.apple.com/macosx/) doesn't need BIOS support, so why add BIOS support to Mac hardware? On the chance that you _might_ gain marketshare via that small percentage of people who want to dual-boot Windows Vista?

If Apple really wants to increase marketshare, the fast, best, and cheapest option (in my opinion) involves promoting virtualization and emulation. No, these aren't conflicting directions. First, support virtualization by promoting open source and commercial virtualization products, such as [VMware](http://www.vmware.com/), [Q](http://www.kberg.ch/q/index.php), and others. (I've mentioned the [use of virtualization to drive marketshare][1] before.) Second, support emulation of the Windows API via such projects as [WINE](http://www.winehq.com/) and [Darwine](http://darwine.opendarwin.org/), which allow Windows applications to run on non-Windows operating systems. Think about it: If the only reason you want to boot Windows on your new Intel-based Mac is for a couple of applications that don't have Macintosh equivalents, why install an entire OS?

[1]: {{< relref "2006-02-13-apple-and-virtualization.md" >}}
