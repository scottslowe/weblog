---
author: slowe
categories: Explanation
comments: true
date: 2009-09-24T12:37:28Z
slug: more-on-vswitch-load-balancing
tags:
- Cisco
- IOS
- Networking
- Virtualization
- VMware
title: More on vSwitch Load Balancing
url: /2009/09/24/more-on-vswitch-load-balancing/
wordpress_id: 1662
---

I had a customer contact me about scaling network throughput when using NFS datastores. Specifically, this customer was interested in knowing if it was possible to utilize more than 1 NIC with IP-based storage. The customer is currently using link aggregation (EtherChannel on a Cisco switch). I pointed the customer to [my post on NIC utilization][1], in which I explain the prerequisites for utilizing more than 1 NIC in this sort of configuration. To refresh your memory, those prerequisites are:

* The vSwitch must be configured for "Route based on IP hash"

* The physical NICs connected to the vSwitch as uplinks must all be configured as active in the failover order

* The physical switch must be configured for link aggregation

* There must be multiple, unique source-destination IP address pairs involved

The customer responded with a question (which I'm paraphrasing here): "That's all? It will just automatically use more than one link?"

Well..._sort of._

There is one little caveat. Cisco IOS uses a hashing algorithm to determine which link a particular traffic flow between a source and destination will use. This algorithm is controlled by the `port-channel load-balance` command. Assuming that you're using source-destination IP hashing, that means the Cisco switch will use a hash of the source IP address and the destination IP address to determine which link it will use. [This page](http://www.cisco.com/en/US/tech/tk389/tk213/technologies_tech_note09186a0080094714.shtml#catalyst) has more detailed information.

It's theoretically possible, based on the number of links in the port channel, that some traffic flows between different pairs of source-destination IP addresses might end up on the same link. That means it's not necessarily just as simple as setting up multiple NFS exports or iSCSI targets on different IP addresses---you also need to know if the IP addresses you are using will _actually result in the traffic being distributed across the links_.

How does one tell? Good question, and one I'm glad you asked. You can tell using this command (this command assumes you are using IP-based hashing):

	switch# test etherchannel load-balance interface <Port channel interface> ip <Src IP Addr> <Dst IP Addr>

So, let's say that you have an ESX/ESXi host with a VMkernel interface whose address is 172.16.5.10. Let's say that you have a storage array (NetApp FAS, EMC Celerra, etc.) that supports NFS and you want to mount two different NFS exports on two different IP addresses so that traffic from this ESX/ESXi host to the storage array. You could use the `test etherchannel load-balance` command to help you determine which address could help ensure traffic distribution across the links:

	switch# test etherchannel load-balance interface Po3 ip 172.16.5.10 172.16.5.100

For more examples of what the output would look like, take a look at [this image](/public/img/test-ethchannel-output.png). This was taken off a Cisco Catalyst 3560G running my test lab (and yes, the IP addresses have been changed to protect the innocent).

This would give you one way of testing whether your link aggregation configuration would actually use multiple links, or only a single link due to the IP hash calculation. Also, don't forget that `esxtop` can also show you NIC utilization; [here's an example](/public/img/esxtop-net-output-ethchannel.png) of both uplinks being used in this sort of configuration.

Unfortunately, what I _can't_ tell you right now is what algorithm the vSwitch itself uses to place traffic onto the uplinks. Does it follow the same sort of mechanism as the Cisco switch? I don't know. If anyone has any information on that, it would be tremendously helpful.

If anyone has any other pertinent information or resources on this topic, please add them to the comments below.

**UPDATE:** Duncan Epping pointed out [an article by Ken Cline from earlier this year](http://kensvirtualreality.wordpress.com/2009/04/05/the-great-vswitch-debatepart-3/) provides the mechanism VMware uses to determine which uplink on a vSwitch will be used. This algorithm performs an XOR operation on the Least Significant Byte (LSB) of the source and destination IP addresses, then finds the modulus of that result and the number of uplinks. Thanks, Duncan and Ken!

[1]: {{< relref "2008-07-16-understanding-nic-utilization-in-vmware-esx.md" >}}
