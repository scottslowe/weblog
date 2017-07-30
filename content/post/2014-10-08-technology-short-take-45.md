---
author: slowe
categories: Information
comments: true
date: 2014-10-08T09:00:00Z
slug: technology-short-take-45
tags:
- Docker
- Hardware
- HyperV
- Intel
- Linux
- Networking
- NSX
- OpenStack
- Security
- SSL
- Storage
- Virtualization
- vSphere
- VXLAN
- Certification
- SSL
title: 'Technology Short Take #45'
url: /2014/10/08/technology-short-take-45/
wordpress_id: 3540
---

Welcome to Technology Short Take #45. As usual, I've gathered a collection of links to various articles pertaining to data center-related technologies for your enjoyment. Here's hoping you find something useful!

## Networking

* Cormac Hogan has a list of [a few useful NSX troubleshooting tips](http://cormachogan.com/2014/09/12/some-useful-nsx-troubleshooting-tips/).

* If you're not really a networking pro and need a "gentle" introduction to VXLAN, [this post](http://professionalvmware.com/2014/09/vxlan-concepts-operation-and-implementation-1-2-by-davidmirror/) might be a good place to start.

* Also along those lines---perhaps you're a VMware administrator who wants to branch into networking with NSX, or you're a networking guru who needs to learn more about how this NSX stuff works. vBrownBag has been running a VCP-NV series covering various objectives from the VCP-NV exam. Check them out--[objective 1](http://professionalvmware.com/2014/09/4869/), [objective 2](http://professionalvmware.com/2014/09/vbrownbag-follow-up-vmware-vcp-nv-objective-2-with-ross-wynne-rosswynne/), [objective 3](http://professionalvmware.com/2014/09/vbrownbag-follow-up-vmware-vcp-nv-objective-3-with-rene-van-den-bedem-vcdx133/), and [objective 4](http://professionalvmware.com/2014/10/vbrownbag-follow-up-vmware-vcp-nv-objective-4-with-paul-mcsharry-pmcsharry/) have been posted so far.

## Servers/Hardware

* I'm going to go out on a limb and make a prediction: In a few years time (let's say 3-5 years), Intel SGX (Software Guard Extensions) will be regarded as important if not more important than the virtualization extensions. What is Intel SGX, you ask? See [here](https://software.intel.com/en-us/blogs/2013/09/26/protecting-application-secrets-with-intel-sgx), [here](https://software.intel.com/en-us/blogs/2014/06/02/intel-sgx-for-dummies-part-2), and [here](https://software.intel.com/en-us/blogs/2014/09/01/intel-sgx-for-dummies-part-3) for a breakdown of the SGX design objectives. Let's be real---the ability for an application to protect itself (and its data) from rogue software (including a compromised or untrusted operating system) is **huge.**

## Security

* CloudFlare (disclaimer: I am a CloudFlare customer) recently [announced Keyless SSL](https://blog.cloudflare.com/announcing-keyless-ssl-all-the-benefits-of-cloudflare-without-having-to-turn-over-your-private-ssl-keys/), a technique for allowing organizations to take advantage of SSL offloading **without** relinquishing control of private keys. CloudFlare followed that announcement with a [nitty gritty technical details post](https://blog.cloudflare.com/keyless-ssl-the-nitty-gritty-technical-details/) that describes how it works. I'd recommend reading the technical post just to get a good education on how encryption and TLS work, even if you're not a CloudFlare customer.

## Cloud Computing/Cloud Management

* William Lam spent some time working with some "new age" container cluster management tools (specifically, govmomi, govc CLI, and Kubernetes on vSphere) and documented his experience [here](http://www.virtuallyghetto.com/2014/09/govmomi-vsphere-sdk-for-go-govc-cli-kubernetes-on-vsphere-part-1.html) and [here](http://www.virtuallyghetto.com/2014/09/how-to-deploy-a-kubernetes-cluster-on-vsphere.html). Excellent stuff!

* YAKA (Yet Another Kubernetes Article), this time looking at [Kubernetes on CoreOS on OpenStack](http://www.cloudssky.com/en/blog/Kubernetes-CoreOS-Cluster-On-Top-Of-OpenStack/). (How's that for buzzword bingo?)

* This [analytical evaluation of Kubernetes](http://www.symantec.com/connect/blogs/google-kubernetes-analytical-evaluation) might be helpful as well.

* Stampede.io looks interesting; I got a chance to see it live at the recent DigitalOcean-CoreOS meetup in San Francisco. Here's [the Stampede.io announcement post](http://www.ibuildthecloud.com/blog/2014/08/21/announcing-stampede-dot-io-a-hybrid-iaas-slash-docker-orchestation-platform-running-on-coreos/).

## Operating Systems/Applications

* Trying to wrap your head around the concept of "microservices"? Here's a write-up that attempts to provide [an introduction to microservices](http://nirmata.com/2014/07/cloud-native-software-microservices/). An earlier blog post on [cloud native software](http://nirmata.com/2014/05/cloud-native-software-key-characteristics/) is pretty good, too.

* Here's a very nice [collection of links about Docker](http://www.nkode.io/2014/08/24/valuable-docker-links.html), ranging from how to use Docker to how to use the Docker API and how to containerize your application (just to name a few topics).

* Here'a a great pair of articles ([part 1](http://www.activestate.com/blog/2014/08/microservices-and-paas-part-i) and [part 2](http://www.activestate.com/blog/2014/08/microservices-and-paas-part-ii)) on microservices and Platform-as-a-Service (PaaS). This is really good stuff, especially if you are trying to expand your boundaries learning about cloud application design patterns.

* This article by CenturyLink Labs---which has been doing some nice stuff around Docker and containers---talks about [how to containerize your legacy applications](http://www.centurylinklabs.com/how-to-migrate-legacy-applications-into-docker-containers/).

* Here's a decent write-up on [comparing LXC and Docker](http://www.flockport.com/lxc-vs-docker/). There are also some decent LXC-specific articles on the site as well (see the sidebar).

* Service registration (and discovery) in a micro-service architecture can be challenging. Jeff Lindsay is attempting to help address some of the challenges with Registrator; more information is available [here](http://progrium.com/blog/2014/09/10/automatic-docker-service-announcement-with-registrator/).

* Unlike a lot of Docker-related blog posts, this post by RightScale on [combining VMs and containers for better cloud portability](http://www.rightscale.com/blog/cloud-management-best-practices/docker-vs-vms-combining-both-cloud-portability-nirvana) is a well-written piece. The pros and cons of using containers are discussed fairly, without hype.

* Single-process containers or multi-process containers? [This site](http://phusion.github.io/baseimage-docker/) presents a convincing argument for multi-process containers; have a look.

* Tired of hearing about containers yet? Oh, come on, you know you love them! You love them so much you want to run them on your OS X laptop. Well, read [this post](http://viget.com/extend/how-to-use-docker-on-os-x-the-missing-guide) for all the gory details.

## Storage

* The storage aspect of Docker isn't typically discussed in a lot of detail, other than perhaps focusing on the need for persistent storage via Docker volumes. However, [this article](http://developerblog.redhat.com/2014/09/30/overview-storage-scalability-docker/) from Red Hat does a great job (in my opinion) of exploring storage options for Docker containers and how these options affect performance and scalability. Looks like OverlayFS is the clear winner; it will be great when OverlayFS is in the upstream kernel and supported by Docker. (Oh, and if you're interested in more details on the default device mapper backend, see [here](https://github.com/docker/docker/blob/master/daemon/graphdriver/devmapper/README.md).)

* [This](http://storagegaga.com/technology-prowess-of-riverbed-steelfusion/) is a nice write-up on Riverbed SteelFusion, aka "Granite."

## Virtualization

* Azure Site Recovery (ASR) is similar to vCloud Air's Disaster Recovery service, though obviously tailored toward Hyper-V and Windows Server (which is perfectly fine for organizations that are using Hyper-V and Windows Server). To help with the setup of ASR, the Azure team has a write-up on [the networking infrastructure setup for Microsoft Azure as a DR site](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/).

* PowerCLI in the vSphere Web Client, eh? Interesting. See [Alan Renouf's post](http://www.virtu-al.net/2014/09/16/powercli-vsphere-web-clientannouncing-poweractions/) for full details.

* PernixData recently released version 2.0 of FVP; Frank Denneman has all the details [here](http://frankdenneman.nl/2014/10/01/pernixdata-fvp-2-0-released/).

That's it for this time, but be sure to visit again for future episodes. Until then, feel free to start (or join in) a discussion in the comments below. All courteous comments are welcome!
