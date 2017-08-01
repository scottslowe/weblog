---
author: slowe
categories: Tutorial
comments: true
date: 2015-09-24T00:00:00Z
tags:
- OpenStack
- Docker
- CLI
- Linux
title: Using Docker Machine with OpenStack
url: /2015/09/24/using-docker-machine-with-openstack/
---

In this post, I'm going to show you how to use [Docker Machine][link-3] with [OpenStack][link-4]. This is something I've been interested in testing for a while, and now that I  _finally_ have my test lab back up and running, I was able to spend some time on this. I'll spend some time later in the post covering my reasons for wanting to look at this, but I'll start with the technical content of how it works.

I tested this setup with the following components:

* The client system was running OS X 10.9.5 with the Docker 1.8.2 client binary and Docker Machine 0.4.1.
* The OpenStack cloud was running the Juno release on Ubuntu 14.04 LTS, KVM hypervisors, and VMware NSX for networking.

There are (at least) two approaches to using Docker Machine and OpenStack together:

1. You can use [Docker Machine's `generic` driver][link-2] to consume already-provisioned OpenStack instances. This is, in large part, very similar to what I covered [here][xref-1], but I'll cover it in this post just for the sake of completeness.
2. You can use [Docker Machine's `openstack` driver][link-1] to automatically provision and configure new instances on an OpenStack cloud. This is the more interesting approach, in my opinion.

Let's take a look at each approach.

## Using the Generic Driver

In [this earlier post][xref-1], I showed how to use the Docker Machine `generic` driver with a Vagrant-managed local VM. Using the `generic` driver with an existing OpenStack instance is largely the same:

    docker-machine create -d generic \
    --generic-ssh-user ubuntu \
    --generic-ssh-key ~/.ssh/openstack.pem \
    --generic-ip-address 192.168.100.1 \
    dm-generic

This example is taken from using the `generic` driver to consume an Ubuntu instance; hence, the value of "ubuntu" for the `--generic-ssh-user` parameter. Obviously, you'd need to change values around for your environment:

* You'll need to replace the value of `--generic-ssh-user` with the name of the default user for your cloud instance.
* You'll need to replace the SSH key specified in `--generic-ssh-key` to the correct key for your environment.
* You'll need to supply the correct IP address via the `--generic-ip-address` parameter. This will need to be an IP address that is reachable from the system where you're running Docker Machine, so you might need to associate a floating IP address to the instance first.
* You'll need to ensure that the instance's assigned security groups will allow SSH and Docker (TCP 2375 and TCP 2376) traffic. Otherwise, Docker Machine will fail to communicate with the Docker Engine.

Assuming you have all the right values, this will create a new engine in Docker Machine that corresponds to an existing, already-provisioned OpenStack instance. Note that this is super-simple in that it doesn't require any OpenStack-specific information---just the username, SSH key, and IP address. You could provision these instances using a Heat template, if you so preferred, and then prep them for use with Docker using Docker Machine.

There's a couple things to note about this:

* If the existing OpenStack instance already has the Docker Engine installed, then Docker Machine will reconfigure the Docker Engine to support remote use (enable listening on TCP port 2376 and configure/enable TLS).
* If the existing OpenStack instance doesn't have the Docker Engine installed, then Docker Machine will install Docker Engine and configure it to support remote use (enable listening on TCP port 2376 and configure/enable TLS).

Note that I did run into a couple of odd issues using the official Ubuntu 14.04.3 cloud image with Docker already installed. In this case, Docker Machine rewrites the contents of `/etc/default/docker`, adding (among other things) `--storage-driver aufs`. For whatever reason (I haven't yet been able to figure out why), this causes the Docker daemon to crash. Only by removing `--storage-driver aufs` (and thus reverting to the Device Mapper driver) was I able to make the Docker daemon run as expected. For now, I'd recommend allowing Docker Machine to provision the Docker Engine onto existing OpenStack instances, which worked flawlessly in my testing.

## Using the OpenStack Driver

As I mentioned earlier, I think this is actually the more interesting approach to use (and I'll discuss why farther down in this post). In this approach, Docker Machine will launch an OpenStack instance on behalf of the user and install/configure Docker on that instance.

Here's the syntax for using the `openstack` driver for Docker Machine:

    docker-machine create -d openstack \
    --openstack-username <OpenStack username of user> \
    --openstack-password <password of OpenStack user account> \
    --openstack-tenant-name <name of OpenStack tenant> \
    --openstack-auth-url http://<IP address of controller>:5000/v2.0 \
    --openstack-flavor-id <ID of flavor to use> \
    --openstack-image-id <ID of image to use> \
    --openstack-net-name <name of private network> \
    --openstack-floatingip-pool <name of external network> \
    --openstack-ssh-user <default username> \
    --openstack-sec-groups <comma separated list of security groups> \
    <name>

Wow...that's quite a bit more complicated than using the `generic` driver! You can simplify the command line a bit by setting some commonly-used OpenStack environment variables:

* If you set `OS_USERNAME` you can omit `--openstack-username`.
* If you set `OS_PASSWORD` you can leave out `--openstack-password`.
* If you set `OS_TENANT_NAME` then you don't need `--openstack-tenant-name`.
* If you set `OS_AUTH_URL` you don't have to include `--openstack-auth-url`.

Since you may have these environment variables already set for use with the various OpenStack CLI clients, the OpenStack driver for Docker Machine simply takes advantage of them.

However, you still need the rest of the command-line parameters, and this requires a good deal bit more OpenStack-specific knowledge/experience. You'll need to determine the flavor ID (or name), the image ID (or name), the floating IP pool in use, the name of the Neutron network, and the name(s) of the security groups to use. All of this information is easily accessible via the OpenStack CLI clients or the OpenStack dashboard, but it's still a lot of information to gather.

So what happens when you run this command?

* Docker Machine authenticates against OpenStack (using the URL, username, password, and tenant specified in environment variables or on the command line).
* Docker Machine launches a new instance (using the image, flavor, and network information) and associates a floating IP to the instance (using the floating IP pool information). A new key is generated for access to this instance.
* Once the instance is accessible via SSH, Docker Machine connects to the instance, installs Docker Engine, and configures it for remote use (listens on TCP port 2375, enables and configures TLS).

The nice thing about doing it this way is that when you're done, `docker-machine rm` not only removes the local reference to the instance, but _also_ removes the instance from the OpenStack cloud. In other words, it cleans up after itself.

(By the way, there's nothing saying you can't mix and match both the `generic` driver and the `openstack` driver in the same environment.)

## Why Docker Machine and OpenStack?

Some readers may be wondering why one might use Docker Machine in an OpenStack environment. After all, doesn't Docker make OpenStack obsolete? (That was sarcasm, in case you didn't notice.)

Here's my line of thinking. OpenStack is really more of an _infrastructure orchestration tool_, designed for provisioning infrastructure (VMs, volumes, networks, routers, etc.). Will it evolve over time to be more "application-aware"? Probably, but that's not where it is today. Docker, on the other hand, is more an an _application provisioning tool_, designed to simplify how developers, operators, and organizations deploy applications. Will it evolve over time to be more involved in infrastructure? Most likely, but that's not really where its strengths are found today. Docker needs infrastructure, and OpenStack supplies infrastructure. By combining Docker Machine and OpenStack, you create what is (in my opinion) a nice "consumer/provider" relationship between OpenStack (which provides infrastructure) and Docker (which consumes infrastructure to deploy applications).

Anyway, that's my 2 cents. I hope you found this information to be helpful, and feel free to hit me up on Twitter or drop me an e-mail if you have any feedback. Thanks!

**UPDATE:** Nathan Ness pointed out that there was an error in the command for the OpenStack driver; it should be `--openstack-auth-url` instead of `--openstack_auth_url`. I've corrected the post. Thanks!



[link-1]: https://docs.docker.com/machine/drivers/openstack/
[link-2]: https://docs.docker.com/machine/drivers/generic/
[link-3]: https://docs.docker.com/machine/
[link-4]: http://www.openstack.org/
[xref-1]: {{< relref "2015-08-04-using-vagrant-docker-machine-together.md" >}}
