---
author: slowe
categories: Liveblog
comments: true
date: 2013-08-27T12:29:44Z
slug: vmworld-2013-day-2-keynote
tags:
- Networking
- Storage
- Virtualization
- VMware
- VMworld2013
title: VMworld 2013 Day 2 Keynote
url: /2013/08/27/vmworld-2013-day-2-keynote/
wordpress_id: 3257
---

This is a liveblog of the day 2 keynote at VMworld 2013 in San Francisco. For a look at what happened in yesterday's keynote, see here. Depending on network connectivity, I may or may not be able to update this post in real-time.

The keynote kicks off with Carl Eschenbach. Supposedly there are more than 22,000 people in attendance at VMworld 2013, making it---according to Carl---the largest IT infrastructure event. (I think some other vendors might take issue with that claim.) Carl recaps the events of yesterday's keynote, revisiting the announcements around vSphere 5.5, VMware NSX, VMware VSAN, VMware Hybrid Cloud Service, and the expansion of the availability of Cloud Foundry. "This is the power of software", according to Carl. Carl also revisits the three "imperatives" that Pat shared yesterday:

1. Extending virtualization to all of it.

2. IT management giving way to automation.

3. Making hybrid cloud ubiquitous.

Carl brings out Kit Colbert, a principal engineer at VMware (and someone who relatively well-recognized within the virtualization community). They show a clip from a classic "I Love Lucy" episode that is intended to help illustrate the disconnect between the line of business and IT. After a bit of back and forth about the needs of the line of business versus the needs of IT, Kit moves into a demo of vCloud Automation Center (vCAC). The demo shows how to deploy applications to a variety of different infrastructures, including the ability to look at costs (estimated) across those infrastructures. The demo includes various database options as well as auto-scaling options.

So what does this functionality give application owners? Choice and visibility. What does it give IT? Governance (control), all made possible by automation. 

The next view of the demo takes a step deeper, showing VMware Application Director deploying the sample application (called Project Vulcan in the demo). vCloud Application Director deploys complex application topologies in an automated fashion, and includes integration with tools like Puppet and Chef. Kit points out that what they're showing isn't just a vApp, but a "full blown" multi-tier application being deployed end-to-end.

The scripted "banter" between Carl and Kit leads to a review of some of the improvements that were included in the vSphere 5.5 release. Kit ties this back to the demo by calling out the improvements made in vSphere 5.5 with regard to latency-sensitive workloads.

Next they move into a discussion of the networking side of the house. (My personal favorite, but I could be biased.) Kit quickly reviews how NSX works and enables the creation of logical network services that are tied to the lifecycle of the application. Kit shows tasks in vCenter Server that reflect the automation being done by NSX with regard to automatically creating load balancers, firewall rules, logical switches, etc., and then reviews how we need to deploy logical network services in coordination with application lifecycle operations.

At Carl's prompting, Kit goes yet another level deeper into how network virtualization works. He outlines how NSX eliminates the need to configure the physical network layer to provision new logical networks, and also discusses how NSX can provide logical routing, and they outline the benefits of distributed east-west routing (when routing occurs locally within the hypervisor). This, naturally, leads into a discussion of the distributed firewall functionality present in NSX, where firewall functionality occurs within the hypervisor, closest to the VMs. Following the list of features in NSX, Carl brings up load balancing, and Kit shows how load balancing works in NSX.

This leads into a customer testimonial video from WestJet, who discusses how they can leverage NSX's distributed east-west firewalling to help better control and optimize traffic patterns in the data center. WestJet also emphasizes how they can leverage their existing networking investment while still deriving tremendous value from deploying NSX and network virtualization.

Next up in the demo is a migration from a "traditional" virtual network into an NSX logical network, and Kit shows how the migration is accomplished via a vMotion operation. This leads into a discussion of how VMware can not only do "V2V" migrations into NSX logical networks, but also "P2V" migrations using NSX's logical-to-physical bridging functionality.

That concludes the networking section of the demo, and leads Carl and Kit into a storage-focused discussion centered around Carl's mythical Project Vulcan. The discussion initially focuses on VMware VSAN, and how IT can leverage VSAN to help address application provisioning. The demo shows how VSAN can dynamically expand capacity by adding another ESXi host in the cluster; more hosts means more capacity for the VSAN datastore. Carl says that Kit has shown him simplicity, scalability, but not resiliency. This leads Kit to a slide that shows how VSAN ensures resiliency by maintaining multiple copies of data within a VSAN datastore. If some part of the local storage backing VSAN fails, VSAN will automatically copy the data elsewhere so that the policy around how many copies of the data is maintained and enforced.

Following the VSAN demo, Carl and Kit move into a demo of a few end-user computing demonstrations, showing application access via Horizon Workspace. Kit wraps up his time on stage with a brief video---taken from "When Harry Met Sally," if I'm not mistaken---that describes how demanding the line of business can be. The wrap-up to the demo was quite natural feeling and demonstrated some good chemistry between Kit and Carl.

Next up on the stage is Joe Baguley, CTO of EMEA, to discuss operations and operational concerns. Joe reviews why script- and rules-based management isn't going to work in the new world, and why the world needs to move toward policy-based automation and management. This leads into a demo, and Joe shows---via vCAC---how vCenter Operations has initiated a performance remediation operation via the auto scale-out feature that was enabled when the application was provisioned. The demo next leads into a more detailed review of application performance via vCenter Operations.

Joe reviews three key parts of automated operations:

1. (missed this one, sorry)

2. Intelligent analytics

3. Visibility into application performance

Next, Joe shows how vCenter Operations is integrating information from a variety of partners to help make intelligent recommendations, one of which is that Carl should change the storage tier based on the disk I/O requirements of his Project Vulcan application. vCAC will show the estimated cost of that change, and when the administrator approves that change, vSphere will leverage Storage vMotion to migrate to a new storage tier.

The discussion between Carl and Joe leads up to a demo of VMware Log Insight, where Joe shows events being pulled from a wide variety of sources to help drill down to the root cause of the storage issue in the demonstration. VMworld attendees (or possibly anyone, I guess) are encouraged to try out Log Insight by simply following [@VMLogInsight](http://twitter.com/VMLogInsight) on Twitter (they will give 5 free licenses to new followers).

Next up in the demo is a discussion of vCloud Hybrid Service, showing how the vSphere Web Client can be used to manage templates in vCHS. Joe brings the demo full-circle by taking us back to vCAC to deploy Project Vulcan into vCHS via vCAC. Carl reviews some of the benefits of vCHS, and asks Joe to share a few use cases. Joe shares that test/dev, new applications (perhaps built on Cloud Foundry?), and rapid capacity expansion are good use cases for vCHS.

Carl wraps up the day 2 keynote by summarizing the technologies that were displayed during today's general session, and how all these technologies come together to help organizations deliver IT-as-a-service (ITaaS). Carl also makes commitments that VMware's SDDC efforts will protect and leverage customers' existing investments and help leverage existing skill sets. He closes the session with the phrase, "Champions drive change, so go drive change, and defy convention!"

And that concludes the day 2 keynote.
