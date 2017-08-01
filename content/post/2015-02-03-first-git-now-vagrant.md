---
author: slowe
categories: General
comments: true
date: 2015-02-03T08:10:00Z
tags:
- Git
- Vagrant
- CLI
- Virtualization
- VMware
title: First Git, now Vagrant
url: /2015/02/03/first-git-now-vagrant/
---

When I shared [the story behind migrating the blog to Jekyll and GitHub][xref-1], I mentioned that one of the reasons for the migration was to embrace Git as a part of my regular workflow. I'd been recommending to folks that they learn and use Git, and now I needed to "walk the walk" as well as "talk the talk." This post describes another step in my effort to "walk the walk."

As the title of the post implies, this step involves the well-known tool [Vagrant][link-1]. (If you are unclear what Vagrant is or what it does, please read [my quick introduction to Vagrant][xref-2].) In the same presentation where I was recommending to folks to learn tools like Git, I was also recommending that they learn (and use, where applicable) tools like Vagrant. Once again, though, I was talking a good game but not backing it up with my actions. So, I've resolved to expand my use of Vagrant, sharing with all of my readers and followers along the way. And, because I believe that [VMware Fusion][link-2] is the most robust virtualization solution for Mac OS X, I'll be using Vagrant with VMware Fusion.

So what will I be sharing, you might ask? Well, a couple of things at least:

1. As I started to expand my use of Vagrant, I found what I felt was a shortage of base boxes for Fusion/Workstation. Therefore, one of the first things I'll be sharing are some Vagrant base boxes built specifically for the `vmware_desktop` provider (which supports both VMware Fusion and [VMware Workstation][link-3]). A couple of these are already online; you can [see the details here][link-4] and pull either of them down for use with Vagrant on your systems. (Feedback is appreciated; I'd particularly like folks to test it with VMware Workstation and versions of VMware Fusion other than 6.0.5.)

2. To make use of these base boxes, I'll also be sharing a number of different `Vagrantfiles`, each designed to help with learning something. For example, I'm looking at building a `Vagrantfile` that will build a system running a recent build of [Open vSwitch (OVS)][link-5] so as to make it easier for folks to learn how to use and work with OVS. As a follow-up to the vBrownBag session I did recently, I'll also post a `Vagrantfile` that helps build out an [etcd][link-6] cluster on [CoreOS Linux][link-7]. (Feel free to drop me an e-mail if you have any other suggestions.)

I have two goals with this effort. First, I want to make it easier for other people to learn what Vagrant is and how they can put it to use in their career. Second, I want to make it easier for people to learn other new technologies that will help arm them for success in the IT industry. Hopefully I'll be able to make some progress toward these goals.


[link-1]: http://www.vagrantup.com/
[link-2]: http://www.vmware.com/products/fusion/
[link-3]: http://www.vmware.com/products/workstation/
[link-4]: https://atlas.hashicorp.com/slowe/
[link-5]: http://openvswitch.org/
[link-6]: https://github.com/coreos/etcd/
[link-7]: https://coreos.com
[xref-1]: {{< relref "2015-01-06-the-story-behind-the-migration.md" >}}
[xref-2]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}