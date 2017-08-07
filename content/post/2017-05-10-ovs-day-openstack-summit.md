---
author: slowe
categories: Liveblog
comments: true
date: 2017-05-10T00:00:00Z
tags:
- OVS
- OVN
- OpenStack
- Kubernetes
- Networking
- Security
title: Open vSwitch Day at OpenStack Summit 2017
url: /2017/05/10/ovs-day-openstack-summit/
---

This is a "liveblog" (not quite live, but you get the idea) of the Open vSwitch Open Source Day happening at the OpenStack Summit in Boston. Summaries of each of the presentations are included below.<!--more-->

## Kubernetes and OVN on Windows

The first session was led by Cloudbase Solutions, a company out of Italy that has been heavily involved in porting OVS to Windows with Hyper-V. The first part of the session focused on bringing attendees up to speed on the current state of OVS and OVN on Hyper-V. Feature parity and user interface parity between OVS/OVN on Hyper-V is really close to OVS/OVN on Linux, which should make it easier for Linux sysadmins to use OVS/OVN on Hyper-V as well.

The second part of the session showed using OVN under Kubernetes to provide networking between Windows containers on Windows hosts and Linux containers on Linux hosts, including networking across multiple cloud providers.

## Lightning Talks

The lightning talks were all under 5 minutes, so a brief summary of these are provided below:

* Joe Stringer showed how to set up OVS with an OpenFlow controller (Faucet) to do networking between multiple hosts in 5 minutes or less.
* A gentleman (I didn't catch his name) from Yahoo Japan discussed his experience with OVS DPDK in OpenStack compute nodes.
* Joe Stringer gets back up again to talk about the Faucet SDN controller and its test suite (leveraging Mininet) to validate OpenFlow performance and compliance.

## OpenStack NFV: Performance with OVS-DPDK for NFV and Connection Tracking

This session provided some in-depth discussions on performance implications of using connection tracking in OVS-DPDK environments. In the first part of the session, the presenters provided a good overview of how connection tracking works with OVS, and showed performance statistics resulting from the use of connection tracking with OVS-DPDK. 

In the second part of the presentation, the presenters provided some information on OVS-DPDK in real-world telco use cases, mostly focusing on debugging and fine-tuning performance. The presenters did a _great_ job of covering how to debug OVS flows and troubleshoot performance, showing exactly the commands that were needed and the key metrics to examine.

## OVN Gateway and IPv6 Support

This was another session with 2 parts; the first part, led by Russell Bryant, discussed OVN's support for various gateway modes. By and large, OVN now supports almost all of the features supplied by the ML2-OVS implementation under OpenStack, although OVN isn't tied to OpenStack (can also be used with oVirt, Kubernetes, and Docker Networking). OVN's IPv6 support is still lacking a few features, although development is still happening in those areas.

## OVN Tutorial

Ben Pfaff led the audience through an OVN tutorial using OVN and DevStack. The tutorial is available [online here][link-1], in case folks who weren't able to attend are interested in running through it. Attendees got a good view of how OVN is structured and commands you can use to better understand what's happening under the hood with OVN.

## Scaling OVN with Kubernetes API Server

This session described a mechanism that eBay is using to scale OVN in a Kubernetes environment; they add a new component---called "Baker", a play on the fact that OVN is often pronounced "oven"---that leverages etcd to provide as scale-out layer above the OVN northbound database for additional scale. The session also shared some performance details around Baker+OVN+Neutron.

## New Approach to OVS Datapath Performance

This session described an alternate approach to an OVS datapath that would supposedly marry the benefits of the OVS kernel datapath (integrated in the Linux kernel) and the DPDK datapath. If I understood the presentation correctly, though, this new approach would require "re-upstreaming" into the Linux kernel, which is not an insignificant effort.

That wrapped up the OVS Open Source Day at the OpenStack Summit. I _think_ the audio from the sessions may have been recorded; if I find any information on where those recordings may be available, I'll update this post.



[link-1]: http://docs.openvswitch.org/en/latest/tutorials/ovn-openstack/
