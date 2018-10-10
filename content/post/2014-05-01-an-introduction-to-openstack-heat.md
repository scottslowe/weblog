---
author: slowe
categories: Education
comments: true
date: 2014-05-01T09:00:00Z
slug: an-introduction-to-openstack-heat
tags:
- Automation
- JSON
- Linux
- OpenStack
- OSS
- Interoperability
- Networking
- Virtualization
title: An Introduction to OpenStack Heat
url: /2014/05/01/an-introduction-to-openstack-heat/
wordpress_id: 3446
---

In this post, I'm going to provide a quick introduction to OpenStack Heat, the orchestration service that allows you to spin up multiple instances, logical networks, and other cloud services in an automated fashion. Note that this is only an introductory post---I'm not an expert on Heat, but I did want to share at least some basic information to help others get started as well.

Let's start with some terminology, so that there is no confusion about the terms later when we start using them in specific examples:

* _Stack:_ In Heat parlance, a stack is the collection of objects---or _resources_---that will be created by Heat. This might include instances (VMs), networks, subnets, routers, ports, router interfaces, security groups, security group rules, auto-scaling rules, etc.

* _Template:_ Heat uses the idea of a template to define a stack. If you wanted to have a stack that created two instances connected by a private network, then your template would contain the definitions for two instances, a network, a subnet, and two network ports. Since templates are central to how Heat operates, I'll show you examples of templates in this post.

* _Parameters:_ A Heat template has three major sections, and one of those sections defines the template's parameters. These are tidbits of information---like a specific image ID, or a particular network ID---that are passed to the Heat template by the user. This allows users to create more generic templates that could potentially use different resources.

* _Resources:_ Resources are the specific objects that Heat will create and/or modify as part of its operation, and the second of the three major sections in a Heat template.

* _Output:_ The third and last major section of a Heat template is the output, which is information that is passed to the user, either via OpenStack Dashboard or via the `heat stack-list` and `heat stack-show` commands.

* _HOT:_ Short for Heat Orchestration Template, HOT is one of two template formats used by Heat. HOT is not backwards-compatible with AWS CloudFormation templates and can only be used with OpenStack. Templates in HOT format are typically---but not necessarily required to be---expressed as YAML (more information on YAML [here](http://yaml.org)). (I'll do my best to avoid saying "HOT template," as that would be redundant, wouldn't it?)

* _CFN:_ Short for AWS CloudFormation, this is the second template format that is supported by Heat. CFN-formatted templates are typically expressed in JSON (see [here](http://json.org) and see [my non-programmer's introduction to JSON][1] for more information on JSON specifically).

OK, that should be enough to get us going. (BTW, the OpenStack Heat documentation actually has [a really good glossary](http://docs.openstack.org/developer/heat/glossary.html). Please note that this link might break as OpenStack development continues.)

Architecturally, Heat has a few major components:

* The `heat-api` component implements an OpenStack-native RESTful API. This components processes API requests by sending them to the Heat engine via AMQP.

* The `heat-api-cfn` component provides an API compatible with AWS CloudFormation, and also forwards API requests to the Heat engine over AMQP.

* The `heat-engine` component provides the main orchestration functionality.

All of these components would typically be installed on an OpenStack "controller node" that also houses the API servers for Nova, Glance, Neutron, etc. As far as I know, though, there is nothing that requires them to be installed on the same system. Like most of the rest of the OpenStack services, Heat uses a back-end database for maintaining state information.

Now that you have an idea about Heat's architecture, I'll walk you through an example template that I created and tested on my own OpenStack implementation (running OpenStack Havana on Ubuntu 12.04 with KVM and VMware NSX). Here's the full template:

``` json
{
  "AWSTemplateFormatVersion" : "2010-09-09",
  "Description" : "Sample Heat template that spins up multiple instances and a private network (JSON)",
  "Resources" : {
    "heat_network_01" : {
      "Type" : "OS::Neutron::Net",
      "Properties" : {
        "name" : "heat-network-01"
      }
    },
 
    "heat_subnet_01" : {
      "Type" : "OS::Neutron::Subnet",
      "Properties" : {
        "name" : "heat-subnet-01",
        "cidr" : "10.10.10.0/24",
        "dns_nameservers" : ["172.16.1.11", "172.16.1.6"],
        "enable_dhcp" : "True",
        "gateway_ip" : "10.10.10.254",
        "network_id" : { "Ref" : "heat_network_01" }
      }
    },
 
    "heat_router_01" : {
      "Type" : "OS::Neutron::Router",
      "Properties" : {
        "admin_state_up" : "True",
        "name" : "heat-router-01"
      }
    },
 
    "heat_router_01_gw" : {
      "Type" : "OS::Neutron::RouterGateway",
      "Properties" : {
        "network_id" : "604146b3-2e0c-4399-826e-a18cbc18362b",
        "router_id" : { "Ref" : "heat_router_01" }
      }
    },
 
    "heat_router_int0" : {
      "Type" : "OS::Neutron::RouterInterface",
      "Properties" : {
        "router_id" : { "Ref" : "heat_router_01" },
        "subnet_id" : { "Ref" : "heat_subnet_01" }
      }
    },
 
    "instance0_port0" : {
      "Type" : "OS::Neutron::Port",
      "Properties" : {
        "admin_state_up" : "True",
        "network_id" : { "Ref" : "heat_network_01" },
        "security_groups" : ["b0ab35c3-63f0-48d2-8a6b-08364a026b9c"]
      }
    },
 
    "instance1_port0" : {
      "Type" : "OS::Neutron::Port",
      "Properties" : {
        "admin_state_up" : "True",
        "network_id" : { "Ref" : "heat_network_01" },
        "security_groups" : ["b0ab35c3-63f0-48d2-8a6b-08364a026b9c"]
      }
    },
 
    "instance0" : {
      "Type" : "OS::Nova::Server",
      "Properties" : {
        "name" : "heat-instance-01",
        "image" : "73669ac0-8677-498d-9d97-76af287bcf32",
        "flavor": "m1.xsmall",
        "networks" : [{
          "port" : { "Ref" : "instance0_port0" }
        }]
      }
    },
 
    "instance1" : {
      "Type" : "OS::Nova::Server",
      "Properties" : {
        "name" : "heat-instance-02",
        "image" : "73669ac0-8677-498d-9d97-76af287bcf32",
        "flavor": "m1.xsmall",
        "networks" : [{
          "port" : { "Ref" : "instance1_port0" }
        }]
      }
    }
  }
}
```

(Want a downloadable version of the code above? Click [here](https://gist.github.com/scottslowe/485fa69644d646052149) to see it as a GitHub Gist.)

Let's walk through this template real quick:

* First, note that I've specified the template version as "AWSTemplateFormatVersion". One thing that confused me at first was the relationship between the template format (CFN vs. HOT) and resource types. It turns out these are independent of one another; you can---as I have done here---use HOT resource types (like `OS::Neutron::Net`) in a CFN template. Obviously, if you use HOT resources you're not fully compatible with AWS. Also, as I stated earlier, CFN templates are typically expressed in JSON (as mine is). Heat does support YAML for CFN templates, although again you'd be sacrificing AWS compatibility.

* You'll note that my template skips any use of parameters and goes straight to resources. This is perfectly acceptable, although it means that some values (like the shared public provider network to which the logical router uplinks and the security group) have to be hard-coded in the template.

* One thing that the template format _does_ control is some of the syntax. So, for example, you'll note the template uses "Resources", "Type", and "Properties." In some of the other template formats, these could be specified lowercase.

* The first resource defined is a logical network, defined as type `OS::Neutron::Net`.

* The next resource is a subnet (of type `OS::Neutron::Subnet`), which is associated with the previously-defined logical network through the use of the `Ref` built-in function on line 20. Built-in functions are another thing controlled by the template format, so when you want to refer to another object in a CFN template, you'll use the `Ref` function as I did here. This associates the "network_id" property of the subnet with the logical network defined just prior. You'll also note that the subnet resource has a number of properties associated with it---CIDR, DNS name servers, DHCP, and gateway IP address.

* The third resource defined is a logical router.

* After the logical router is defined, the template links the logical router to a pre-existing provider network via the `OS::Neutron::RouterGateway` type. (This was deprecated in Icehouse in favor of an `external_gateway_info` property on the logical router.) The UUID listed there is the UUID of a pre-existing provider network. Note the use of the `Ref` function again to link this resource back to the logical router.

* Next up the template creates an interface on the logical router, using two `Ref` instances to link this router interface back to the logical router and the subnet created earlier. This means we are adding an interface to the referenced logical router on the specified subnet (and that interface will assume the IP address specified by the "gateway_ip" property on the subnet).

* Next the template creates two Neutron ports, and links them to the default security group. Note that if you don't specify a security group when creating the Neutron port, it will have none---and no traffic will pass.

* Finally, the Heat template creates two instances (type `OS::Nova::Server`), using the "m1.xsmall" flavor and a hard-coded Glance image ID. These instances are connected to the Neutron ports created earlier using the `Ref` function once more.

(In case it wasn't obvious already, you can't just copy-and-paste this Heat template and use it in your own environment, as it references UUIDs for objects in _my_ environment that won't be the same.)

If you are going to use JSON (as I have here), then I'd recommend bookmarking a JSON validation site, such as [jsonlint.com](http://jsonlint.com).

Once you have your Heat template defined, you can then use this template to create a stack, either via the `heat` CLI client or via the OpenStack Dashboard. I'll attach a screenshot from a stack that I deployed via the Dashboard so that you can see what it looks like (click the image for a larger version):

[![A deployed Heat stack in OpenStack Dashboard](/public/img/heat-screenshot-small.png)](/public/img/heat-screenshot.png)

Kinda nifty, don't you think? Anyway, I hope this brief introduction to OpenStack Heat has proven useful. I do plan on covering some additional topics with OpenStack Heat in the near future, so stay tuned. In the meantime, if you have any questions, corrections, or clarifications, I invite you to add them to the comments below.

[1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
