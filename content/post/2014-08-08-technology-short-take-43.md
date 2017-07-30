---
author: slowe
categories: Information
comments: true
date: 2014-08-08T09:00:00Z
slug: technology-short-take-43
tags:
- Automation
- BSD
- Docker
- Fusion
- KVM
- Linux
- Networking
- NSX
- OpenFlow
- OpenStack
- OVS
- Puppet
- Security
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #43'
url: /2014/08/08/technology-short-take-43/
wordpress_id: 3484
---

Welcome to Technology Short Take #43, another episode in my irregularly-published series of articles, links, and thoughts from around the web, focusing on data center technologies like networking, virtualization, storage, and cloud computing. Here's hoping you find something useful.

## Networking

* Jason Edelman recently took [a look at Docker networking](http://www.jedelman.com/home/docker-networking). While Docker is receiving a great deal of attention, I have to say that I feel Docker networking is a key area that hasn't received the amount of attention that it probably needs. It would be great to see Docker get support for connecting containers directly to Open vSwitch (OVS), which is generally considered the _de facto_ standard for networking on Linux hosts.

* Ivan Pepelnjak asks the question, ["Is OpenFlow the best tool for overlay virtual networks?"](http://blog.ipspace.net/2014/06/is-openflow-best-tool-for-overlay.html) While so many folks see OpenFlow as the answer regardless of the question, Ivan takes a solid look at whether there are better ways of building overlay virtual networks. I especially liked one of the last statements in Ivan's post: "Wouldn't it be better to keep things simple instead of introducing yet-another less-than-perfect abstraction layer?"

* Ed Henry tackles the idea of [abstraction vs. automation](https://networkn3rd.wordpress.com/2014/05/28/abstraction-vs-automation/) in a fairly recent post. It's funny---I think Ed's post might actually be a response to a Twitter discussion that I started about the value of the abstractions that are being implemented in Group-based Policy (GBP) in OpenStack Neutron. Specifically, I was asking if there was value in creating an entirely new set of abstractions when it seemed like automation might be a better approach. Regardless, Ed's post is a good one---the decision isn't about one versus the other, but rather recognizing, in Ed's words, "abstraction will ultimately lead to easier automation." I'd agree with that, with one change: the _right_ abstraction will lead to easier automation.

* Jason Horn provides an example of [how to script NSX security groups](http://virtuallygone.wordpress.com/2014/07/11/scripting-nsx-security-groups/).

* Interested in setting up overlays using Open vSwitch (OVS)? Then check out this article from the ever-helpful Brent Salisbury on [setting up overlays on OVS](http://networkstatic.net/setting-overlays-open-vswitch/).

* Another series on VMware NSX has popped up, this time from Jon Langemak. Only two posts so far (but very thorough posts), one on [setting up VMware NSX](http://www.dasblinkenlichten.com/working-with-vmware-nsx-logical-networking/) and another on [logical networking with VMware NSX](http://home.nuug.no/~peter/pf/en/bruteforce.html).

## Servers/Hardware

Nothing this time around, but I'll keep my eyes open for more content to include next time.

## Security

* Someone mentioned I should consider using `pfctl` and its ability to automatically block remote hosts exceeding certain connection rate limits. See [here](http://home.nuug.no/~peter/pf/en/bruteforce.html) for details.

* Bromium published [some details](http://labs.bromium.com/2014/07/31/remote-code-execution-on-android-devices/) on a Android security flaw that's worth reviewing.

## Cloud Computing/Cloud Management

* Want to add some Docker to your vCAC environment? [This post](http://www.vmtocloud.com/docker-as-a-service-in-vcac-part-1/) provides more details on how it is done. Kind of cool, if you ask me.

* I am rapidly being pulled "higher" up the stack to look at tools and systems for working with distributed applications across clusters of servers. You can expect to see some content here soon on topics like [fleet](https://github.com/coreos/fleet), [Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes), [Mesos](https://mesos.apache.org), and others. Hang on tight, this will be an interesting ride!

## Operating Systems/Applications

* A fact that I think is sometimes overlooked when discussing Docker is access to the Docker daemon (which, by default, is accessible only via UNIX socket---and therefore accessible locally only). This post by Adam Stankiewicz tackles [configuring remote TLS access to Docker](http://sheerun.net/2014/05/17/remote-access-to-docker-with-tls/), which addresses that problem.

* [CoreOS](https://coreos.com) is a pretty cool project that takes a new look at how Linux distributions should be constructed. I'm kind of bullish on CoreOS, though I haven't had nearly the time I'd like to work with it. There's a lot of potential, but also some gotchas (especially right now, before a stable product has been released). The fact that CoreOS takes a new approach to things means that you might need to look at things a bit differently than you had in the past; [this post](https://medium.com/coreos-linux-for-massive-server-deployments/coreos-logging-to-remote-destinations-defb984185c5) tackles one such item (pushing logs to a remote destination).

* Speaking of CoreOS: here's how to [test drive CoreOS from your Mac](http://www.linuxjedi.co.uk/2014/06/test-drive-of-coreos-from-mac.html).

* I think I may have mentioned this before; if so, I apologize. It seems like a lot of folks are saying that Docker eliminates the need for configuration management tools like Puppet or Chef. Perhaps (or perhaps not), but in the event you need or want to combine Puppet with Docker, a good place to start is this article by James Turnbull (formerly of Puppet, now with Docker) on [building Puppet-based applications inside Docker](http://puppetlabs.com/blog/building-puppet-based-applications-inside-docker).

* Here's a tutorial for [running Docker on CloudSigma](https://www.cloudsigma.com/2014/07/21/how-to-run-docker-on-cloudsigma-with-cloudinit/).

## Storage

* It's interesting to watch the storage industry go through the same sort of discussion around what "software-defined" means as the networking industry has gone through (or, depending on your perspective, is still going through). A few articles highlight this discussion: [this one](http://griffithscorner.wordpress.com/2014/05/16/the-problem-with-sds-under-cinder/) by John Griffith (Project Technical Lead [PTL] for OpenStack Cinder), [this response](http://virtualgeek.typepad.com/virtual_geek/2014/06/what-is-sds-battle-carddecoder.html) by Chad Sakac, [this response](http://theruddyduck.typepad.com/theruddyduck/2014/05/openstack-cinder-and-software-defined-storage-sds.html) by the late Jim Ruddy, [this reply](http://cloudarchitectmusings.com/2014/06/17/the-problem-sds-under-openstack-cinder-solves/) by Kenneth Hui, and finally John's response in [part 2](http://griffithscorner.wordpress.com/2014/06/17/the-problem-with-sds-under-cinder-part-2/).

## Virtualization

* The ability to run nested hypervisors is the primary reason I still use VMware Fusion on my laptop instead of switching to VirtualBox. In this post Cody Bunch talks about [how to use Vagrant to configure nested KVM on VMware Fusion](http://openstack.prov12n.com/enabling-nested-kvm-in-devstack-on-fusion/) for using things like DevStack.

* A few different folks in the VMware space have pointed out the [VMware OS Optimization Tool](https://labs.vmware.com/flings/vmware-os-optimization-tool), a tool designed to help optimize Windows 7/8/2008/2012 systems for use with VMware Horizon View. Might be worth checking out.

* The VMware PowerCLI blog has a nice three part series on working with Customization Specifications in PowerCLI ([part 1](http://blogs.vmware.com/PowerCLI/2014/05/working-customization-specifications-powercli-part-1.html), [part 2](http://blogs.vmware.com/PowerCLI/2014/06/working-customization-specifications-powercli-part-2.html), and [part 3](http://blogs.vmware.com/PowerCLI/2014/06/working-customization-specifications-powercli-part-3.html)).

* Jason Boche has a great collection of information regarding [vSphere HA and PDL](http://www.boche.net/blog/index.php/2014/07/14/yet-another-blog-post-about-vsphere-ha-and-pdl/). Definitely be sure to give this a look.

That's it for this time around. Feel free to speak up in the comments and share any thoughts, clarifications, corrections, or other ideas. Thanks for reading!
