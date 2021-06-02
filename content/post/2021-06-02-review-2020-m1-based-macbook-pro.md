---
author: slowe
categories: Review
comments: true
date: 2021-06-02T11:00:00-06:00
tags:
- Apple
- Hardware
- macOS
title: 'Review: 2020 M1-Based MacBook Pro'
url: /2021/06/02/review-2020-m1-based-macbook-pro/
---

I hadn't done a personal hardware refresh in a while; my laptop was a 2017-era MacBook Pro (with the much-disliked butterfly keyboard) and my tablet was a 2014-era iPad Air 2. Both were serviceable but starting to show their age, especially with regard to battery life. So, a little under a month ago, I placed an order for some new Apple equipment. Included in that order was a new 2020 13" MacBook Pro with the Apple-designed M1 CPU. In this post, I'd like to provide a brief review of the 2020 M1-based MacBook Pro based on the past month of usage.<!--more-->

The "TL;DR" of my review is this: the new M1-based MacBook Pro offers impressive performance _and_ even more impressive battery life. While the raw performance may not "blow away" its 2020 Intel-based counterpart---at least, it didn't in my real-world usage---the M1-based MacBook Pro offered consistently responsive performance with a battery life that easily blew past any other laptop I've ever used, bar none.

Read on for more details.

## Hardware

The build quality is really good, with a **significant** improvement in keyboard quality relative to the earlier butterfly keyboard models (such as my 2017-era MacBook Pro). However, the overall design of the laptop remains identical to earlier models. In some respects, that's OK; for example, I don't mind the oversized trackpad, and given that this is my first laptop with the Touch Bar I'm finding I don't really mind it either. (I wouldn't say I _like_ it, necessarily, but I don't hate it.) In other areas, like connectivity, sticking to the previous design means having a limited number of Thunderbolt/USB-C ports instead of a full complement of ports. There are rumors that the "next-generation" MacBook Pros will have updated Apple-designed CPUs and more ports; we shall see soon (since Apple's Worldwide Developer Conference [WWDC] is just around the corner).

Although the updated keyboard (aka the "Magic Keyboard", a marketing name that Apple seems to splash on every keyboard they make these days) is far better than the butterfly keyboard, I would still rank the keyboard on the Lenovo X1 Carbon higher (see [my review of a 5th generation X1 Carbon][xref-1]). That being said, though, at least I don't hate typing on the M1-based MacBook Pro, and the typing noise is far reduced.

Finally, I'm finding the addition of Touch ID to be a useful and practical addition. Apple is certainly not alone in providing fingerprint readers on their laptops and other devices, but the integration here between hardware and software makes the addition of a fingerprint reader something that actually positively impacts the day-to-day user experience.

Speaking of software...

## Software

The M1-based MacBook Pro, along with all other models based on Apple-designed ARM CPUs, ship with macOS 11 "Big Sur". Because earlier versions of macOS didn't support ARM-based CPUs, and given that a fair amount of user value is derived from the tight integration of hardware and software, I think it's reasonable to discuss macOS 11 in the context of a review of my 2020 M1-based MacBook Pro.

Touch ID is a great example (this isn't new to macOS 11, however). As I mentioned a couple paragraphs above, the addition of a fingerprint reader at the hardware level isn't revolutionary at all. However, providing OS-level constructs to use that fingerprint reader to authenticate the user _does_ make a difference in the day-to-day usage of the product.

Similarly, integrations between macOS 11 and iOS via features like Handoff, Airdrop, and others are practically useful integrations that enhance the user experience. Again, although none of these features are new to macOS 11, they are present (maybe improved?) in macOS 11 and are worth mentioning here, I think.

Another area where software enhances the hardware is through macOS' use of Quality of Service (QoS) in placing workloads across the efficiency and performance cores of the M1 CPU. [This article][link-1] provides some good details on this QoS, and my own observations of CPU utilization during my day-to-day usage mirror the findings outlined in the article.

Apple's use of Rosetta 2 seems to work well; I've run several Intel-based applications, both GUI and text-based, without any issues. I also haven't noted any performance concerns, although I have not specifically conducted any performance tests. I mention this simply to assuage concerns about running older Intel-based applications on the M1-based hardware. That being said, I will note that finding ARM-native versions of popular GUI apps is becoming easier ([this site][link-4] is helpful, BTW), but finding ARM-native command-line interface (CLI) tools may be a bit more difficult. [Homebrew][link-3] helps quite a bit here.

Unlike some reports, I haven't had any issues with the overall stability of macOS 11. It's been pretty rock-solid for me. I'm also finding that I don't really mind the refreshed UI in macOS 11, although it is a bit inconsistent in areas (have a look at the UI for the macOS Preview app, for example, and compare the window when the markup tools are showing versus when they are hidden).

## Summary

The new M1-based MacBook Pro offers respectable performance while also delivering amazing battery life. In my real-world usage, I haven't found it to be "incredibly faster" than Intel-based competitors, but the combination of the Apple-designed CPU plus QoS mechanisms in macOS 11 "Big Sur" provide a consistently responsive user experience and fewer instances of the "spinning beachball." I'm sure some operations are just plain faster, especially when the application is an ARM-native application (as more and more are), but I haven't personally experienced any such instances yet. Your mileage may vary, of course. All in all, I'm pretty pleased with the laptop and the user experience.

Feel free to contact me if you have any questions. I'd also love to hear impressions from other M1-based Mac users; perhaps I can publish another post with other users' feedback. You can drop me an e-mail (my address isn't too hard to find or figure out), or contact [me on Twitter][link-2] (my DMs are open).

[link-1]: https://eclecticlight.co/2021/05/17/how-m1-macs-feel-faster-than-intel-models-its-about-qos/
[link-2]: https://twitter.com/scott_lowe
[link-3]: https://brew.sh
[link-4]: https://doesitarm.com
[xref-1]: {{< relref "2018-04-13-review-lenovo-thinkpad-x1-carbon.md" >}}
