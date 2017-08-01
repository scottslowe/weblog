---
author: slowe
categories: Education
comments: true
date: 2013-08-12T09:38:10Z
slug: introduction-to-networking-part-1-the-basics
tags:
- Networking
title: 'Introduction to Networking: Part 1, The Basics'
url: /2013/08/12/introduction-to-networking-part-1-the-basics/
wordpress_id: 3233
---

This is the kick-off to a series of posts introducing networking concepts to VMware administrators. Based on the feedback I've gotten in speaking to VMware admins, networking is an area in which a lot of VMware-focused folks aren't particularly comfortable. So, I thought it might be helpful to put up a few blog posts on networking concepts for VMware administrators. (If you're already familiar with networking concepts, you probably don't need to read this---unless you just have some free time on your hands.)

In this first article, I'll cover some important networking basics. This will set the stage for discussions that will take place in future articles. Here are some of the topics that I'm going to cover in this first article:

* Layer 2 versus Layer 3: the OSI and DoD models

* Theory into reality: TCP/IP and Ethernet

* Bridging, switching, and routing

* Spanning Tree Protocol (STP)

* ARP and Flooding

Ready? Let's get started.

## Layer 2 Versus Layer 3: The OSI and DoD Models

In networking, two "models" of how networked systems should communicate drive almost everything. In the beginning, there was the OSI (Open Systems Interconnection) model, a seven layer model that described and defined how two systems might communicate with each other in a standardized fashion across a network. This model is largely theoretical, but it is still important to understand because it shapes much of what is done today in networking. Each of these layers was written with the idea of being as generic and interoperable as possible; the idea was that you could have standards (or protocols) at each layer so that the layers could evolve somewhat independently of one another.

Two of these layers---layer 2, the Data Link layer, and layer 3, the Network layer---are the basis for the "layer 2" and "layer 3" discussions that you so frequently hear thrown about when someone is discussing networking. A "sub-layer" of the Data Link layer (layer 2) is also the basis for another term that you'll hear frequently thrown about in networking: the MAC address. This sublayer is the Media Access Control, or MAC, sublayer, and it's where MAC addresses are used.

&lt;aside&gt;Note that "MAC" and "Mac" are very different! The first pertains to the Media Access Control sublayer of the OSI model; the second pertains to a line of computers made by Apple, Inc. You shouldn't call an Apple computer a MAC, and you shouldn't refer to a Media Access Control address as a Mac address. OK, stepping off the soap box now...&lt;/aside&gt;

The third layer of the OSI model, the Network layer, is the "layer 3" that so often referenced in network discussions. Protocols like Internet Protocol (IP) and IPX operate at this layer.

After the creation of the OSI model, the US Department of Defense started work on what would eventually become the Internet. (No, it wasn't invented by Al Gore. Sorry, Al.) As part of that work---I won't cover that here as there have been plenty of other write-ups of [that history](https://en.wikipedia.org/wiki/History_of_the_Internet)---they created a four-layer model that became known as the DoD model or the TCP/IP model. The four layers of the DoD model---Link, Internet, Transport, and Application---have a rough correlation to the seven layers of the OSI model, as shown [here](http://www.sis.pitt.edu/~icucart/networking_basics/4LayersofTCPIPModel.html). Despite this fact, discussions of "layer 2" and "layer 3" still refer to the Data Link and Network layers of the OSI model, and not to the DoD-TCP/IP model.

And that leads us to our next section...

## Theory Into Reality: Ethernet and TCP/IP

The OSI model, in particular, is highly theoretical. As far as I know, there are no implementations that actually implement all seven layers. (They might implement the functions of all seven layers, but not the actual layers themselves.) However, the abstract nature of the OSI model was beneficial in the early days, when there were a number of different physical media types (Ethernet, Token Ring, ATM, etc.) and different network protocols (NetBEUI/NetBIOS, IPX/SPX, TCP/IP, SNA, etc.). Over time, though, the networking industry has---in the data center, at least, where this discussion is primarily focused---whittled itself down to just two standards that are almost universally deployed: Ethernet and TCP/IP.

Therefore, as I move through this series, I'm going to assume the use of Ethernet and TCP/IP. Yes, other protocols will almost certainly be present, but in the data center I think it is reasonably safe to assume the use of Ethernet and TCP/IP.

So, when I talk about "layer 2," then, I am talking about Ethernet (and all of its variations: Ethernet, Fast Ethernet, Gigabit Ethernet, 10 Gigabit Ethernet). When I talk about a MAC address, I'm talking about an Ethernet address.

Similarly, when I talk about "layer 3," I'm talking about Internet Protocol (the IP in TCP/IP).

This is not to say that other standards and other protocols don't exist, but simply to narrow down the discussion so that I can productively move ahead with what I need to discuss.

## Bridging, Switching, and Routing

A _bridge_ is a device that operates at the Data Link layer (layer 2 of the OSI model) and is therefore referred to as a "layer 2 device" or a "Data Link device." According to most definitions, a bridge has only two ports, and serves to connect two separate networks. A bridge can't and doesn't understand anything more than layer 2 stuff---it doesn't know anything about upper layer protocols. Although there are different types of bridges, for the purposes of our discussion I'm going to assume the use of transparent bridges, meaning that the bridges are transparent to the hosts communicating across them. Cisco has [more information on transparent bridging](http://docwiki.cisco.com/wiki/Transparent_Bridging), which is the most common form of bridging in Ethernet-based networks.

A _switch_ is, essentially, a multi-port bridge. It also operates at OSI layer 2 and doesn't know about or understand anything with regards to upper layer protocols. Like a transparent bridge, a switch is invisible to the hosts communicating across it.

Neither bridges nor switches modify the frames that move across them.

A _router_ is a device that operates at OSI layer 3. Because network protocols exist at layer 3, routers are generally protocol specific. I've limited the discussion here to TCP/IP, but routers exist for other network protocols as well. A key difference between bridges/switches and routers is that routers actually modify the packets moving across them, typically by changing the layer 2 addresses in the packet and by decrementing the Time To Live (TTL) counter. The TTL counter is a field that keeps a packet from endlessly circling the network; when the TTL expires (reaches 0), the packet is discarded. Ethernet frames do not have a TTL. (Routers also change the source and destination layer 2 addresses, but I'll discuss that in more detail in a future post.)

Although you will see references to layer 3 switching, for the purposes of this series I will use _switching_ to refer strictly to layer 2 functions and _routing_ to refer strictly to layer 3 functions.

Now might also be a good time to discuss the idea of a _broadcast domain_ (more information [here](https://en.wikipedia.org/wiki/Broadcast_domain)). A broadcast domain encompasses all the devices and hosts that can reach each other by broadcast at the Data Link layer (layer 2). In other words, a broadcast domain will include all the devices and hosts that are connected by bridges or switches, but it will **not** include devices or hosts connected by a router. Broadcast domains are separated by layer 3 devices like routers.

## Spanning Tree Protocol

Pop quiz time! Here's your question: what happens if an Ethernet frame enters a switch, is forwarded out a port connected to another switch, which forwards the frame out a port back to the original switch?

This, boys and girls, is what is called a _bridging_ (or _switching_) _loop_, and it is a Very Bad Thing. As I stated earlier, Ethernet frames don't have a TTL, so there's no notion of ensuring that a frame doesn't circle the network endlessly.

Contrast that with what's called a _routing loop_, where packets are sent to a router, which in turns sends them to another router, where they are then sent back to the first router, etc., etc. In this case, though, the TTL will be decremented each time the packet passes through the router, and eventually the TTL will expire. When the TTL expires, the packet is removed from the network.

To protect networks against bridging loops, the network experts created something called _Spanning Tree Protocol_ (STP). The purpose of STP is to ensure that loops aren't created as switches are connected to one another in larger networks. (In a single switch network, there's obviously no need for STP.) This is clearly an admirable goal, but a side effect of STP is that it prevents multiple layer 2 paths between switches. I'll have more to say about STP in future posts, but for now understand that STP is a necessary component in larger environments in order to prevent bridging loops.

## ARP and Flooding

Recall from earlier in this post that addresses are employed at OSI layer 2 (these are called MAC addresses and, in the context of this discussion, are Ethernet addresses that look something like `aa:bb:cc:11:22:33`). You probably also already know that addresses are also employed at OSI layer 3, where IP resides. IP addresses---specifically IPv4 addresses, no need to discuss IPv6 just yet---are typically expressed in what's called _dotted decimal notation_ and look something like 192.168.100.123.

There's a reason I'm mentioning all of this. In order for two systems (I'll call them Host A and Host B) to communicate with each other, they must know how to get from A to B (and back again). This means they need to know the correct addresses in order to be able to communicate. Generally, the hosts will know the IP address (or hostname, which will resolve to IP address via the Domain Name System [DNS]), such as in the example of your laptop connecting to a web server via a URL like "http://192.168.100.123" or "http://www.vmware.com".

Stepping back to our layer 2 vs. layer 3 discussion for a second, recall that IP addresses operate at layer 3. However, in order to communicate across an Ethernet network, the hosts need to know more than just the IP addresses---the hosts also need to know the Ethernet (MAC) addresses that operate at layer 2.

That's where ARP (Address Resolution Protocol) comes into play. ARP performs the necessary function of associating an IP address with a MAC address. When Host A needs to communicate with Host B and knows the IP address of Host B but not its MAC address, it will send out an _ARP query_. This ARP query is sent to the Ethernet broadcast address of `ff:ff:ff:ff:ff:ff`. Because this is a layer 2 broadcast address, it will reach all other hosts in the same broadcast domain (I defined what a broadcast domain is earlier). Assuming Host B is on the same broadcast domain as Host A, then Host B will receive the ARP query and will respond directly to Host A (not via broadcast, but via unicast to Host A's MAC address). If Host B is not on the the same broadcast domain, then it won't see the ARP query, won't respond to Host A, and Host A will fail to connect to Host B.

_Flooding_ is a term used to describe the behavior of a switch under certain conditions. Layer 2 broadcasts, such as ARP queries, have to be sent to all the ports on the switch; after all, a broadcast frame is supposed to be sent to all hosts. A broadcast frame has to be flooded to all ports. However, there are other instances in which flooding occurs. Recall that a switch is a multi-port bridge, operating at layer 2, that directs frames from a source MAC address to a destination MAC address. What happens if the switch doesn't know which destination MAC address corresponds to which switch port? In this case, the switch must flood the frame out all ports, because it doesn't have the necessary MAC address-to-port mapping, even though it's not a broadcast frame. The switch then listens for the response to that frame in order to learn what port should be used next time it needs to direct traffic to that particular MAC address. When it does learn the mapping between port and MAC address, it stores that in an internal data structure so that it can use it for future traffic flows.

I'll wrap up this first post here. I'll build on and expand upon these basics in the next post. In the meantime, feel free to post any questions you might have in the comments below. Networking experts, if I have misrepresented something---keeping in mind that I've simplified certain concepts to keep things digestible for newcomers---you are welcome to post corrections or clarifications in the comments. Courteous comments are always welcome.
