---
author: slowe
categories: Rant
comments: true
date: 2009-07-16T09:09:06Z
slug: mudslinging-between-cisco-and-hp
tags:
- Cisco
- Hardware
- HP
- Networking
- UCS
title: Mudslinging Between Cisco and HP
url: /2009/07/16/mudslinging-between-cisco-and-hp/
wordpress_id: 1463
---

I'll preface this article by saying that I am not (yet) an expert with Cisco's Unified Computing System (UCS), so if I have incorrect information I'm certainly open to clarification. Some would also accuse me of being a UCS-hater, since I had the audacity to call UCS a blade server (the horror!). Truth is, I'm on the side of the customer, and as we all know there is no such thing as a "one size fits all" solution. Cisco can't provide one, and HP can't provide one.

The mudslinging that I'm talking about is taking place between Steve Chambers (formerly with VMware, now with Cisco) and HP. HP published a page with a list of reasons why Cisco UCS should be dismissed, and Steve responded on his personal blog. Here are the links to the pages in question:

[The Real Story about Ciscos "One Giant Switch" view of the Datacenter](http://h71028.www7.hp.com/enterprise/us/en/messaging/realstory-cisco-datacenter-view.html) _(this was based, in part at least, on the next link)_  

[Buyer beware of the "one giant switch" data center network model](http://www.procurve.com/network-pro-news/articles/may09/buyer-beware.htm?ed=na)  

[HP on the run](http://viewyonder.com/2009/07/15/hp-on-the-run/)

I thought I might take a few points from these differing perspectives and try to call out some mudslinging that's occurring on _both_ sides. To be fair, Steve states in the comments to his article that it was intended to be entertaining and light-hearted, so please keep that in mind.

## Point #1: Complexity

The reality of these solutions is that they are both equally complex, just in different ways. HP's BladeSystem Matrix uses reasonably well-understood and mature technologies, while Cisco UCS uses newer technologies that aren't as widely understood. This is not a knock against either; as I've said before in many other contexts and many other situations, there are advantages and disadvantages to every approach. HP's advantage is that leverages the knowledge and experience that people have with their existing technologies: StorageWorks storage solutions, ProLiant blades, ProCurve networking, and HP software. The disadvantage is that HP is still tied to the same "legacy" technologies.

In building UCS, Cisco's advantage is that the solution uses the latest technologies (including some that are still Cisco-proprietary) and doesn't have any ties to "legacy" technologies. The disadvantage, naturally, is that this technological leap creates additional perceived complexity because people have to learn the new technologies embedded within UCS.

Adding to the simple fact that both of these solutions are equally complex in different ways is the fact that you must re-architect your storage in order to gain the full advantage of either solution. To get the full benefit of both UCS and HP BladeSystem Matrix, you need to be doing boot from SAN. (Clearly, this doesn't apply to virtualized systems, but both Cisco and HP are touting their solutions as equally applicable to non-virtualized workloads.) This is a fact that, in my opinion, has been significantly understated.

Neither HP nor Cisco really have the right to proclaim their solution is less complex than the other. Both solutions are complex in their own ways.

## Point #2: Standards-Based vs. Proprietary

Again, neither HP nor Cisco really have any room to throw the rock labeled "Proprietary". Both solutions have their own measure of vendor lock-in. HP is right; you can't put an HP blade or an IBM blade into a Cisco UCS chassis. Steve Chambers is right; you can't put a Dell blade or a Cisco blade server into an HP chassis. The reality, folks, is that every vendor's solution is has a certain amount of vendor lock-in. Does VMware vSphere have vendor lock-in? Sure, but so does Hyper-V and Citrix XenServer. Does Microsoft Windows have vendor lock-in? Of course, but so does...so does...well, you get the idea.

HP says VNTag is proprietary and won't even work with some of Cisco's own switches. OK, let's talk proprietary...does Flex-10 work with other vendor's switches? The fact of the matter is that both Cisco and HP have their own forms of vendor lock-in and neither can cry "foul" on the other. It's a draw.

## Point #3: The "Giant Network Switch"

At one point in HP's article (I believe it was under the Complexity heading) they make this point about the network traffic in a Cisco UCS environment:

>In Cisco's one-giant-switch model, all traffic must travel over a physical wire to a physical switch for every operation. Consequently, it appears that traffic even between two virtual servers running next to each other on the same physical would have to traverse the network, making an elaborate "hairpin turn" within the physical switch, only to traverse the network again before reaching the other virtual server on the same physical machine. Return traffic (or a "response" from the second virtual machine) would have to do the same. Each of these packet traversals logically accounts for multiple interrupts, data copies and delays for your multi-core processor.

I do have to call "partial FUD" on this one. In a virtualized environment, even a virtualized environment running the Cisco Nexus 1000V, traffic from one virtual server to another virtual server on the same host never leaves that host. HP's statement seems to imply that's not the case, and as far as I know it is. However, HP's statement is partially true: traffic from one virtual server on one physical host does have to travel to the fabric interconnect and then back again in order to communicate with a virtual server running on a physical host in the same chassis. The fabric extenders don't provide any switching functionality; that all occurs in the interconnect. Based on the information I've seen thus far, I would say that using Cisco's SR-IOV-based "Palo" adapter and attaching VMs directly to a virtual PCIe NIC _would_ put you into the situation HP is describing, which then just reinforces a question that Brad Hedlund and I tossed back and forth a couple of times: is hypervisor-bypass, aka VMDirectPath, with "Palo" the right design for all environments? In my opinion, no---I again go back to my statement that there is no "one size fits all" solution. And considering that the use of hypervisor-bypass with "Palo" would put you into a situation where traffic between two virtual machines on the same physical host has to travel to the fabric interconnect and back again, I'm even less inclined to use that architecture.

In the end, it's pretty clear to me that both HP and Cisco have some advantages and disadvantages to their respective solutions, and neither vendor really has the room to label the other as "more complex" or "more proprietary" than the other. But what do you think? Do you agree or disagree? Courteous comments (with full vendor disclosure) are welcome.
