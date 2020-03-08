---
author: slowe
categories: Tutorial
comments: true
date: 2016-05-06T00:00:00Z
tags:
- OpenStack
- CoreOS
- Terraform
- Automation
- OSS
- Docker
- etcd
title: Using Terraform to Build an etcd2 Cluster on OpenStack
url: /2016/05/06/using-terraform-etcd2-openstack/
---

In this post I'll build on [my earlier introduction to Terraform][xref-1] to show a practical example of using [Terraform][link-1] to build a [CoreOS][link-3]-based etcd2 cluster on [OpenStack][link-2]. This information is based upon a demo that I created for a session at the Austin OpenStack Summit in late April, so all the files I reference in this post are available in [the GitHub repo for that session][link-5].

You may recall that Terraform is a platform-independent orchestration tool that allows you to create _configurations_ (in either Terraform format or JSON format) specifying resources to be created/modified/deleted. This allows users to take an "infrastructure as code" approach where infrastructure configuration can be declaratively defined and managed via well-known version control mechanisms. In my previous post, I used JSON for the Terraform configurations; in this post, I'll use the "standard" Terraform format.

As in the [intro to Terraform post][xref-1], I'll use three different files:

* The `vars.tf` file, which contains variables we'll reference later
* The `main.tf` file, which has the actual resource definitions
* The `output.tf` file, which will provide some feedback to the user on the resources being created by Terraform (in this case, IP addresses)

Note that there's no provider configuration here; instead, it relies on the use of the appropriate OpenStack environment variables (`OS_PASSWORD`, `OS_AUTH_URL`, etc.) for connecting to OpenStack.

First up is the `vars.tf` file. I won't post the full file here (go see [the GitHub repo][link-5] for the full file), but here's a snippet:

``` text
# Image ID for CoreOS Stable 899.13.0 image
variable "image" {
    default = "6ebd5b1e-f5d5-4bf7-9ca8-713a2696307f"
}
```

In this particular snippet, I'm defining a variable that specifies the image ID for the CoreOS Stable 899.13.0 image in my OpenStack cloud. Also found in the `vars.tf` file but not shown here are definitions for the flavor the instances should use, the external gateway to which a newly-created router should be connected, the floating IP pool from which floating IP addresses should be allocated, and the SSH key pair that should be injected into new instances.

All these values are referenced by `main.tf`, which is the core part of the Terraform configuration. In `main.tf`, the following parts of OpenStack infrastructure are defined:

* A new tenant network, along with a corresponding subnet
* A new tenant logical router, connected to the new tenant network and an external provider network
* Floating IP addresses for each of the five instances to be launched
* Five instances based on the CoreOS image, as defined in `vars.tf`

The contents of `main.tf` are pretty self-explanatory. For example, here's a snippet that attaches a tenant logical router to the newly-created subnet and an external provider network:

``` text
# Attach Swarm router to new Swarm network created earlier
resource "openstack_networking_router_interface_v2" "swarm-rtr-if" {
    router_id = "${openstack_networking_router_v2.swarm-rtr.id}"
    subnet_id = "${openstack_networking_subnet_v2.swarm-subnet.id}"
}
```

Not shown here but also included in `main.tf` and referenced here are the tenant logical router itself (as referenced by `openstack_networking_router_v2.swarm-rtr.id`) and the subnet associated with the new tenant network (referenced by `openstack_networking_subnet_v2.swarm-subnet.id`).

The code for defining the instances is fairly straightforward as well, with one new addition that you may not have seen before:

``` text
resource "openstack_compute_instance_v2" "node-01" {
    name = "node-01"
    image_id = "${var.image}"
    flavor_name = "${var.flavor}"
    key_pair = "${var.key_pair}"
    security_groups = ["default","docker","basic-services","etcd2"]
    floating_ip = "${openstack_compute_floatingip_v2.node-01-fip.address}"
    network {
        uuid = "${openstack_networking_network_v2.swarm-net.id}"
    }
    user_data = "${file(\"cloud-config.yml\")}"
}
```

The new part that you may not have seen before is the `user_data` line, which allows you to provide a cloud-init configuration script to the instance being created. CoreOS has a rather robust implementation of cloud-init (it's one of the things I really like about CoreOS);the file listed there (`cloud-config.yml`) looks something like this:

``` yaml
#cloud-config

coreos:
  etcd2:
    discovery: "https://discovery.etcd.io/2bc6f751df536e597a532221fe0c5bff"
    advertise-client-urls: "http://$public_ipv4:2379"
    initial-advertise-peer-urls: "http://$private_ipv4:2380"
    listen-client-urls: "http://0.0.0.0:2379,http://0.0.0.0:4001"
    listen-peer-urls: "http://$private_ipv4:2380,http://$private_ipv4:7001"
  update:
    reboot-strategy: "etcd-lock"
    group: "stable"
  units:
    - name: "etcd2.service"
      command: "start"
    - name: "docker-tcp.socket"
      command: "start"
      enable: "true"
      content: |
        [Unit]
        Description=TCP socket for the Docker API

        [Socket]
        ListenStream=2375
        Service=docker.service
        BindIPv6Only=both

        [Install]
        WantedBy=sockets.target
```

This cloud-init script does a couple of things:

* It establishes an etcd 2.x cluster. The value of the `discovery` key needs to change for every new cluster you create, so if you wanted to use this configuration for your own purposes be sure to generate your own, valid discovery URL.
* It reconfigures the Docker daemon to also listen on a TCP socket (TCP port 2375). This is not the default on CoreOS.

The last part is the `output.tf` file, which contains only enough information to have Terraform spit out the floating IP address for each instance launched, like this:

``` text
output "addr-node-01" {
    value = "${openstack_compute_floatingip_v2.node-01-fip.address}"
}
```

Putting it all together, what this Terraform configuration does is this:

1. It creates an tenant network, including the necessary subnet for the tenant network.
2. It creates a tenant logical router.
3. It connects the tenant logical router to the newly-created tenant network and to an external provider network. This enables outbound network connectivity for any instances attached to the new tenant network.
4. It allocates 5 floating IP addresses from the supplied floating IP pool.
5. It spins up 5 instances of CoreOS Linux, attaching them to the new tenant network and associating a floating IP address with each instance.
6. It supplies the cloud-init data to the instances, which then reconfigure themselves to run etcd2 and enable network connectivity to the Docker daemon.
7. Finally, it spits out the floating IP address associated with each instance.

Handy!

If you'd like to give this a try in your own OpenStack environment, feel free to clone [the GitHub repo][link-5], which also contains some [Vagrant][link-6], [Ansible][link-7], and [Docker Machine][link-8]-related content along with the Terraform configurations I've described in this post. You can then modify them as needed with your own information. Have fun!



[link-1]: https://www.terraform.io/
[link-2]: http://www.openstack.org/
[link-3]: https://coreos.com/
[link-4]: https://github.com/coreos/etcd/
[link-5]: https://github.com/scottslowe/dev-tools-openstack/
[link-6]: http://www.vagrantup.com/
[link-7]: http://www.ansible.com/
[link-8]: https://www.docker.com/products/docker-machine
[xref-1]: {{< relref "2015-11-25-intro-to-terraform.md" >}}
