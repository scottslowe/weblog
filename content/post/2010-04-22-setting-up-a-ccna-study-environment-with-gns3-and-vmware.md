---
author: slowe
categories: Tutorial
comments: true
date: 2010-04-22T10:07:49Z
slug: setting-up-a-ccna-study-environment-with-gns3-and-vmware
tags:
- BSD
- Cisco
- IOS
- Linux
- Networking
- Ubuntu
- Virtualization
- VMware
title: Setting up a CCNA Study Environment with GNS3 and VMware
url: /2010/04/22/setting-up-a-ccna-study-environment-with-gns3-and-vmware/
wordpress_id: 1898
---

If you were following [my tweets](http://twitter.com/scott_lowe) over the last few days, you probably already know that I have been working on setting up a CCNA study environment using [Ubuntu Linux](http://www.ubuntu.com), [GNS3](http://gns3.net/), and [VMware Workstation](http://www.vmware.com/products/workstation/index.html). After a couple days of difficulties, I finally managed to make it work last night. Here are the steps that I took to make it work.

Before we start, there is the standard disclaimer: these are the steps that worked _for me_; these steps might or might not work for you, and are almost guaranteed not to work with different Linux distributions or different versions of the associated software.

Here are the software components and versions that I am using in my environment:

* Ubuntu Linux 8.04.4 LTS, 32 bit  
* GNS3 0.6.1  
* Dynamips 0.2.8-RC2  
* Dynagen 0.10.1.090807  
* VMware Workstation 7.0.1 for Linux, 32 bit

I won't go into great detail on setting up Ubuntu Linux as there are plenty of resources available for that portion of this environment. You will need to be at least vaguely familiar with the Linux command-line interface (CLI) and basic Linux commands, or you'll find this process a bit difficult.

Once you have Ubuntu Linux installed and configured appropriately, the first step is to go ahead and install some dependencies using `apt-get`:

	sudo apt-get install dynagen python-qt4

This should download and install both Dynagen and the Python-QT4 libraries. Next, you'll need to download and install GNS3 0.6.1. There are newer versions of GNS3 available, but earlier attempts to get this environment running with the newer version of GNS3 resulted in problems. Again, your results might differ. Version 0.6.1 of GNS3 is available from [the GNS3 SourceForge site](http://sourceforge.net/projects/gns-3/files/).

Once you have GNS3 downloaded, extract it into the directory of your choice (I chose to use `/opt/GNS3`).

After you've downloaded and extracted GNS3, create the following directories under the directory where GNS3 is found:

```text
<GNS3 directory>/project  
<GNS3 directory>/ios  
<GNS3 directory>/cache  
<GNS3 directory>/tmp  
<GNS3 directory>/dynamips
```

Use the `chmod` and `chown` commands as necessary to ensure that your user account has full read/write permissions all of these directories except the `dynamips` directory.

Download a copy of Dynamips (it's generally available [here](http://www.ipflow.utc.fr/blog/)), put it into the `dynamips` directory you created, and use the `chmod` command to make it executable. I also found it necessary to set the Dynamips binary's SUID bit so that it would always run as root; I know this is not best practice but I could not find any other workaround. (Without setting it SUID, GNS3 would always report an error when trying to launch Dynamips.)

Now launch GNS3 and use the Preferences in the application to set the correct path to your project directory (`<GNS3 directory>/project`) and the IOS/PIX directory (`<GNS3 directory>/ios`), the correct path to the Dynamips binary (`<GNS3 directory>/dynamips`), the correct path to the working directory (`<GNS3 directory>/tmp`), and the working directory for capture files (set it to your project directory).

At this point you should have a working GNS3 installation. You'll still need to locate IOS images to use; once you have valid IOS images, place them in the `ios` directory you created earlier and configure them within GNS3 as needed. You should then be able to create a router instance, boot it, and access the router console from within GNS3.

You _could_ stop there and have a pretty cool environment, but I wanted to go a step further. I also installed VMware Workstation 7.0.1 (I won't go into detail here, it's a pretty simple process) and then used the Virtual Network Editor to create some additional host-only networks (in addition to the default vmnet1). Again, this is well-documented already, so I won't discuss the process in any length. Where it gets interesting is in how you connect GNS3 and these host-only networks so that VMs can be incorporated into your GNS3 router topology.

Here's how you connect GNS3 and the VMware Workstation host-only networks:

1. In GNS3, add a cloud object to the topology.

2. Right-click the cloud object and select Configure.

3. On the NIO Ethernet tab in the Generic Ethernet NIO section, select one of the host-only networks (like vmnet1) and click Add. This creates a link between the cloud object and the selected host-only network.

At this point, you can attach a VM to the selected host-only network, attach a router to the cloud, and be able to pass traffic from the VM to the router. Pretty cool, huh?

What I've done so far is create a simple network with two VMs attached to two different host-only networks which are in turn connected to two different cloud objects and two different routers. Then I created a "serial WAN link" between the two routers (GNS3 won't, as far as I can tell, actually simulate WAN links with bandwidth limits and latency) and configured everything so that I could pass traffic from one VM to the second VM across the "virtual WAN". The plan is to increase the network complexity---as much as my poor little Dell laptop will allow given its limited CPU and RAM---and work through the various CCNA study guides in preparation for my exam.

One other quick note about this setup (and the reason why I chose Linux as my host platform): by setting up SSH on the Linux system (with a simple `sudo apt-get install openssh-server`), I can now SSH into the Linux host system and then use Telnet from there to access all the various routers. In addition, because I'm using [OpenBSD](http://www.openbsd.org) as the guest OS on my VMs, I can also SSH from the Linux host to the OpenBSD VMs (assuming my GNS3 network is configured correctly). I'm also thinking that there's a way I can leverage some VNC connectivity through Workstation to access the VMs as well, but I'll need to research that a bit to see how it works.

I would be remiss if I did not point out a couple of sites that were extremely helpful in getting this setup up and running. First, this site provided an [excellent overview of the GNS3 installation on Ubuntu](http://www.thenetworktechnician.com/2009/08/how-to-install-gns3-in-ubuntu-9-04/). Although the walk-through was for a newer version of Ubuntu, the instructions worked perfectly on 8.04.4 LTS. Second, this site gave me the "missing link" on [how to connect GNS3 and VMware Workstation's host-only networks](http://geexhq.com/simulating-network-labs-using-gns3-and-vmware-on-your-pc/) so that you could mix the two environments. Thank you to both sites for outstanding information!

If you are a GNS3 expert or have some additional tips or tricks to share, please add them in the comments below so that all readers can benefit. Courteous comments are always welcome.
