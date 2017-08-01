---
author: slowe
categories: Explanation
comments: true
date: 2015-12-04T00:00:00Z
tags:
- Linux
- SSH
- Networking
- Security
- OpenStack
title: One Use Case for an SSH Bastion Host
url: /2015/12/04/use-case-ssh-bastion-host/
---

In this post, I'm going to explore one specific use case for using an SSH bastion host. I described [this configuration and how to set it up][xref-1] in a previous post; in this post, though, I'd like to focus on one practical use case.

This use case is actually one I depicted graphically in [my earlier post][xref-1]:

![SSH bastion host diagram](/public/img/ssh-bastion-host.png)

This diagram could represent a couple different examples. For example, perhaps this is an AWS VPC. Security best practices suggest that you should limit access from the Internet to your instances as much as possible; unless an instance needs to accept traffic from the Internet, don't assign a public IP address (or an Elastic IP address). However, without a publicly-accessible IP address, how does one connect to and manage the instance? You can't SSH to it without a publicly-accessible IP address---unless you use an SSH bastion host.

Or perhaps this diagram represents an [OpenStack][link-3] private cloud, where users can deploy instances in a private tenant network. In order for those instances to be accessible externally (where "externally" means external to the OpenStack cloud), the tenant must assign each instance a floating IP address. Security may not be as much of a concern here, since "external" access to the OpenStack cloud is probably still originating within the corporate firewalls. Nevertheless, users may still want a way to deploy instances _without_ having to assign a floating IP address for every instance. Leveraging an SSH bastion host allows this sort of configuration while still enabling SSH access to the private instances. (Face it, console access just isn't enough.)

Looking more at this second example (an OpenStack private cloud), you might be thinking that this sort of configuration---having instances to which you don't want to assign a floating IP address---is kind of rare. Just off the top of my head, I can think of a couple different cases where this makes sense to me:

1. Let's say you want to set up a [Docker Swarm][link-1] cluster backed by [etcd][link-2]. The etcd cluster is an essential part of running a Swarm cluster, but it's really only being used by the Swarm cluster, and doesn't need to be accessible from any other tenant network. Why assign floating IP addresses (which go against your quota) if it's not necessary?
2. Another example might be a multi-tier application being deployed by a tenant, where the web tier needs floating IP addresses but the application and database tiers do not (after all, the application and database tiers are not accessed by any external sources, only the web tier).

In both these examples, you're going to want (dare I say _need_?) some type of management access to these instances that don't have floating IP addresses assigned. While there may be situations where using multiple networks---such as management network and an application network---is the right solution, I believe that in most situations the use of an SSH bastion host satisfies the requirements, is simpler, is well-understood, and is easier to troubleshoot.

Hopefully this post has provided some insight and information on _when_ and _why_ you might find an SSH bastion host a useful addition to your design. If you have any questions, feel free [to hit me up on Twitter][link-4]. Thanks!



[link-1]: http://docs.docker.com/swarm/
[link-2]: https://github.com/coreos/etcd
[link-3]: http://openstack.org/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-11-21-using-ssh-bastion-host.md" >}}
