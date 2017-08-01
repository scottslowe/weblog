---
author: slowe
categories: Rant
comments: true
date: 2015-02-17T10:42:00Z
tags:
- ToL
- Docker
- Virtualization
title: A Quick Thought About Mesos-DNS
url: /2015/02/17/quick-thought-mesos-dns/
---

A colleague recently pointed me to [the recent Mesosphere announcement][link-1] of [Mesos-DNS][link-2], a DNS-based service discovery mechanism for [Apache Mesos clusters][link-3]. A comment made in the announcement got me thinking, and I wanted to briefly share my thoughts.

The comment that got me thinking was this:

>Mesos-DNS is simple and stateless. Unlike [Consul][link-4] and [SkyDNS][link-5], it does not require consensus mechanisms, persistent storage, or a replicated log.

If you've been following along here on my site, you know that I've written about Consul before (see [here][xref-1]), and I have more Consul content planned. I'm reasonably familiar with Consul's architecture and requirements. Likewise, although I haven't specifically written about SkyDNS, it's based on [etcd][link-6], which I have talked about (see [here][xref-2]). The Mesos-DNS article seems to imply that Mesos-DNS is somehow less complex than either of these two solutions because it doesn't require consensus mechanisms, persistent storage, or a replicated log.

However, in my mind that's a misleading statement. Yes, Consul _does_ require a consensus mechanism (it uses Raft, like etcd). SkyDNS (as I understand it, at least) simply leverages etcd, so technically SkyDNS itself doesn't require a consensus mechanism. And the assertion that SkyDNS itself doesn't require a consensus mechanism is no different than the assertion that Mesos-DNS itself doesn't require one either---both of them rely on something else that provides this functionality. In the case of SkyDNS, it's etcd; in the case of Mesos-DNS, it's Mesos, which _does_ require a consensus mechanism. Therefore, to say that Mesos-DNS doesn't require a consensus mechanism (when in fact it relies on one in Mesos) while SkyDNS does (when in fact that functionality is provided by etcd) is misleading. You can either say that **neither Mesos-DNS nor SkyDNS** require a consensus mechanism (because it is provided by a different component), or you can say that **both Mesos-DNS and SkyDNS** require a consensus mechanism because both of them rely on another component that incorporates consensus functionality.

Now, this statement _is_ fair when discussing Consul, which does incorporate its own consensus and clustering technologies. Of course, the key difference between Mesos-DNS and Consul is that Mesos-DNS is purpose-built for Mesos environments, whereas Consul could work with any kind of environment. Therefore, Consul _had_ to build in its own consensus/clustering functionality, while Mesos-DNS knew it could rely on the underlying functionality found in Mesos.

I know this doesn't seem like a very big deal, but I felt like it needed to be mentioned. There is a saying that goes something like this:

>"We judge others by their behavior. We judge ourselves by our intentions."

(The earliest known attribution for this quote is 1917, by Montague Jocelyn King-Harmon in "British Boys: Their Training and Prospects.")

I think a variation of this saying is applicable here: you can't chide other products for their external dependencies while overlooking your own.


[link-1]: http://mesosphere.com/2015/01/21/mesos-dns-service-discovery/
[link-2]: https://github.com/mesosphere/mesos-dns
[link-3]: http://mesos.apache.org/
[link-4]: https://consul.io/
[link-5]: https://github.com/skynetservices/skydns
[link-6]: https://github.com/coreos/etcd/
[xref-1]: {{< relref "2015-02-06-quick-intro-to-consul.md" >}}
[xref-2]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}