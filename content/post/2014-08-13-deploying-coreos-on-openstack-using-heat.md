---
author: slowe
categories: Tutorial
comments: true
date: 2014-08-13T09:00:00Z
slug: deploying-coreos-on-openstack-using-heat
tags:
- Automation
- CLI
- Linux
- OpenStack
- CoreOS
- Virtualization
title: Deploying CoreOS on OpenStack Using Heat
url: /2014/08/13/deploying-coreos-on-openstack-using-heat/
wordpress_id: 3495
---

In this post, I'm going to illustrate one way to deploy [CoreOS](http://coreos.com/) on [OpenStack](http://www.openstack.org/) using Heat. By no means is this intended to be seen as the _only_ way to use Heat to deploy CoreOS, but rather as _one_ way of using Heat to deploy CoreOS. I'm publishing this in the hopes that others will be able to use this as a building block for their own deployments.

If you aren't already familiar with OpenStack Heat or CoreOS, you might want to take a moment and refer to this introductory posts for some foundational information:

* [An Introduction to OpenStack Heat][1]

* [Another Look at an OpenStack Heat Template][2]

* [A Quick Introduction to CoreOS][3]

Moving forward, OpenStack Heat is trying to standardize on OpenStack resource types (like `OS::Nova::Server`) and the HOT format (using YAML). Therefore, the Heat template I'm presenting here will use OpenStack resource types and YAML. Note that it's certainly possible to do this using CloudFormation (CFN) resource types and JSON formatting. I'll leave the conversion of the template found here into CFN/JSON as an exercise for the readers.

Here's the example Heat template you can use to deploy and customize CoreOS on OpenStack:

``` yaml
heat_template_version: 2013-05-23
description: >
  A simple Heat template to deploy CoreOS into an existing cluster.
parameters:
  network_id:
    type: string
    label: Network ID
    description: ID of existing Neutron network to use
    default: <ID of Neutron network to which instances should connect>
  image_id:
    type: string
    label: Glance Image ID
    description: ID of existing Glance image to use
    default: <ID of CoreOS Glance image>
resources:
  instance0_port0:
    type: OS::Neutron::Port
    properties:
      admin_state_up: true
      network_id: { get_param: network_id }
      security_groups:
        - <ID of security group to apply to this Neutron port>
  instance0:
    type: OS::Nova::Server
    properties:
      name: coreos-04
      image: { get_param: image_id }
      flavor: m1.small
      networks:
        - port: { get_resource: instance0_port0 }
      key_name: <Name of SSH key to inject into CoreOS instance>
      user_data_format: RAW
      user_data: |
        #cloud-config
        coreos:
          etcd:
            discovery: https://discovery.etcd.io/<unique cluster ID here>
            addr: $private_ipv4:4001
            peer-addr: $private_ipv4:7001
          units:
            - name: etcd.service
              command: start
            - name: fleet.service
              command: start
```

(Click [here](https://gist.github.com/scottslowe/43ea98cf49ff91445d0f) to view the code block above as a GitHub Gist.)

Let's walk through this template real quick:

* On line 9, you'll need to provide the ID for the Neutron network to which the new CoreOS instance(s) should connect. You can get this a couple of different ways; running `neutron net-list` is one way.

* On line 14, you'll need to supply the ID for the CoreOS image you've uploaded into Glance. Again, there are multiple ways to obtain this; running `glance image-list` is one way of getting that information.

* On line 22, replace the text (including the "<" and ">" symbols) with the ID of the security group you want applied to the CoreOS instance(s) being deployed. The `neutron security-group-list` command can give you the information you need to put here.

* On line 31, supply the name of the SSH key you want to inject into the instance(s).

* On line 37, you'll need to generate a unique cluster ID to place here for the configuration of etcd within the CoreOS instance(s). You can generate a new ID (also called a _token_) by visiting `https://discovery.etcd.io/new`. That will return another URL that contains the new etcd cluster token. Supply that token here to create a new etcd cluster out of the CoreOS instance(s) you're deploying with this template.

* This template only deploys a single CoreOS instance. To deploy multiple CoreOS instances, you'll need a separate `OS::Neutron::Port` and `OS::Nova::Server` resource for each instance. For each Neutron port, you can reference the same security group ID and network ID. For each instance, you can reference the same Glance image ID, same SSH key, and same etcd cluster token; the only thing that would change with each instance is line 30. Line 30 should point to a unique Neutron port resource created for each instance (something like `instance1_port0`, `instance2_port0`, etc.).

Now, there are obviously lots of other things you could do here---you could create your own Neutron network to host these CoreOS instances, you could create a logical router to provide external connectivity (which _is_ required, by the way, in order for the etcd cluster token discovery to work correctly), and you could create and assign floating IPs to the instances. Examples of some of these tasks are in the articles I provided earlier; others are left as an exercise for the reader. (Or I'll write up something later. We'll see.)

Once you have your template, you can deploy the stack using Heat, and then---after your CoreOS cluster is up and running---begin to deploy applications to the cluster using tools like fleet. That, my friends, is another story for another day.

Any questions? Corrections? Clarifications? Feel free to start (or join) the discussion below. All courteous comments are welcome.

[1]: {{< relref "2014-05-01-an-introduction-to-openstack-heat.md" >}}
[2]: {{< relref "2014-05-02-another-look-at-an-openstack-heat-template.md" >}}
[3]: {{< relref "2014-08-01-a-quick-introduction-to-coreos.md" >}}
