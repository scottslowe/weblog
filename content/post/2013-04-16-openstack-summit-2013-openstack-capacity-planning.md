---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-16T18:19:48Z
slug: openstack-summit-2013-openstack-capacity-planning
tags:
- OpenStack
title: 'OpenStack Summit 2013: OpenStack Capacity Planning'
url: /2013/04/16/openstack-summit-2013-openstack-capacity-planning/
wordpress_id: 3147
---

This is a liveblog of a session titled "OpenStack Capacity Planning." The presenter starts out with a shout-out to the OpenStack Operations Guide that was recently written.

Here's how to make capacity planning easy and simple:

* A blank check backed by limitless funds

* Unlimited time

* A well-organized team of geniuses

* Perfectly clear expectations that never change (up front & in writing)

Don't have all that? Well, then you have to worry about capacity planning.

To start with capacity planning, the presenter suggests that the absolute best place to start is DevStack. Using DevStack allows you to test various capacity planning scenarios easily and quickly. However, the presenter warns against trying to use DevStack in production.

Next, you need to answer the question: Public cloud or private cloud? The answer to this question will drive a lot of the follow-up questions that you must answer. If the answer to this first question is private cloud, then it's typically easier to do capacity planning because you'll generally have a better idea of what sort of applications and workloads will be deployed. The presenter also feels that limited hardware/networking/storage choices in private cloud deployments makes capacity planning easier (although I'm not sure I agree). Deployment is also easier and can be a bit more leisurely due to lower growth rates.

For a public cloud, capacity planning is much trickier. You'll have to design against a generic use case/workload because you won't know the types of applications and workloads that your customers will actually put on this public cloud. In public cloud environments, you'll likely be standing up new compute nodes constantly, so the speed and frequency of deployment is a key issue. Tools for fast, reliable provisioning and configuration management are very important.

The presenter mentions a few tools in this space: Crowbar, Puppet, Razor, Cobbler, Chef, CFEngine, and Fuel.

Monitoring is often an afterthought, but it really shouldn't be---it should be done up front as much as possible. Here you can look at tools like Nagios, but there is a lot of debate in this space within the open source community. One of the reasons monitoring is important is to help establish some trending information, which helps in forecasting capacity needs.

What about the hypervisor you're going to use? The presenter feels like this is an important decision. Pick the best hypervisor for your workload. The presenter prefers KVM, but there are others (he notably doesn't mention vSphere, but does mention Hyper-V---interesting). He recommends against mixed hypervisor/heterogeneous environments. Hypervisor decision will also drive some storage decisions down the road (among other things).

At this point, the presenter circles back to DevStack, and recommends the use of DevStack to "test the heck out of" your selected hypervisor and anticipated workload. This isn't necessarily for performance benchmarks, but rather to validate that everything works as expected.

Networking is the next big topic. The presenter recommends being very intentional about network selections, as he says that it is **extremely** difficult to switch between networking architectures (like from FlatDHCP to Quantum). Also be sure to watch out for bandwidth requirements and design accordingly. Naturally, IP addressing will be another concern you'll need to consider.

Software-defined networking (SDN) is what the presenter recommends for any sizable deployments. He's partial to NEC's OpenFlow solution.

Compute density is a key factor in capacity planning. You'll need to incorporate physical CPU cores, RAM, oversubscription ratio, and instance storage (ephemeral storage is local or shared). Using shared storage, like NFS or Ceph, means recovering VMs is much easier. Naturally, this means you'll need to balance IOPS against storage capacity (GB/TB). Based on the speaker's comments, I'd guess that he heavily favors Ceph.

When it comes to storage, your options for object storage are Swift, Ceph, and Gluster. Bandwidth is a concern (moving data to/from the object storage platform). The presenter stresses the importance of testing to understand the impact of the workloads on the object storage platform. The same goes for persistent block storage.

Finally, the presenter touches on concerns over the scalability of the cloud controller (which hosts the API endpoints and database). At some point you'll have to consider separating the API endpoints onto dedicated boxes and using a load balancer. The presenter also suggests considering the use of Nova cells to help with growth and partitioning the load on the cloud controllers. (Cells are something that I really need to understand a bit better.)

And that concludes the session.

Overall, I didn't find this session as useful as I would have liked---I expected this to be about ongoing capacity planning, not implementation design considerations. However, others might have come into the session with different expectations and might have found this session helpful. The key takeaway for me is that, as I saw from a similar session yesterday, it's just as important when designing an OpenStack implementation to consider the resource demands, workload needs, and similar requirements. Just because it's "cloud" doesn't mean that it doesn't still require some knowledge of how these components work under the covers.
