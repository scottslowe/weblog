---
author: slowe
categories: Explanation
comments: true
date: 2013-12-05T09:00:00Z
slug: learning-nvp-part-8-an-update-on-the-nvp-to-nsx-transition
tags:
- Networking
- NSX
- NVP
- Virtualization
- VMware
title: 'Learning NVP, Part 8: An Update on the NVP to NSX Transition'
url: /2013/12/05/learning-nvp-part-8-an-update-on-the-nvp-to-nsx-transition/
wordpress_id: 3379
---

In [part 7][1] of the Learning NVP series, I mentioned that I was planning to transition this series from NVP to NSX through an upgrade. I had an existing NVP installation running (all virtually) inside an OpenStack cloud, and I would just upgrade that to NSX 4.0.0. Here's a quick update on that plan and the NVP-to-NSX transition.

As I mentioned, I have an installation of NVP 3.1.1 running successfully in a nested (virtualized) environment. (Yes, it _is_ possible to run all of NVP completely virtualized, though we don't support that for production environments.) Starting with NVP 3.1._x_, NVP offered an "Update Coordinator" that coordinated and orchestrated the upgrade of the various components within an NVP domain. Since I was running NVP 3.1.1, I could just use the Update Coordinator to upgrade my installation and walk you (the readers) through the process along the way.

Using the Update Coordinator (which is built into NVP Manager), an NVP upgrade would typically look something like this:

* You'd log into NVP Manager and go to the Update Coordinator screen.

* If you hadn't already, you'd upload the update files (appliance update files and OVS update files) to NVP Manager.

* Once all the update files were uploaded, you'd select the version to which you're upgrading and kick it off.

* NVP Manager itself is upgraded first.

* Next, the Update Coordinator pushes out the appliance update files (sometimes called NUB files because of their `.nub` extension) out to all the appliances (service node, gateways, and controllers).

* Next, the non-hypervisor transport nodes are upgraded (this is the service nodes and gateways).

* Following that, the hypervisors need to be upgraded, though this isn't handled by the Update Coordinator. (You could, of course, leverage a tool like Puppet or Chef or similar to help automate this process.)

* After you've verified that the hypervisors have been updated, then the Update Coordinator upgrades the controller nodes.

* Following the successful upgrade of the controller nodes, there is a cleanup phase and then you're all set.

This is really high-level and I'm glossing over some details, naturally. Because an NVP upgrade is a pretty big deal---it could have an effect on the network connectivity of all the VMs and hypervisors within the NVP domain---it typically involves lots of planning, lots of testing, proper backups of all the components, and so on. However, since this was a lab environment and not a real production environment, just running through the Update Coordinator should have been fine.

As it turns out, though, I ran into a few problems---not problems with NVP, but problems with how I had deployed it. Basically, I didn't do my due diligence and _read the documentation._

When I first deployed the virtualized NVP appliances, I selected VMs that had a 10GB root disk. While this was enough to get NVP up and running, it turns out that it is **not enough space to perform an upgrade**. Specifically, it's not enough space to do an upgrade on the controllers; the transport nodes upgraded successfully. After the installation of the controllers, I was left with only a couple gigabytes of free space remaining. A fair portion of that is taken up then by the appliance update file, and this did not leave enough to actually perform the controller software upgrade.

Unfortunately, there was no easy workaround. Because the NVP controller cluster is scale out and highly available, I could have taken the controllers out (one at a time), rebuilt them with more disk space, and then re-joined the cluster---a rolling upgrade, if you will. However, because NVP 3.1.1 is a much older build of NVP, it wasn't possible to rebuild the controllers with a matching software version (not easily, anyway).

So, long story short: instead of wasting cycles trying to fix a deployment issue that is completely my fault (and, by the way, completely documented---had I paid closer attention to the documentation I wouldn't find myself in this position), I'm simply going to rebuild my lab environment from scratch using NSX 4.0.0. I had really hoped to be able to walk you through the upgrade process, but sadly it just doesn't make sense to do so.

This will be the last post titled "Learning NVP"; moving forward, all future posts will be titled "Learning NSX." The next post will discuss adding a gateway service to a logical network; this builds on information from [part 5 (creating a logical network)][2] and [part 6 (adding a gateway appliance)][3].

As always, your feedback is welcome and encouraged, so feel free to speak up in the comments below.

[1]: {{< relref "2013-11-01-learning-nvp-part-7-handling-the-nvp-to-nsx-transition.md" >}}
[2]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[3]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
