---
author: slowe
categories: Information
comments: true
date: 2020-01-16T16:00:00+01:00
tags:
- macOS
- Linux
- Personal
- AWS
title: Removing Unnecessary Complexity
url: /2020/01/16/removing-unnecessary-complexity/
---

Recently, I've been working to remove unnecessary complexity from my work environment. I wouldn't say that I'm going full-on minimalist (not that there's anything wrong with that), but I was beginning to feel like maintaining this complexity was taking focus, willpower, and mental capacity away from other, more valuable, efforts. Additionally, given the challenges I know lie ahead of me this year (see [here][xref-4] for more information), I suspect I'll need all the focus, willpower, and mental capacity I can get!<!--more-->

When I say "unnecessary complexity," by the way, I'm referring to added complexity that doesn't bring any real or significant benefit. Sometimes there's no getting around the complexity, but when that complexity doesn't create any value, it's unnecessary in my definition.

Primarily, this "reduction in complexity" shows up in three areas:

1. My computing environment
2. My home office setup
3. My lab resources

## My Computing Environment

Readers who have followed me for more than a couple years know that I migrated away from macOS for about 9 months in 2017 (see [here][xref-1] for a wrap-up of that effort), then again in 2018 when I joined Heptio (some details are available in [this update][xref-2]). Since switching to [Fedora][link-1] on a Lenovo X1 Carbon when I joined Heptio, I was using Linux on the desktop full-time. ([This post][xref-3] contains links to all the posts I wrote about my Linux migration.)

About a month ago, though, I switched back to macOS (macOS Mojave, specifically). Why? Although I was making Linux work for me, there was just enough "extra friction" in daily tasks to make it more of a chore than I felt it needed to be:

* E-mail connectivity to Office 365 was a challenge (my employer shut down IMAP/SMTP access to e-mail).
* Calendaring was difficult, at best, and the shared calendar for my team totally confused most solutions I tried.
* Interoperability between LibreOffice and Office 365 remained subpar (at best) for all but the most basic documents, unfortunately.

Contrast that with macOS, where in a matter of minutes I had full connectivity to e-mail, full calendar access (including the shared team calendar), and a working version of Office 365. I also had multi-lingual support working with relative ease, something with which I suspect Linux desktop environments would've struggled.

Although I had workable solutions (use a Windows 10 desktop on VMware View for some things, use the Office web applications for other aspects), these solutions still introduced what I felt was unnecessary complexity. So, to remove this unnecessary complexity, I switched back to macOS full-time.

I also switched back to iOS (I bought an iPhone 11 Pro) from Android (I'd been using a Google Pixel). Given that I was moving back into the Apple ecosystem, and given that using an Android-powered phone had made it a bit more difficult to stay in touch with my wife when I was traveling, it didn't seem to make sense to continue down that path. There was no significant benefit in exchange for the added friction.

## My Home Office Setup

In addition to simplifying my computing environment, I also set out to simplify my home office setup. For years, I'd used both a 2012-era Mac Pro (one of the classic aluminum tower systems, which I'd purchased to help with video rendering) and a laptop, and used tools like [Synergy][link-2] (to have a single shared keyboard and mouse across both systems) and [Unison][link-3] (to keep files synchronized across both systems). In reality, though, I didn't _need_ the Mac Pro as a desktop system; I really needed it more like a server system to which I could offload rendering tasks.

Around the same time I switched back to macOS, I also switched back to a single-computer setup. I no longer use Synergy to share a keyboard and mouse across multiple systems; instead, I have only my 2017 13" MacBook Pro. It uses a USB-C port replicator to connect to an external monitor (the same 34" 21:9 ultra-wide monitor I'd been using previously), external keyboard, external microphone and webcam, and USB-based headset for Zoom meetings. I use a small application named [ControlPlane][link-4] to automatically adjust settings based on whether the laptop is "docked" or not. Plug the laptop in, and it reconfigures itself; unplug it, and it automatically adapts.

The Mac Pro sits off to the side; I'm still working out details on exactly how I'll utilize it. It's still running Linux (Fedora, specifically), and will most likely continue to do so; since it isn't being used as a desktop system I see little value in moving back to macOS on that hardware. It'll probably take over tasks from an aging Mac mini as well as helping with video rendering tasks, once I figure out the specific details on the best way to make that work.

## My Lab Resources

Making mention of the aging Mac mini naturally leads me into the last area, my lab resources. This effort actually started a few years ago when I moved into my current home. Prior to that, I ran a full-fledged home lab: eight dual-socket Dell servers as hypervisors (a mix of KVM and ESXi), 10Gb and 40Gb Ethernet with multiple VLANs, multiple VMs running for a variety of purposes. When I moved into the current house, though, I did away with _all_ of it except the Mac mini and an Apple Time Capsule. Moving "up the stack" to focus on things like Linux containers and [Kubernetes][link-6] meant there wasn't as much value in running all this hardware, and why spend time managing all that complexity if I didn't really need to?

Instead, I started leveraging ephemeral instances in AWS for everything I needed when it came to "lab resources." Over the last few years, I've used that approach to strengthen and expand my skills around infrastructure-as-code. My current approach leverages [Pulumi][link-5] to manage a base set of AWS resources in multiple regions. Additional capacity is spun up on-demand, and shut down when it's no longer actively needed. Since it's almost exclusively Kubernetes I'm using, I leverage `kubeadm` and [Cluster API][link-7] to create and destroy Kubernetes clusters as needed. Yes, there are some limitations, but for the most part this arrangement has worked reasonably well.

Anyone else undertaken a similar journey? I'd be interested to hear from you, and perhaps learn of some additional ways I may be able to simplify and eliminate unnecessary complexity. Feel free to [reach out to me on Twitter][link-8], or drop me an e-mail (my e-mail address isn't hard to find, to be honest). Thanks!

[link-1]: https://getfedora.org/
[link-2]: https://symless.com/synergy
[link-3]: https://www.cis.upenn.edu/~bcpierce/unison/
[link-4]: https://www.controlplaneapp.com/
[link-5]: https://www.pulumi.com/
[link-6]: https://kubernetes.io/
[link-7]: https://github.com/kubernetes-sigs/cluster-api
[link-8]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-10-25-linux-migration-wrap-up.md" >}}
[xref-2]: {{< relref "2018-12-31-linux-migration-december-2018-progress-report.md" >}}
[xref-3]: {{< relref "2018-12-23-the-linux-migration-series.md" >}}
[xref-4]: {{< relref "2019-12-30-new-year-new-adventure.md" >}}
