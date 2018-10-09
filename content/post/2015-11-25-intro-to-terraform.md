---
author: slowe
categories: Education
comments: true
date: 2015-11-25T00:00:00Z
tags:
- Automation
- OpenStack
- OSS
- CLI
- AWS
- Terraform
title: An Introduction to Terraform
url: /2015/11/25/intro-to-terraform/
---

In this post, I'm going to provide a quick introduction to [Terraform][link-1], a tool that is used to provision and configure infrastructure. Terraform allows you to define infrastructure configurations and then have those configurations implemented/created by Terraform automatically. In this respect, you could compare Terraform to similar solutions like [OpenStack][link-2] Heat, AWS CloudFormation, and others.

Before I continue, though, allow me to first address this question: _why Terraform?_

## Why Terraform?

This is a fair question, and one that you should be asking. After all, if Terraform is considered similar to OpenStack Heat or AWS CloudFormation, then why use Terraform instead of one of the comparable solutions? I believe there are a couple (related) reasons why you might consider Terraform over a similar solution:

1. Within a single Terraform definition, you can orchestrate across multiple cloud services. For example, you could create instances with a cloud provider (AWS, DigitalOcean, etc.), create DNS records with a DNS provider, and register key/value entries in Consul. Heat and CloudFormation are, quite naturally, designed to work almost exclusively with OpenStack and AWS, respectively. (Astute readers will know that Heat supports CloudFormation templates, but you get the idea.) Therefore, one reason to use Terraform would be because you need to orchestrate multiple services/providers in a single definition.

2. Because Terraform supports multiple services/providers (more on that in a moment), you might choose to use Terraform out of a desire to use a single solution that supports multiple services. Even if you only choose to use a single provider within any given definition, you still have only one solution to learn/master/use/manage/maintain in the event you add more cloud services or more providers at some point in the future. In other words, you might only be supporting OpenStack right now, but by choosing Terraform you're building in a certain level of protection against having to switch solutions when you add more solutions later. (Note that this is subtly different from, although related to, the first reason.)

Now that I've covered the "why," let's take a quick look at how Terraform works.

## Terraform Components

As I've mentioned already, Terraform allows you to create infrastructure configurations that affect resources across multiple cloud services and cloud platforms. Terraform does this with a few different components:

* _Configurations:_ A Terraform configuration is the text file that contains the infrastructure resource definitions. You can write Terraform configurations in either Terraform format (using the `.tf` extension) or in JSON format (using the `.tf.json` extension). In this article, I'm going to use the JSON format. (Why? One, I want to improve my skills in JSON. Two, I found very few good examples of JSON-formatted Terraform configurations, so I want to help remedy that oversight.)
* _Providers:_ Terraform leverages multiple providers to talk to back-end platforms and services, like AWS, Azure, DigitalOcean, Docker, or OpenStack. In this post, I'm going to focus on using Terraform with OpenStack.
* _Resources:_ Resources are the basic building blocks of a Terraform configuration. When you define a configuration, you are defining one or more (typically more) resources. Resources are provider-specific, so a resource for the AWS provider is different than a resource for OpenStack. This, in my opinion, is the one major flaw in Terraform. If you wanted to convert a configuration from using AWS to using OpenStack (or vice versa), you'd essentially have to re-create the configuration because all the resources are provider-specific.
* _Variables:_ To help make configurations more portable and more flexible, Terraform supports the use of variables. By changing the value of the variables, you could potentially re-use a single configuration multiple times (but not across providers, since resources are provider-specific).

Perhaps an example will help. Let's take a look at an example Terraform configuration that will spin up an environment in OpenStack.

## Example Terraform Configuration

This example Terraform configuration will perform the following tasks:

- Create a new Neutron network and subnet.
- Create a new Neutron logical router connected the new network and with an uplink to an external network.
- Create a new instance, assign some security groups, and associate a floating IP.

To do this, we'll split the information Terraform needs into four different files:

    provider.tf.json
    vars.tf.json
    main.tf.json
    output.tf.json

The `provider.tf.json` file contains information specific to Terraform's OpenStack provider. Splitting this into a separate file makes it easier to share the Terraform configuration without sharing confidential credentials.

The `vars.tf.json` file contains variables that will be used later by Terraform. Putting as much information as possible into a separate variables file makes the Terraform configuration more reusable.

The `main.tf.json` file contains the bulk of the Terraform configuration.

Finally, the `output.tf.json` file contains information that should be output to the operator.

Let's start with the `provider.tf.json` file. For the OpenStack provider, it should look something like this:

``` json
{
    "provider": {
        "openstack": {
            "user_name": "demo",
            "tenant_name": "demo",
            "password": "password",
            "auth_url": "http://openstack.domain.net:5000/v2.0"
        }
    }
}
```

Technically, this file isn't required; Terraform can leverage OpenStack-specific environment variables (like OS_AUTH_URL and similar) that are typically required by the OpenStack command-line clients anyway.

Next up is the variables file, `vars.tf.json`. This is the file that would change most between uses of the configuration, since it contains the information specific to that particular use case. Here's an example of what it might look like:

``` json
{
    "variable": {
        "image": {
            "default": "Ubuntu 14.04.3 LTS x64"
        }
    },
    "variable": {
        "flavor": {
            "default": "m1.small"
        }
    },
    "variable": {
        "external_gateway": {
            "default": "c842f32c-f583-4d6a-af34-73846790c2f5"
        }
    },
    "variable": {
        "key_pair": {
            "default": "homelab"
        }
    },
    "variable": {
        "pool": {
            "default": "ext-net-5"
        }
    }
}
```

On its own, this file doesn't make a whole lot of sense. However, when viewed in light of the main configuration file (`main.tf.json`) it makes a lot more sense:

``` json
{
    "resource": {
        "openstack_networking_network_v2": {
            "tf-net": {
                "name": "tf-net",
                "admin_state_up": "true"
            }
        }
    },
    "resource": {
        "openstack_networking_subnet_v2": {
            "tf-subnet": {
                "name": "tf-subnet",
                "network_id": "${openstack_networking_network_v2.tf-net.id}",
                "cidr": "192.168.200.0/24",
                "ip_version": 4,
                "dns_nameservers": [
                    "8.8.8.8",
                    "8.8.4.4"
                ]
            }
        }
    },
    "resource": {
        "openstack_networking_router_v2": {
            "tf-router": {
                "name": "tf-router",
                "admin_state_up": "true",
                "external_gateway": "${var.external_gateway}"
            }
        }
    },
    "resource": {
        "openstack_networking_router_interface_v2": {
            "tf-router-iface": {
                "router_id": "${openstack_networking_router_v2.tf-router.id}",
                "subnet_id": "${openstack_networking_subnet_v2.tf-subnet.id}"
            }
        }
    },
    "resource": {
        "openstack_compute_floatingip_v2": {
            "tf-fip": {
                "pool": "${var.pool}",
                "depends_on": [
                    "openstack_networking_router_interface_v2.tf-router-iface"
                ]
            }
        }
    },
    "resource": {
        "openstack_compute_instance_v2": {
            "tf-instance": {
                "name": "tf-instance",
                "image_name": "${var.image}",
                "flavor_name": "${var.flavor}",
                "key_pair": "${var.key_pair}",
                "floating_ip": "${openstack_compute_floatingip_v2.tf-fip.address}",
                "security_groups": [
                    "default",
                    "basic-services"
                ],
                "network": {
                    "uuid": "${openstack_networking_network_v2.tf-net.id}"
                }
            }
        }
    }
}
```

Let's break this down a little bit:

* The `openstack_networking_network_v2` resource creates a Neutron network.
* The `openstack_networking_subnet_v2` resource creates a subnet and associates the subnet with the new Neutron network (via the `network_id` value, which you can see refers back to the network object's ID).
* The `openstack_networking_router_v2` resource creates a logical router. This is where our first variable comes into play, where Terraform uses the `var.external_gateway` variable (taken from `vars.tf.json`) to assign the uplink (external network) for the logical router.
* The `openstack_networking_router_interface_v2` resource creates a router interface connecting the logical router to the new Neutron network created earlier. It does this by referencing IDs from those resources (`openstack_networking_router_v2.tf-router.id` references the logical router, and `openstack_networking_subnet_v2.tf-subnet.id` references the subnet).
* The `openstack_compute_floatingip_v2` resource creates a new floating IP address from an external address pool (where it uses another variable again).
* Finally, the `openstack_compute_instance_v2` resource creates a compute instance, leveraging variables to define the image, flavor, and SSH key pair. Other resources are referenced, like the Neutron network and floating IP address, and some values---the security groups---are hard-coded in the configuration.

Somewhat anti-climactically, the final file in our solution is `output.tf.json`, which simply defines what information Terraform will display when the configuration is applied. Here's a sample `output.tf.json` file:

``` json
{
    "output": {
        "address": {
            "value": "${openstack_compute_floatingip_v2.tf-fip.address}"
        }
    }
}
```

In this case, the output is only the IP address created by the `openstack_compute_floatingip_v2` resource and assigned to the new compute instance. You could, naturally, include more information to be displayed here.

## Using the Example Configuration

To use this example Terraform configuration, you'd place these files into a directory on a system that has Terraform installed, and simply run `terraform apply`. (This is after you've made sure the files have the correct information for your environment, naturally.) Terraform will apply the configuration, creating and modifying resources as needed, and when it is finished it will display the information specified in `output.tf.json` to the screen.

Later, if you needed to modify the environment, you could edit the configuration files and run `terraform apply` again. Terraform would make the necessary changes to have the environment conform to the configuration. (Note that this may entail deleting and re-creating resources, so be sure to double-check [the documentation][link-3] for the resources you're using to understand Terraform's behavior.)

If you put the configuration files into a source control repository (using something like Git---see [here][xref-1] for a Git intro), then now you have versioned infrastructure configurations, and you can track changes, rollback changes, etc.

When you're finished with the environment, you run `terraform destroy` and Terraform will tear down the resources contained in the configuration.

## Summary

There's more to Terraform than what I've covered here, but this should at least get you started. To help you along in case you want to experiment with Terraform, I've placed both Terraform-formatted and JSON-formatted configurations in the "terraform" directory of [my GitHub learning-tools repository][link-4]. Feel free to use those as a starting point for your own exploration.

[link-1]: http://terraform.io
[link-2]: http://www.openstack.org
[link-3]: https://terraform.io/docs/
[link-4]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2015-01-14-non-programmer-git-intro.md" >}}
