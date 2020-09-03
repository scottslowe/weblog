---
author: slowe
categories: Review
comments: true
date: 2020-06-08T15:45:00-07:00
tags:
- Hardware
- Apple
- Linux
- Fedora
title: "Review: Magic Mouse 2 and Magic Trackpad 2 on Fedora"
url: /2020/06/08/review-magic-mouse-2-and-magic-trackpad-2-on-fedora/
---

I recently purchased a new Apple Magic Mouse 2 and an Apple Magic Trackpad 2---not to use with my MacBook Pro, but to use with my [Fedora][link-1]-powered laptop (a Lenovo 5th generation ThinkPad X1 Carbon; see [my review][xref-1]). I know it seems odd to buy Apple accessories for a non-Apple laptop, and in this post I'd like to talk about why I bought these items as well as provide some (relatively early) feedback on how well they work with Fedora.<!--more-->

## The Justification/Background

First, let me talk about the _why_ behind my purchase of these items. Several years ago, I started simultaneously using both an external trackpad/touchpad _and_ an external mouse with my macOS-based home office setup. I realize this is probably odd, but I adopted the practice as a way of eliminating "mouse finger" on my right hand. With this arrangement, I stopped trying to scroll with my right-hand (either using a mouse wheel with older mice or using the scroll-enabled back of the Magic Mouse), and instead shifting scrolling to my left hand (using two-finger scrolling on the trackpad). This "division of labor" worked well. Because my existing Magic Mouse and Magic Trackpad---both earlier generations---don't work with non-Apple laptops, I had to give up this arrangement when I switched to Linux. Sure enough, though, my index finger on my right hand started hurting again from the use of the scroll wheel. Switching fingers helped only temporarily. I needed to go back to a solution that allowed me to do two-finger scrolling.

After searching quite a bit for a Linux-compatible external trackpad, I [put out a query on Twitter][link-2]. A couple folks responded with the Magic Trackpad 2, which I'd seen referenced in other places as a potential candidate as well. I was skeptical (the original generation product hadn't worked _at all_ with Linux), I went ahead and ordered both a Magic Trackpad 2 and a Magic Mouse 2 (both in "Space Gray," so as to match my keyboard---no sense in giving up aesthetics!). In the next couple of sections, I'll provide my feedback after a few hours of using the devices with Fedora 31.

## Pairing

My intent was to use both devices wirelessly, so as to preserve USB ports. I unboxed the Magic Trackpad 2 first. Upon powering up the device, it immediately appeared in the Bluetooth settings screen on my laptop, and with a single-click I paired it with Fedora. No muss, no fuss---it just worked. Honestly, I was pleasantly surprised.

I followed that up with the Magic Mouse 2, which I was using to replace a wired Microsoft Intellimouse Pro. Just like the Magic Trackpad 2, it immediately paired with the laptop with no issues whatsoever.

## Functionality

Out of the box, all of the functionality of the Magic Trackpad 2 worked with no configuration needed. Mind you, I'm using GNOME on X11 (not Wayland, because reasons), so "all of the functionality" is two-finger scrolling, clicking with one finger (primary button), and clicking with two fingers (secondary button). GNOME on Wayland support a three-finger "pinch" or "zoom" gesture that activates the GNOME Activities screen; it worked as expected with the Magic Trackpad 2. The Magic Mouse 2 works fine as a mouse, with both primary button and secondary button working as expected, but without any scrolling functionality. The loss of scrolling functionality is OK with me, since I typically disable it anyway. If you want scrolling functionality, be aware that it didn't work for me under GNOME on X11.

## Caveats and Issues

Nothing significant, but a couple of things to note:

* There's no straightforward way to check the charge level of either device. I haven't found any external visual indicators, and Fedora (Linux in general, as far as I can tell) isn't displaying that information, either. It's possible there's an obscure command I can run; I'll update this post if I find it.
* Occasionally, two-finger scrolling on the Magic Trackpad 2 won't work; it's almost like the pressure sensitivity of the trackpad (to recognize multiple touch points) needs to be adjusted. Unfortunately, there doesn't seem to be an easy way to adjust it. Fortunately, thus far this behavior has been intermittent and easily resolved by shifting the placement of my fingers on the trackpad.
* The Magic Mouse 2 retains the absurdly-stupid placement of the charging port on the bottom of the mouse, rendering it unusable while charging. I much rather would've seen Apple redesign the mouse to allow a charging port at the "top end" of the mouse, even if the overall aesthetics of the mouse had to be altered as a result.
* _(Added after a week of usage)_ As a Bluetooth device, the Magic Trackpad 2 does occasionally "disconnect" itself, and has issues reconnecting. I haven't been able to find any patterns here, so I switched to using it as a USB device (daisy-chained off my keyboard). It works fabulously as a USB device. _(Update: While my original testing was on Fedora 31, I've also seen this behavior after upgrading to Fedora 32.)_

Overall, I'm reasonably satisfied with both the Magic Mouse 2 and the Magic Trackpad 2 and their support for/in Linux. I hope this review has been helpful; feel free to connect with [me on Twitter][link-3] if you have additional questions or other feedback. Thanks for reading!

[link-1]: https://getfedora.org
[link-2]: https://twitter.com/scott_lowe/status/1268606420540198913
[link-3]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-04-13-review-lenovo-thinkpad-x1-carbon.md" >}}
