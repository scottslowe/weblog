---
author: slowe
categories: Liveblog
comments: true
date: 2014-09-10T11:15:43Z
slug: idf-2014-data-center-mega-session
tags:
- Hardware
- IDF2014
- Intel
title: 'IDF 2014: Data Center Mega-Session'
url: /2014/09/10/idf-2014-data-center-mega-session/
wordpress_id: 3524
---

This is a liveblog of the Data Center Mega-Session from day 2 of Intel Developer Forum (IDF) 2014 in San Francisco.

Diane Bryant, SVP and GM of the Data Center Group takes the stage promptly at 9:30am to kick off the data center mega-session. Bryant starts the discussion by setting out the key drivers affecting the data center: new devices (and new volumes of devices) and new services (AWS, Netflix, Twitter, etc.). This is the "digital service economy," and Bryant insists that today's data centers aren't prepared to handle the digital service economy.

Bryant posits that in the future (not-so-distant future):

* Systems will be workload optimized

* Infrastructure will be software defined

* Analytics will be pervasive

Per Bryant, when you're operating at scale then efficiency matters, and that will lead organizations to choose platforms selected specifically for the workload. This leads to a discussion of customized offerings, and Bryant talks about an announcement earlier in the summer that combined a Xeon processor and a FPGA (field-programmable gate array) on the same die.

Bryant then introduces Karl Triebes, EVP and CTO of F5 Networks, who takes the stage to talk about FPGAs in F5 and how the joint Xeon/FPGA integrated solution from Intel plays into that role. F5's products use Intel CPUs, but they also leverage FPGAs to selectively enable certain functions in hardware for improved performance. Triebes talks about how F5 and Intel have been working together for about 10 years, and discusses how F5 uses instruction set changes (they write their own microkernel---is that really sustainable moving forward?), new features, etc., and that includes leveraging the integrated FPGA in Intel's new product.

The discussion now shifts to low-power system-on-chips (SoCs), such as the 64-bit Intel Atom. Bryant announces the third-generation SoC, named Xeon D and based on the Xeon platform. The Xeon D is sampling now. Bryant brings on stage Patty Kummrow, who is Director of Server SoC Development. Bryant and Kummrow talk about how Intel is addressing the need to customize the platform to address critical workloads: software (storage acceleration library, for example); in-package accelerator (FPGA, for example); SoC (potentially incorporating customer IP); and instruction set architectures (like the AES-NI instructions to enhance cryptographic functions). Kummrow shows off a Xeon D SoC and board.

Bryant shifts the discussion to software-defined infrastructure (SDI). The first area of SDI that Bryant focuses upon is storage, where growth is happening rapidly but storage is still siloed. Per Bryant, Intel believes that software-defined storage will address these concerns, and doing so in three ways:

* Intel Storage Acceleration Libraries (ISA-L)

* Open source investments in Ceph and OpenStack Swift

* Prototype SDS controller providing separate of control plane and data plane

Bryant now turns to software-defined networking (SDN) and network functions virtualization (NFV), and---quite naturally---points to the telcos as the prime example of why SDN/NFV are so important. According to Bryant, NFV originated in October 2011, and now (just three years later) there will be commercial deployments by companies like AT&T, Verizon Wireless, SK telecom, and China Mobile. Bryant also talks about Intel's Network Builders program, and talks about Nokia's recent announcement (which is based on Intel Xeon).

Shifting now to the compute side, Bryant talks about Intel's rack-scale architecture (RSA) efforts. RSA attempts to provide disaggregated pools of resources, a standard method of exposing hardware to software, and a composable infrastructure that can be assembled based on application resources.

Core to Intel's RSA efforts is silicon photonics, which is a key point to allowing high-speed, low-latency connections between the disaggregated resources within an RSA approach. Silicon photonics will enable 100Gbps at greater than 300 meters, at a low cost and with high reliability. Also important, but sometimes overlooked, is that the silicon photonics cabling will be smaller and thinner.

Bryant introduces Andy Bechtolsheim, Founder and Chief Development Officer and Chairman of Arista Networks. Bryant gives Bechtolsheim the opportunity to talk about Arista's recent launch of 100Gb networking and why 100Gb networking is important and necessary in modern data centers. Bryant states that she believes silicon photonics will be essential in delivering cost-effective 100Gb solutions, and that leads to a discussion of the CLR4 alliance. CLR4 is focused on delivering 100Gb over even greater distances.

Next, Bryant introduces Das Kamhout to talk about the need for an orchestration system in the data center. Kamhout talks about how advanced telemetry can be exposed to the orchestration system, which can make decisions based on that advanced telemetry. This will eventually lead to predictive actions. It boils down to a "watch, act, learn" feedback loop. The foundation is built on Intel technologies like Cache Acceleration, ISA-L, DPDK, QuickAssist, Cache QoS, and power and thermal awareness.

This "finally" leads into a discussion of pervasive analytics, which is one of the three key attributes of future data centers. Bryant states that pervasive analytics will help improve cities, discover treatments, reduce costs, and improve products---obviously all through data centers powered by Intel products. Intel's focus is to enable analytics, and is working closely with the Hadoop community (specifically Cloudera).

According to Bryant, the new Intel E5-2600 v3 more than doubles the performance of Cloudera's Hadoop distribution. Bryant brings out Mike Olson, Co-Founder and Chief Strategy Officer for Cloudera. Olson states that the consumer Internet "discovered" the idea of big data, but this is now taking off in all kinds of industries. Olson gives examples of hospitals instrumenting neonatal care units and cities gathering data on air quality more frequently and more comprehensively. Both Olson and Bryant reinforce the value of open source to "amplify" the effect of certain efforts. Olson again conflates big data and the Internet of Things (IoT), indicating that he believes that the two efforts are naturally coupled and will drive each other. Bryant next gives Olson the opportunity to talk about Cloudera Hadoop 5.2, which is optimized for Intel architectures to provide more performance and more security, which in turn will lead to accelerated adoption of Hadoop. Bryant reinforces the link between IoT/wearables and big data, mentioning again the "A-wear" program discussed yesterday in the keynote.

At this point Bryant wraps up the keynote and the session ends.
