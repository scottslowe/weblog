---
author: slowe
categories: Review
comments: true
date: 2018-04-13T08:00:00Z
tags:
- Linux
- Hardware
- Fedora
title: 'Review: Lenovo ThinkPad X1 Carbon'
url: /2018/04/13/review-lenovo-thinkpad-x1-carbon/
---

As part of the transition into my new role at [Heptio][link-1] (see [here][xref-3] for more information), I had to select a new corporate laptop. Given that my last attempt at running Linux full-time was thwarted due primarily to work-specific collaboration issues that would no longer apply (see [here][xref-1]), and given that other members of my team (the Field Engineering team) are also running Linux full-time, I thought I'd give it another go. Accordingly, I've started working on a Lenovo ThinkPad X1 Carbon (5th generation). Here are my thoughts on this laptop.<!--more-->

This is now my second non-Apple laptop in the last year. My previous non-Apple laptop, a Dell Latitude E7370, was a pretty decent laptop (see [my review][xref-2]). As good as the E7370 was, though, the X1 Carbon is _better_.

The X1 Carbon features a dual-core i7 7500U CPU, which (subjectively, anyway) outperforms the mobile CPU in the E7370. This makes the X1 Carbon feel quite snappy and responsive. CPU performance was an issue for me with the Dell---it didn't take much to tax that mobile CPU. I haven't seen that issue so far with the X1 Carbon. Coupled with 16GB of RAM, the X1 Carbon is no slouch. My particular model only came with a 256GB NVMe SSD (the E7370 had a 512GB NVMe SSD), but given that I won't be storing tons of media files or dozens of VMs I don't anticipate that the smaller disk will be an issue.

Like the E7370, the X1 Carbon also leverages Intel HD graphics. This is one area where the Dell may have had an edge on the X1 Carbon; the Dell offered a 3200x1800 touchscreen, whereas the X1 Carbon---my particular model---supports 1920x1080, and no touchscreen support. To be fair, I ended up running the Dell at a lower resolution to work around issues pertaining to scaling with external monitors, so the lower resolution of the X1 Carbon isn't really a big deal. I also didn't use the touchscreen functionality, so I don't miss it either. Screen quality and brightness are really good, in my opinion.

Like many other laptops, the ThinkPad also uses an Intel wireless card with support for all the major 802.11 standards. Wireless performance thus far has been pretty respectable; I haven't run into any issues or problems.

Battery life has (so far) been _very_ good. I'm impressed. I haven't even done any tuning yet, and the battery life easily surpasses the E7370 (which was fairly decent).

I do feel that I need to call out the keyboard on the X1 Carbon, which is nothing short of fantastic. It is handily far better than the keyboards on the latest MacBook Pros (which have had no end of problems). It's probably the best laptop keyboard I've used in quite a while.

The quality of the laptop is good. It feels strong and sturdy but is still light and thin, making it easy to carry while traveling (an important aspect for me). I didn't know quite what to expect since this was my first ThinkPad (hard to believe, right?).

Last, but not least, how is the X1 Carbon with regards to Linux support? With the exception of the fingerprint scanner (which is a known issue), everything works out of the box with [Fedora][link-2] 27. Keyboard function keys work without any fiddling or extra configuration required, suspend (by closing the lid) and resume work without any issues, and both the trackpoint and the trackpad work as expected. I'd seen some reports that one or the other needed to be disabled, but both have been working for me without any issues.

Overall, I'm pretty happy with the ThinkPad X1 Carbon, and would recommend it to anyone looking for a lightweight but powerful laptop on which to run Linux.

[link-1]: https://heptio.com/
[link-2]: https://getfedora.org/
[xref-1]: {{< relref "2017-10-25-linux-migration-wrap-up.md" >}}
[xref-2]: {{< relref "2017-01-30-review-dell-latitude-e7370.md" >}}
[xref-3]: {{< relref "2018-04-02-the-future-is-containerized.md" >}}
