---
author: slowe
categories: Education
comments: true
date: 2015-02-06T16:29:00Z
tags:
- Linux
- CLI
title: A Quick Introduction to Consul
url: /2015/02/06/quick-intro-to-consul/
---

For reasons that (hopefully) will become clear in the relatively near future, I decided I needed to take a look at [Consul][link-1], a distributed service discovery tool and key value store. I know Consul's description sounds like a mouthful of buzzwords, but it's pretty accurate. This post provides a quick introduction to Consul, in which I'll break down what Consul does and how it works (at a high level). I'll then build on this introduction in later posts.

There's a lot to Consul, so let's start by breaking down the description of Consul, which I provided as "a distributed service discovery tool and key value store". What does this mean, exactly?

* Consul is _distributed_. This means it runs as a cluster of systems so that there is no single point of failure. Consul uses a gossip protocol (known as [Serf][link-2]) to manage cluster membership, failure detection, and general orchestration. Managing cluster state via Serf is only part of the picture, though; the cluster must also manage consistency via a consensus protocol known as Raft. (Raft is the same consensus protocol used by [etcd][link-3], for example.)
* Consul is also _a service discovery tool_. Applications can register with Consul to provide a service, such as HTTP or MySQL. Other systems can then use Consul, either via HTTP or via DNS, to discover the providers of a given service. Part of this service discovery functionality includes some health checking functionality, to ensure that the provider of a service is actually working as expected.
* Finally, Consul is a _key/value store_. In this respect, you could make a comparison between Consul and etcd, the distributed key/value store bundled with [CoreOS Linux][link-4]. Both Consul and etcd provide HTTP APIs to both store and retrieve key/value data in their distributed key/value store. (If you're unfamiliar with etcd, have a look at [this article I wrote][xref-1].)

## Installing Consul

Installing Consul is pretty easy, actually:

1. Download the Consul software (binaries are provided for 32-bit Linux, 64-bit Linux, Mac OS X, and Windows).

2. Unzip the downloaded file and save to a location of your choice.

That's it.

## Running Consul

_Running_ Consul, on the other hand, is a bit more challenging. This is not a reflection on Consul, but rather a reflection of the complexity that is inherent in distributed systems. The single Consul binary you downloaded above can run either as a server (which actively participates in the consensus protocol) or as a client (which forwards requests to a server). If you are running Consul as a server, you'll need to follow a set of instructions to bring up the Consul servers as a functioning cluster ([see the Consul docs][link-5] for an explanation of how this is done).

If, on the other hand, you're running Consul as a client to connect to an existing Consul cluster, not too much additional is required:

* If you're running Consul (as a client) on a system where Consul is already running (as a server/daemon), you can just run `consul` directly. For example, if you wanted to see a list of the members of the Consul cluster, you could just run `consul members`.
* If you running Consul (as a client) on a system that is _not_ part of the Consul cluster, then you'll need to tell Consul where to find the cluster by adding the `-rpc-addr` parameter. For example, if a host with IP address 10.10.10.10 was part of the Consul cluster, you'd run `consul members -rpc-addr=10.10.10.10:8400` (8400 is the default port for Consul to listen for RPCs) to see a list of the members in the cluster.

## Interacting with Consul

The `consul` command is really only for interacting with the cluster itself, such as to execute a command on the Consul cluster nodes, or to reload a Consul node's configuration. Applications would interact with Consul either via service definition files, via the HTTP API, or via DNS.

* _Service definition files_ are JSON files that tell Consul what services are available and where. (Not familiar with JSON? See [here][xref-2].)
* The HTTP API allows applications to register themselves, or to query Consul for information about a service (or for a list of services). For example, if you want to get a list of all the services about which Consul was aware, you'd just run `curl http://<IP address of Consul cluster node>:8500/v1/catalog/services` (obviously substituting a valid IP address in that command). Optionally, you might want to pipe it through `python -m json.tool` to "prettify" the output. From that list of services, you could then get more information by running `curl http://<IP address of Consul cluster node>:8500/v1/catalog/service/<service name>`. The HTTP API is pretty well-documented [here][link-6].
* Users and applications can also use DNS to discover services. To see this in action for yourself, just run `dig @<IP address of Consul cluster node> <service name>.service.consul` and Consul will happily return DNS information about the hosts/nodes that provide that service. This includes support for DNS SRV records as well, which means Consul can report on the port being used by a particular instance of a service (which might be handy in environments where ports are dynamically assigned...like say environments running Docker containers).

&lt;aside&gt;In my opinion, the DNS support is **huge.** To make it even more powerful, you can configure your DNS servers to forward requests to Consul to make service discovery even more seamless.&lt;/aside&gt;

Now, if you're wondering how you might put this to use in your environment, I'll simply provide one word: _Docker_. In a distributed micro-service architecture, services may pop up on any node on a dynamically assigned port. Using Consul, you could provide a mechanism for discovering these distributed service instances, and you could provide it over DNS, a service _that most applications are using today anyway._ That's pretty powerful, and that's even before we get into stuff like Consul's health checks and such.

So that's a quick introduction to Consul. Have I whetted your appetite for more? I hope so, because I'll be building on this introduction soon. Stay tuned.


[link-1]: https://www.consul.io
[link-2]: https://www.serfdom.io
[link-3]: https://github.com/coreos/etcd/
[link-4]: http://coreos.com/
[link-5]: https://www.consul.io/intro/getting-started/join.html
[link-6]: https://www.consul.io/docs/agent/http.html
[xref-1]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
[xref-2]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}