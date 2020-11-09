---
author: slowe
categories: Information
comments: true
date: 2017-01-19T11:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Vagrant
- Ansible
- OVS
- VXLAN
- HyperV
- Cisco
- AWS
- VMware
- Linux
- LXC
- macOS
- Ubuntu
- Docker
title: 'Technology Short Take #76'
url: /2017/01/19/technology-short-take-76/
---

Welcome to Technology Short Take #76, the first Technology Short Take of 2017. Normally, I'd publish this on a Friday, but due to extenuating circumstances (my mother-in-law's funeral is tomorrow) I'm posting it today. Here's hoping you find something useful!

## Networking

* As of Vagrant 1.9.1, the Cumulus plugin for Vagrant is no longer needed. See [this post][link-6] for more details.
* Ivan Pepelnjak (one of my absolutely favorite folks in the IT industry) has a great post on [using regex filters in Ansible to parse text printouts from network devices][link-12]. This is a super-practical post that anyone working with network automation can put to use right away.
* Craig Matsumoto has an informative article on [why machine learning is hard to apply to networking][link-13]. I've heard David Meyer---who is quoted extensively in the article---speak several times, and most of the time his stuff is way beyond my understanding. At least now, though, I get the general direction he's heading.
* Interested in using OVS with VXLAN on Hyper-V, but _without_ OpenStack? [This article][link-15] by Alin Serdean from Cloudbase Solutions describes how it's done.
* Here's an article describing [how to establish a site-to-site VPN between a Cisco ASA and Microsoft Azure][link-21]. Anecdotally, I've also heard that it works for establishing a VPN between NSX and Azure.
* Major Hayden has a post describing [an issue with systemd-networkd][link-26] (for configuring networking) in systemd 229.

## Servers/Hardware

* I mentioned in a tweet a few weeks ago that I ordered my new corporate laptop, a Dell Latitude E7370 (which will run Ubuntu 16.04). Stay tuned for a quick review of the hardware once it arrives.

## Security

* Andrew Langhorn discusses some aspects of [limiting your attack surface][link-8] when running workloads on AWS.

## Cloud Computing/Cloud Management

* Out of the cloud(s) and [into the fog][link-1]. Interesting. (Hat tip to Kelsey Hightower for pointing this out on Twitter.)
* Luke Youngblood has a three-part series (so far) on immutable infrastructure with AWS and Ansible ([part 1][link-3], [part 2][link-4], and [part 3][link-5]). These are some really good articles with useful information on using Ansible to create/destroy AWS resources. I'm not new to this topic but still found some useful information here.
* Tomas Fojta shares a script to [reboot all hosts in vCloud Director][link-9], but to do so in a way that minimizes the impact to users/customers/tenants.
* Rowan Udell has [a quick AWS CLI command that prints an ordered list of all the resources][link-16] in a CloudFormation stack. Although Rowan doesn't specifically mention it, this command relies on tools like `tr`, `sort`, and `uniq`, so I'm guessing it will only work on OS X/Linux/*BSD systems.
* Stephane Graber has an article explaining [how to run Kubernetes in LXD][link-29]. He also has a nice post detailing the [networking functionality in LXD 2.3][link-30], which is something I need to explore.

## Operating Systems/Applications

* It's probably confirmation bias, but now that I've decided to step up my efforts to migrate from OS X to Linux (see [this post][xref-1]) I noticed this post by Nicolas about [moving from OS X to Ubuntu][link-7]. The post has a nice list of Linux replacements for popular OS X apps.
* If you've heard all this hubbub about containers but still aren't really sure what's going on, this post by Eric Chiang on [containers from scratch][link-11] will do a great job of explaining the underlying OS mechanisms that make containers possible (on Linux, at least).
* Rafael Benevides has a list of [10 things to avoid in Docker containers][link-14].
* Docker took a bit of a hit in a couple articles (perhaps more, but these were the ones that bubbled up for me) recently. First up is "The HFT Guy" with [a post on a "history of failure" with Docker in production][link-19]. Then we have a post by David Pollak that outlines his position, which is that [Docker is not ready for prime-time][link-20].
* As a counterpoint to the "Docker isn't ready" articles from the previous bullet, consider [this article][link-23]. The author (Pat Robinson, I'm guessing from the URL) shares some viewpoints on why Docker is ready for production _for his use case._ (Hat tip to John Troyer for a pointer to this article.)
* Cody Bunch has this nice tip on [enabling/disabling OS X Finder tabs][link-24].
* This post is a bit older, but still useful, I think. Zhenyun Zhuang and the LinkedIn Engineering time have a post on [not letting Linux control groups be uncontrolled][link-25]. One key takeaway from the article is that control groups (cgroups) don't reserve memory, so it's still quite possible for memory contention to occur.
* Trishna Guha has an article on [using Docker remotely on Atomic Host][link-26] (something I struggled with myself).
* Want to get all your data out of Evernote following the revelation that their employees have access to your data? Check [this][link-28] out.

## Storage

I don't have anything this time, but I'll keep alert for links or articles to include next time.

## Virtualization

* Feidhlim O'Leary has a post warning of [a potential concern with password expiration on the VCHA user account][link-10] (this is in the vSphere 6.5 release). 
* Simon Sharwood of El Reg has the details on [a Windows Server 2016 Hotfix for Update Rollup 1][link-22] (how's that for a mouthful?). Apparently, the rollup introduces issues with live migrations, and the hotfix addresses those issues.

## Career/Soft Skills

* I briefly entertained the idea of changing my Twitter handle (finally decided against it, for various reasons), but thought that [this article][link-2] provided by one of my followers (thanks Paul!) might be useful to others.
* Andy Callow has [a nice summary of pioneer/settler/town planner (PST) articles and implementations][link-17]. If you're a fan of this three-mode model or just want to learn more, have a look at Andy's article. Andy also references [this article][link-18] by Neil Perkin, which I found very useful as well.

OK, that's it for this time around. I have a ton more content I'd love to include, but this post is already packed to the gills as it is. I guess I'll just have to save it for the next one. Until then, enjoy!



[link-1]: https://www.openfogconsortium.org/
[link-2]: https://thinkerbit.com/articles/change-twitter-username
[link-3]: http://vcdxpert.com/?p=105
[link-4]: http://vcdxpert.com/?p=148
[link-5]: http://vcdxpert.com/?p=193
[link-6]: https://getsatisfaction.cumulusnetworks.com/cumulus/topics/cumulus-vx-and-vagrant-cumulus-plugin
[link-7]: https://nicolas.perriault.net/code/2016/from-osx-to-ubuntu/
[link-8]: https://www.awsadvent.com/2016/12/15/limiting-your-attack-surface-in-the-aws-cloud/
[link-9]: https://fojta.wordpress.com/2016/03/18/reboot-all-hosts-in-vcloud-director/
[link-10]: https://haveyoutriedreinstalling.com/2017/01/16/caution-vcha-user-password/
[link-11]: https://ericchiang.github.io/post/containers-from-scratch/
[link-12]: http://automation.ipspace.net/Example:Parsing_Text_Printouts_within_Ansible_Playbooks
[link-13]: https://www.sdxcentral.com/articles/news/machine-learning-hard-apply-networking/2017/01/
[link-14]: https://developers.redhat.com/blog/2016/02/24/10-things-to-avoid-in-docker-containers/
[link-15]: http://superuser.openstack.org/articles/manage-hyper-v-open-vswitch/
[link-16]: http://blog.rowanudell.com/cloudformation-stack-resources-summary/
[link-17]: https://medium.com/@andy.callow.hscic/exploring-pioneers-settlers-and-town-planners-239be83ae37c#.srcovq6sf
[link-18]: http://www.onlydeadfish.co.uk/only_dead_fish/2016/04/pioneers-settlers-and-town-planners.html
[link-19]: https://thehftguy.com/2016/11/01/docker-in-production-an-history-of-failure/
[link-20]: https://blog.goodstuff.im/docker_not_prime_time
[link-21]: https://supportforums.cisco.com/blog/12926156/site-site-vpn-between-cisco-asa-and-microsoft-azure-virtual-network-arm
[link-22]: http://www.theregister.co.uk/2016/12/15/windows_server_2016s_vm_migration_tools_broken_by_a_patch/
[link-23]: http://patrobinson.github.io/2016/11/05/docker-in-production/
[link-24]: http://blog.codybunch.com/2017/01/18/Disabling-Enabling-OSX-Finder-Tabs/
[link-25]: https://engineering.linkedin.com/blog/2016/08/don_t-let-linux-control-groups-uncontrolled
[link-26]: https://major.io/2017/01/15/systemd-networkd-on-ubuntu-16-04-lts-xenial/
[link-27]: https://fedoramagazine.org/use-docker-remotely-atomic-host/
[link-28]: https://github.com/shawndaniel/evernote-exporter/
[link-29]: https://www.stgraber.org/2017/01/13/kubernetes-inside-lxd/
[link-30]: https://www.stgraber.org/2016/10/27/network-management-with-lxd-2-3/
[xref-1]: {{< relref "2016-12-16-linux-migration-initial-progress-report.md" >}}
