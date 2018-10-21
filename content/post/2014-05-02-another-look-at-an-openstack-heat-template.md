---
author: slowe
categories: Explanation
comments: true
date: 2014-05-02T09:00:00Z
slug: another-look-at-an-openstack-heat-template
tags:
- Automation
- Linux
- OpenStack
- OSS
- Interoperability
- Networking
- Virtualization
- YAML
title: Another Look at an OpenStack Heat Template
url: /2014/05/02/another-look-at-an-openstack-heat-template/
wordpress_id: 3448
---

In an earlier post, I provided [an introduction to OpenStack Heat][1], and provided an example Heat template that launched two instances with a logical network and a logical router. Here I am going to provide another view of a Heat template that does the same thing, but uses YAML and the HOT format instead of JSON and the CFN format.

Here's the full template (click [here](https://gist.github.com/scottslowe/1ed38b586a1751138c8d) for this code as a GitHub Gist):

```yaml
heat_template_version: 2013-05-23
description: >
  A simple Heat template that spins up multiple instances and a private network (HOT template in YAML).
resources:
  heat_network_01:
    type: OS::Neutron::Net
    properties:
      admin_state_up: true
      name: heat-network-01
  heat_subnet_01:
    type: OS::Neutron::Subnet
    properties:
      name: heat-subnet-01
      cidr: 10.10.10.0/24
      dns_nameservers: [172.16.1.11, 172.16.1.6]
      enable_dhcp: true
      gateway_ip: 10.10.10.254
      network_id: { get_resource: heat_network_01 }
  heat_router_01:
    type: OS::Neutron::Router
    properties:
      admin_state_up: true
      name: heat-router-01
  heat_router_01_gw:
    type: OS::Neutron::RouterGateway
    properties:
      network_id: 604146b3-2e0c-4399-826e-a18cbc18362b
      router_id: { get_resource: heat_router_01 }
  heat_router_int0:
    type: OS::Neutron::RouterInterface
    properties:
      router_id: { get_resource: heat_router_01 }
      subnet_id: { get_resource: heat_subnet_01 }
  instance0_port0:
    type: OS::Neutron::Port
    properties:
      admin_state_up: true
      network_id: { get_resource: heat_network_01 }
      security_groups:
        - b0ab35c3-63f0-48d2-8a6b-08364a026b9c
  instance1_port0:
    type: OS::Neutron::Port
    properties:
      admin_state_up: true
      network_id: { get_resource: heat_network_01 }
      security_groups:
        - b0ab35c3-63f0-48d2-8a6b-08364a026b9c
  instance0:
    type: OS::Nova::Server
    properties:
      name: heat-instance-01
      image: 01b0eb5d-14ae-4c9e-8025-a21e6f733034
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance0_port0 }
  instance1:
    type: OS::Nova::Server
    properties:
      name: heat-instance-02
      image: 01b0eb5d-14ae-4c9e-8025-a21e6f733034
      flavor: m1.xsmall
      networks:
        - port: { get_resource: instance1_port0 }
```

I won't walk through the whole template again, but rather just talk briefly about a couple of the differences between this YAML-encoded template and the earlier JSON-encoded template:

* You'll note the syntax is _much_ simpler. JSON can trip you up on commas and such if you're not careful; YAML is simpler and cleaner.

* You'll note the built-in functions are different, as I pointed out in [my first Heat post][1]. Instead of using `Ref` to refer to an object defined elsewhere in the template, HOT uses `get_resource` instead.

Aside from these differences, you'll note that the resource types and properties match between the two; this is because resource types are separate and independent from the template format.

Feel free to post any questions, corrections, or clarifications in the comments below. Thanks for reading!

[1]: {{< relref "2014-05-01-an-introduction-to-openstack-heat.md" >}}
