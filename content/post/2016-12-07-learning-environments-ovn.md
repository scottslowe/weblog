---
author: slowe
categories: Information
comments: true
date: 2016-12-07T00:00:00Z
tags:
- OVS
- OVN
- Vagrant
- Linux
- CLI
- Ubuntu
title: Learning Environments for OVN
url: /2016/12/07/learning-environments-ovn/
---

Over the last few days, I've added two new [Vagrant][link-3]-based learning environments to [my GitHub "learning-tools" repository][link-1], both of them focused on Open Virtual Network (OVN). OVN, if you aren't aware, is part of the [Open vSwitch (OVS)][link-2] project aimed at adding open source network virtualization functionality to OVS. If you're interested in learning more about OVN, you may want to check out these new learning environments.

Here's more details on the two new learning environments:

1. The first one, found in [the "ovn" folder][link-4] of the repository, just builds out a simple three-node OVN 2.6.0 environment running Ubuntu 16.04. This would allow you to run OVN commands like `ovn-nbctl`, `ovn-sbctl`, `ovs-vsctl`, and other related commands to better understand how the components interact with each other and how OVN works.

2. The second environment, found in [the "ovn-docker-ansible" folder][link-5], builds on the first one by adding Docker Engine to each node in the environment and adding the OVN driver for Docker networking. In addition to being able to run various OVS and OVN commands, this environment allows you to build OVN-backed overlay networks between Docker containers running on any node in the environment. Note that this environment uses Docker 1.11.2, as Docker 1.12 and later aren't compatible with most networking drivers (including the OVN driver). However, you _can_ use both Docker Compose and "classic" Docker Swarm clusters with this environment.

I hope that these two environments prove useful to others who are new to OVN and are interested in learning more.



[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: http://openvswitch.org/
[link-3]: https://www.vagrantup.com/
[link-4]: https://github.com/scottslowe/learning-tools/tree/master/ovn
[link-5]: https://github.com/scottslowe/learning-tools/tree/master/ovn-docker-ansible
