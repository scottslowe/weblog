---
author: slowe
categories: Tutorial
comments: true
date: 2013-08-22T10:25:45Z
slug: learning-nvp-part-4-adding-hypervisors-to-nvp
tags:
- KVM
- Linux
- Networking
- Nicira
- NVP
- OVS
- Ubuntu
- Virtualization
title: 'Learning NVP, Part 4: Adding Hypervisors to NVP'
url: /2013/08/22/learning-nvp-part-4-adding-hypervisors-to-nvp/
wordpress_id: 3251
---

This is part 4 of the Learning NVP blog series. Just to quickly recap what's happened so far, in [part 1][1] I provided the high-level architecture of NVP and discussed the role of the components in broad terms. In [part 2][2], I focused on the NVP controllers, showing you how to build/configure the NVP controllers and establish a controller cluster. [Part 3][3] focused on NVP Manager, which allowed us to perform a few additional NVP configuration tasks. In this post, I'll show you how to add hypervisors to NVP so that you can turn up your first logical network.

## Assumptions

In this post, I'm using Ubuntu 12.04.2 LTS with the KVM hypervisor. I'm assuming that you've already gone through the process of getting KVM installed on your Linux host; if you need help with that, a quick Google search should turn up plenty of "how to" articles (it's basically a `sudo apt-get install kvm` operation). If you are using a different Linux distribution or a different hypervisor, the commands you'll use as well as the names of the prerequisite packages you'll need to install will vary slightly. Please keep that in mind.

## Installing Prerequisites

To get a version of [libvirt](http://libvirt.org/) that supports [Open vSwitch (OVS)](http://openvswitch.org/), you'll want to enable the Ubuntu Cloud Archive. The Ubuntu Cloud Archive is technically intended to allow users of Ubuntu LTS releases to install newer versions of OpenStack and its dependent packages (including libvirt). Instructions for enabling and using the Ubuntu Cloud Archive are found [here](https://wiki.ubuntu.com/ServerTeam/CloudArchive). However, I've found using the Ubuntu Cloud Archive to be an easy way to get a newer version of libvirt (version 1.0.2) on Ubuntu 12.04 LTS.

Once you get the Ubuntu Cloud Archive working, go ahead and install libvirt:

    sudo apt-get install libvirt-bin

Next, go ahead and install some prerequisite packages you'll need to get OVS installed and working:

    sudo apt-get install dkms make libc6-dev

Now you're ready to install OVS.

## Installing OVS

Once your hypervisor node has the appropriate prerequisites installed, you'll need to install an NVP-specific build of OVS. This build of OVS is identical to the open source build in every way _except_ that it includes the ability to create STT tunnels and includes some extra NVP-specific utilities for integrating OVS into NVP. For Ubuntu, this NVP-specific version of OVS is distributed as a compressed tar archive. First, you'll need to extract the files out like this:

    tar -xvzf <file name>

This will extract a set of Debian packages. For ease of use, I recommend moving these files into a separate directory. Once the files are in their own directory, you would install them like this:

    cd <directory where the files are stored>
    sudo dpkg -i *.deb

Note that if you don't install the prerequisites listed above, the installation of the DKMS package (for the OVS kernel datapath) will fail. Trying to then run `apt-get install <package list>` at that point will also fail; you'll need to run `apt-get -f install`. This will "fix" the broken packages and allow the DKMS installation to proceed (which it will do automatically).

These particular OVS installation packages do a couple of different things:

* They install OVS (of course), including the kernel module.

* They automatically create and configure something called the _integration bridge_, which I'll describe in more detail in a few moments.

* They automatically generate some self-signed certificates you'll need later.

Now that you have OVS installed, you're ready for the final step: to add the hypervisor to NVP.

## Adding the Hypervisor to NVP

For this process, you'll need access to NVP Manager as well as SSH access to the hypervisor. I strongly recommend using the same system for access to both NVP Manager and the hypervisor via SSH, as this will make things easier.

Adding the hypervisor to NVP is a two-step process. First, you'll configure the hypervisor; second, you'll configure NVP. Let's start with the hypervisor.

### Verifying the Hypervisor Configuration

Before you can add the hypervisor to NVP, you'll need to be sure it's configured correctly so I'll walk you through a couple of verification steps. NVP expects there to be an _integration bridge_, an OVS bridge that it will control. This integration bridge will be separate from any other bridges configured on the system, so if you have a bridge that you're using (or will use) outside of NVP this will be separate. I'll probably expand upon this in more detail in a future post, but for now let's just verify that the integration bridge exists and is configured properly.

To verify the present of the integration bridge, use the command `ovs-vsctl show` (you might need to use `sudo` to get the appropriate permissions). The output of the command should look something like this:

Note the bridge named "br-int"---this is the default name for the integration bridge. As you can see by this output, the integration bridge does indeed exist. However, you must also verify that the integration bridge is configured appropriately. For that, you'll use the `ovs-vsctl list bridge br-int`, which will produce output that looks something like this:

Now there's a lot of goodness here ("Hey, look---NetFlow support! Mirrors! sFlow! STP support even!"), but try to stay focused. I want to draw your attention to the `external_ids` line, where the value "bridge-id" has been set to "br-int". This is exactly what you want to see, and I'll explain why in just a moment.

One final verification step is needed. NVP uses self-signed certificates to authenticate hypervisors, so you'll need to be sure that the certificates have been generated (they should have been generated during the installation of OVS). You can verify by running `ls -la /etc/openvswitch` and looking for the file "ovsclient-cert.pem". If it's there, you should be good to go.

Next, you'll need to do a couple of things in NVP Manager.

### Create a Transport Zone

Before you can actually add the hypervisor to NVP, you first need to ensure that you have a transport zone defined. I'll assume that you don't and walk you through creating one.

1. In NVP Manager (log in if you aren't already logged in), select Network Components > Transport Zones.

2. In the Network Components Query Results, you'll probably see no transport zones listed. Click Add.

3. In the Create Transport Zone dialog, specify a name for the transport zone, and optionally add tags (tags are used for searching and sorting information in NVP Manager).

4. Click Save.

That's it, but it's an important bit. I'll explain more in a moment.

### Add the Hypervisor

Now that you've verified the hypervisor configuration and created a transport zone, you're ready to add the hypervisor to NVP. Here's how.

1. Log into NVP Manager, if you aren't already, and click on Dashboard in the upper left corner.

2. In the Summary of Transport Components section, click the Add button on the line for Hypervisors.

3. Ensure that Hypervisor is listed in the Transport Node Type drop-down list, then click Next.

4. Specify a display name (I use the hostname of the hypervisor itself), and---optionally---add one or more tags. (Tags are used for searching/sorting data in NVP Manager.) Click Next.

5. In the Integration Bridge ID text box, type "br-int". (Do you know why? It's not because that's the name of the integration bridge. It's because that's the value set in the `external_ids` section of the integration bridge.) Be sure that Admin Status Enabled is checked, and optionally you can check Tunnel Keep-Alive Spray. Click Next to continue.

6. Next, you need to authenticate the hypervisor to NVP. This is a multi-step process. First, switch over to the SSH session with the hypervisor (you still have it open, right?) and run `cat /etc/openvswitch/ovsclient-cert.pem`. This will output the contents of the OVS client certificate to the screen. Copy everything between the `BEGIN CERTIFICATE` and the `END CERTIFICATE` lines, including those lines.

7. Flip back over to NVP Manager. Ensure that Security Certificate is listed, then paste the clipboard contents into the Security Certificate box. The red X on the left of the dialog box should change to a green check mark, and you can click Next to continue.

8. The final step is creating a transport connector. Click Add Connector.

9. In the Create Transport Connector dialog, select STT as the Transport Type.

10. Select the transport zone you created earlier, if it isn't already populated in the Transport Zone UUID drop-down.

11. Specify the IP address of the interface on the hypervisor that will be used as the source for all tunnel traffic. This is generally _not_ the management interface. Click OK.

12. Click Save.

This should return you to the NVP Manager Dashboard, where you'll see the Hypervisors line in the Summary of Transport Components go from 0 to 1 (both in the Registered and the Active columns). You can refresh the display of that section only by clicking the little circular arrow button. You should also see an entry appear in the Hypervisor Software Version Summary section. This screen shot shows you what the dashboard would look like after adding 3 hypervisors:

[![NVP Manager dashboard](/public/img/nvp-dash-with-hv-small.png)](/public/img/nvp-dash-with-hv-large.png)

(Side note: This dashboard shows a gateway and service node also added, though I haven't discussed those yet. It also shows a logical switch and logical switch ports, though I haven't discussed those either. Be patient---they're coming. I can only write so fast.)

Congratulations---you've added your first hypervisor to NVP! In the next part, I'm going to take a moment to remind readers of a few concepts that I've covered in the past, and show how those concepts relate to NVP. From there, we'll pick back up with adding our first logical network and establishing connectivity between VMs on different hypervisors.

Until that time, feel free to post any questions, thoughts, corrections, or clarifications in the comments below. Please disclose vendor affiliations, where appropriate, and---as always---I welcome all courteous comments.

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
