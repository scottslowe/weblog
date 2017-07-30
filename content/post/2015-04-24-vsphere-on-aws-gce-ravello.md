---
author: slowe
categories: News
comments: true
date: 2015-04-24T14:00:00Z
tags:
- VMware
- vSphere
- Cloud
- AWS
title: Running vSphere on AWS or GCE
url: /2015/04/24/vsphere-on-aws-gce-ravello/
---

By now you've probably seen or heard the news about Ravello Systems launching Inception---the ability to run nested VMware ESXi on AWS or GCE, including the ability to run VMs on these nested ESXi instances. ([Here's Ravello's press release.][link-1])

In my opinion, this is pretty cool, and it opens the door to a _lot_ of different possibilities: upgrade testing, automation testing, new feature testing, hosted home labs (aka "Lab as a Service"). Lots of folks are interested in using this new Ravello functionality for "Lab as a Service." [Here's Andrea Mauro's take on this topic.][link-3]

As part of the pre-launch activities, a number of bloggers and community advocates were able to work with Ravello on some very interesting projects:

* William Lam built both a 32-node VSAN cluster (running vSphere 5.5) as well as a 64-node VSAN cluster (running vSphere 6.0). He posted details [here][link-2], along with a great walkthrough of setting up vSphere on Ravello.
* Mike Preston built out an environment that allowed him [to perform a vMotion from AWS to GCE][link-4].

I was also engaged with Ravello on a project: building a (reasonably) large-scale vSphere environment on Ravello. The original goal was to spin up 500 virtual ESXi hosts, each running 2 or 3 VMs, to help show the scalability and stability of the Inception offering. The design that I created for the Ravello team to use was a pretty standard design:

* The ESXi hosts would be run in clusters, starting small and eventually scaling to 8 clusters of approximately 64 hosts.
* Because NIC redundancy wouldn't be an issue, the virtual ESXi hosts each had two network interfaces.
* The environment would use Host Profiles to help with configuration of the ESXi hosts.
* Both network interfaces on the ESXi hosts were connected to a vSphere Distributed Switch.
* VCSA would manage the environment. To help avoid any potential interactions resulting from running VCSA on the environment it was managing, I recommended a separate management cluster.
* The environment would use a "home grown" VSA running NFS for VM storage. Initially, because we were unclear how well this makeshift VSA would perform, the plan was to have a VSA for each cluster.  

As you can see, the design is pretty straightforward. As is often the case, the actual implementation ended up looking a bit different than the original design in a couple of ways:

1. Using New Relic, the Ravello team was able to monitor the performance of the home-grown VSA, and it held up remarkably well. Instead of using a separate VSA for each cluster, we ended up with a single VSA for the entire environment.
2. The implementation team ended up skipping the separate management cluster and running the VCSA directly on the same hosts it was managing. This did not introduce any issues or problems, but was a deviation from the original design.

[This blog post from Ravello][link-5] describes in more detail the specific steps that were taken to build the environment. Note that very little of what is documented in that blog post is specific to Ravello; most of it pertains to any reasonably sizable vSphere environment.

So, what was the result of our project?

* We grew the environment to 250 ESXi hosts, not the original 500 that was planned. This was due strictly to time constraints around the timing of the announcement, and not due to any limitation in Ravello's platform. (Although Ravello did find and resolve a couple of issues as a result of the scale testing, thus improving the quality and stability of the offering.)
* The singular home-grown NFS VM managed to support approximately 500 VMs. Granted, these VMs generated very little disk traffic, and the NFS VM did suffer from boot storm issues, but we were still a bit surprised that it scaled as far as it did.
* The primary bottleneck identified was the VCSA itself, which started having stability and performance issues at the 250-host mark. The Ravello team was going to reconfigure VCSA to see if those issues could be resolved, but (as of this writing) I don't know if that ever happened.

All in all, it was a pretty nice project in which to be involved, and it demonstrated Ravello's ability to host sizable vSphere environments through their platform. I'm keen to see how the community and the customers use this functionality from Ravello.


[link-1]: http://www.ravellosystems.com/news/nested-esxi-cloud-launch
[link-2]: http://www.virtuallyghetto.com/2015/04/running-nested-esxi-vsan-home-lab-on-ravello.html
[link-3]: http://vinfrastructure.it/2015/04/ravello-system-and-its-lab-as-a-service-solution/
[link-4]: http://blog.mwpreston.net/2015/04/15/a-google-cloud-to-amazon-vmotion-the-ravello-way/
[link-5]: http://www.ravellosystems.com/blog/250-node-esxi-cloud/
