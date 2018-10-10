---
author: slowe
categories: Explanation
comments: true
date: 2015-12-17T00:00:00Z
tags:
- Linux
- OpenStack
- Networking
title: Using Cloud-Init to Register an Instance into Consul
url: /2015/12/17/cloud-init-register-instance-into-consul/
---

This post describes a method for using [cloud-init][link-1] to register a cloud instance into [Consul][link-2] on provisioning. I tested this on [OpenStack][link-3], but it should work on any cloud platform that supports metadata services that can be leveraged by cloud-init.

I worked out the details for this method because I was interested in using Consul as a means to provide a form of "dynamic DNS" for OpenStack instances. (You can think of it as service registration and discovery for OpenStack instances.) As I'll point out later in this post, there are a number of problems with this approach, but---if for no other reason---it was helpful as a learning exercise.

The idea was to automatically register OpenStack instances into Consul as they were provisioned. Since Consul offers a DNS interface, other instances and/or workloads could use DNS to look up these nodes' registration. Consul offers an HTTP API (see [here][link-4] for details), so I started there. I used [Paw][link-5] (a tool I described [here][xref-1]) to explore Consul's HTTP API, building the necessary `curl` commands along the way. Once I had the right `curl` commands, the next step was to build a shell script that would pull the current hostname and IP address from the instance.

Once I'd worked out the details for the shell script, it was pretty straightforward to use cloud-init's `write_files` and `runcmd` modules to write the shell script into the instance and then execute it. Here's the snippet of cloud-init configuration needed to make all this work:

``` yaml
#cloud-config
write_files:
  - content: |
      #!/bin/bash

      addr=`ip -4 addr show eth0 | awk '/inet / {print $2}' | cut -d/ -f1`
      host_name=`hostname`
      data="{\"Node\": \"$host_name\",\"Address\": \"$addr\"}"

      curl -X POST "http://consul.example.tld:8500/v1/catalog/register" -d "$data"
    path: /home/ubuntu/register.sh
    permissions: '0755'
runcmd:
  - [ /home/ubuntu/register.sh ]
```

If you supply this snippet of cloud-init configuration to your cloud platform, then the instance will automatically register its hostname and IP address (for the `eth0` interface) into the Consul cluster. You can then use DNS or Consul's HTTP API to retrieve that information.

As I mentioned earlier, there are a couple of flaws with this approach. First, there's no deregistration method---once a node is registered into Consul, this approach doesn't provide a way to de-register the node (remove it from Consul). Second, Consul's DNS interface does have a couple oddities that make  integration into a larger DNS infrastructure less than seamless. Even so, I did find this to be useful as a learning exercise:

* I became much more familiar with Consul's HTTP API;
* I deepened my knowledge of how to use cloud-init; and
* I improved my shell scripting skills.

Besides, there may be a way that others can use this information in some as-yet-unforeseen way; it wouldn't be the first time that's happened. If you do find this information helpful in some way, feel free [to contact me on Twitter][link-6].

[link-1]: http://cloud-init.org/index.html
[link-2]: https://consul.io/
[link-3]: http://www.openstack.org/
[link-4]: https://consul.io/docs/agent/http.html
[link-5]: https://luckymarmot.com/paw
[link-6]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-11-14-handy-gui-tool-working-apis.md" >}}
