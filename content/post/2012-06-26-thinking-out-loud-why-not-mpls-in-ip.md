---
author: slowe
categories: Musing
comments: true
date: 2012-06-26T09:02:46Z
slug: thinking-out-loud-why-not-mpls-in-ip
tags:
- Networking
- Virtualization
- VXLAN
- MPLS
- ToL
title: 'Thinking Out Loud: Why Not MPLS-in-IP?'
url: /2012/06/26/thinking-out-loud-why-not-mpls-in-ip/
wordpress_id: 2648
---

As I was reviewing my list of actions in OmniFocus this morning, I saw an action I'd added a while back to review [RFC 4023](http://datatracker.ietf.org/doc/rfc4023/?include_text=1). RFC 4023 is the RFC that defines MPLS-in-IP and MPLS-in-GRE encapsulations, and was written in 2005. I thought, "Let me just read this real quick." So I did.

It's not a terribly long RFC (only about 13 pages or so), but I'll attempt to summarize it here. (Networking gurus, feel free to correct my summary if I get it wrong.) The basic idea behind RFC 4023 is to allow two MPLS-capable LSRs (an LSR is a label switching router) that are adjacent to each other with regard to a Label Switched Path (LSP) to communicate over an intermediate IP network that does _not_ support MPLS. Essentially, it tunnels MPLS inside IP (or GRE).

"OK," you say. "So what?"

Well, here's my line of thinking:

1. All networking experts agree that we need to move away from the massive layer 2 networks we're building to support virtualized applications in enterprise data centers. (Case in point: see [this post](http://blog.ioshints.info/2012/05/layer-2-network-is-single-failure.html) from Ivan on a layer 2 network being a single failure domain.)

2. Networking experts also seem to agree that the ideal solution is IP-based at layer 3. It's ubiquitous and well understood.

3. However, layer 3 alone doesn't provide the necessary isolation and multi-tenancy features that we all believe are necessary for cloud environments.

4. Therefore, we need to provide some sort of additional isolation but also maintain layer 3 connectivity. Hence, the rise of protocols such as VXLAN and NVGRE, that isolate traffic using some sort of virtual network identifier (VNI) and wrap traffic inside IP (or GRE, as in the case of NVGRE).

It seems to me---and I freely admit that I could be mistaken, based on my limited knowledge of MPLS thus far---that MPLS-in-IP could accomplish the same thing: provide isolation between tenants **and** maintain layer 3 connectivity. Am I wrong? Why not build MPLS-in-IP endpoints (referred to in RFC 4023 as "tunnel head" and "tunnel tail") directly into our virtualization hosts and build RFC 4023-style tunnels between them? Wouldn't this solve the same problem that newer protocols such as VXLAN, NVGRE, and STT are attempting to solve, but with protocols that are already well-defined and understood?

Perhaps my understanding is incorrect. Help me understand---speak up in the comments!
