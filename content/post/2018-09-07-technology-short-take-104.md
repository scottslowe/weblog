---
author: slowe
categories: Information
comments: true
date: 2018-09-07T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- AWS
- VMware
- Linux
- CLI
- KVM
title: 'Technology Short Take 104'
url: /2018/09/07/technology-short-take-104/
---

Welcome to Technology Short Take 104! For many of my readers, VMworld 2018 in Las Vegas was "front and center" for them since the last Tech Short Take. Since I wasn't attending the conference, I won't try to aggregate information from the event; instead, I'll focus on including some nuggets you may have missed amidst all the noise.<!--more-->

## Networking

* Michael Kashin takes a look at AppSwitch and [likens it to "serverless SDN."][link-14]
* Nick Buraglio [exposes the different kinds of NAT][link-15]---helpful if you're new to networking and want to better understand NAT and the nuances involved.

## Servers/Hardware

* Greg Schulz discusses new Power9-based systems announced by IBM; see [his post][link-11]. Normally I wouldn't be too interested in non-x86 stuff, as it seems like x86 is ascendant. However, given the rise of all the various speculative execution attacks, and given the recent interest in ARM platforms (can't recall if they are affected by the speculative execution attacks), is a revival of non-x86 platforms in the works?

## Security

Nothing this time around, but I'll stay alert for items to include next time!

## Cloud Computing/Cloud Management

* [This is just awesome.][link-2]
* It's fairly simplistic, but it could be useful nevertheless: the [kube-resource-report project][link-5] offers some insight into the costing behind Kubernetes clusters on AWS.
* One of the interesting announcements to come out of the recent VMworld 2018 conference in Las Vegas is also described in [this post on the AWS News Blog][link-9]: Amazon RDS on VMware. I suppose it's a win-win deal for both VMware and AWS (else why bother?), though the long-term implications of having AWS' policy layer on top may come back to bite VMware. Time will tell.
* I wouldn't say [noboby noticed][link-12], Maish. I called this out when I saw the announcement as well. Combine this with other recent moves (see previous bullet), and it sure seems that AWS is out for the "private cloud" use case (which isn't really a private cloud, but that's an entirely different discussion).

## Operating Systems/Applications

* I've made no secret of the fact that I'm a Terraform fan, and I recently uncovered a great series by Ned Bellavance called [Function of the Day (FotD)][link-1], in which he discusses a Terraform interpolation function each day. This is a really useful learning resource.
* Want to become more of a CLI wizard? Start [here][link-3].
* If you're still in need of an introductory article on building your own Docker image, [this one][link-4] may help you out.
* Here's a great tutorial (sent in by a reader, thanks Mike!) on [building your own Bash completion script][link-6].
* Based partly on [this article][link-7], I've started using `fzf` and `fd` on my Fedora laptop. I must admit that these are both pretty cool tools---I'd recommend giving them a look if you spend a lot of time at a prompt. If you're interested in possibly giving `fzf` a try, you might have a look [here][link-17] and [here][link-18] as well.
* Microsoft is adding a third Windows container image, in addition to the existing Nano Server and Windows Server Core images. More details and an explanation of why an additional image is needed are in [this blog post][link-8].

## Storage

* An 8TB NVMe drive? Holy moly! Chris Evans has [more details][link-10].

## Virtualization

* Curtis (don't have the last name since it's not listed anywhere) has a write-up on [using cloud images with KVM][link-16].
* I said I wouldn't try to aggregate the news from VMworld, but I couldn't resist throwing in at least one "wrap up" from the show. Eric Siebert's post-conference thoughts are [here][link-19]; he does a pretty good job of capturing the major announcements. I also saw [this recap][link-20] by Rachel Stevens; she's pretty bullish about the RDS on VMware announcement (as a lot of folks are).

## Career/Soft Skills

* David Gee [shares his "automation learning charter,"][link-13] and the post contains some useful tidbits for those of us who are embracing lifelong learning and growth.

That's all for today; it's a bit shorter than usual but hopefully there are still some useful tidbits in there. Feel free to [hit me up on Twitter][link-21] with comments or feedback!

[link-1]: https://nedinthecloud.com/tag/fotd/
[link-2]: https://carolynvanslyck.com/blog/2017/10/my-little-cluster/
[link-3]: https://github.com/jlevy/the-art-of-command-line
[link-4]: https://ipnet.xyz/2018/07/how-to-create-your-own-docker-image/
[link-5]: https://github.com/hjacobs/kube-resource-report
[link-6]: https://iridakos.com/tutorials/2018/03/01/bash-programmable-completion-tutorial.html
[link-7]: https://remysharp.com/2018/08/23/cli-improved
[link-8]: https://blogs.technet.microsoft.com/virtualization/2018/06/27/insider-preview-windows-container-image/
[link-9]: https://aws.amazon.com/blogs/aws/in-the-works-amazon-rds-on-vmware/
[link-10]: https://blog.architecting.it/samsung-8tb-nf1-nvme-ssd/
[link-11]: https://storageioblog.com/ibm-announces-new-power9-processor-based-e950-e980-server-systems/
[link-12]: https://technodrone.blogspot.com/2018/07/the-aws-world-shook-and-nobody-noticed.html
[link-13]: http://ipengineer.net/2018/08/automation-learning-approach/
[link-14]: https://networkop.co.uk/post/2018-05-29-appswitch-sdn/
[link-15]: https://www.forwardingplane.net/2018/07/trouble-with-tribbles-errr-nat/
[link-16]: http://serverascode.com/2018/06/26/using-cloud-images.html
[link-17]: https://monades.roperzh.com/weekly-command-fuzzy-finding-everything-with-fzf/
[link-18]: https://mike.place/2017/fzf-fd/
[link-19]: http://vsphere-land.com/news/my-thoughts-and-observations-on-vmworld-2018.html
[link-20]: https://redmonk.com/rstephens/2018/09/06/impressions-from-vmworld/
[link-21]: https://twitter.com/scott_lowe
