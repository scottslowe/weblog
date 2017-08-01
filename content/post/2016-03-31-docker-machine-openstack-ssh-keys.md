---
author: slowe
categories: Information
comments: true
date: 2016-03-31T00:00:00Z
tags:
- Docker
- OpenStack
- SSH
- Security
title: Docker Machine, OpenStack, and SSH Keys
url: /2016/03/31/docker-machine-openstack-ssh-keys/
---

I wanted to provide readers a quick "heads up" about some unexpected behavior regarding [Docker Machine][link-1] and [OpenStack][link-2]. It's not a huge deal, but it could catch someone off-guard if they aren't aware of what's happening.

This post builds on the earlier post I published on [using Docker Machine with OpenStack][xref-1]; specifically, the section about using Docker Machine's native OpenStack driver to provision instances on an OpenStack cloud. As a quick recap, recall that you can provision instances on an OpenStack cloud (and have Docker Engine installed and configured on those instances) with a command like this:

``` bash
docker-machine create -d openstack \
--openstack-flavor-id 3 \
--openstack-image-name "Ubuntu 14.04.3 LTS x64" \
--openstack-net-name lab-net-5 \
--openstack-floatingip-pool ext-net-5 \
--openstack-sec-groups docker,basic-services
instance-name
```

(Note that I didn't include _all_ of the optional parameters; refer to either [my earlier blog post][xref-1] or [the Docker Machine OpenStack driver reference][link-3] for more details).

One of the optional parameters for Docker Machine's OpenStack driver is the `--openstack-keypair-name` parameter, which allows you to specify the name of an existing keypair to use with instances created by Docker Machine. If you omit this parameter, as I have above, then Docker Machine will auto-generate a new SSH key and inject that into the instance upon provisioning. Then, when you use `docker-machine rm <instance-name>` to remove the instance, Docker Machine will terminate the instance _and_ delete the auto-generated SSH key. So far, so good---everything seems normal and pretty straightforward.

The unexpected behavior comes when you use the `--openstack-keypair-name` parameter to tell Docker Machine to use an existing keypair. This might be handy if you already have a keypair in OpenStack that you'd like to use with Docker Machine-generated instances. Docker Machine will do just as you instruct---it will use the existing keypair and inject it into the instance. And, when you run `docker-machine rm <instance-name>` to remove the Docker Machine-generated instance, it _will also remove the pre-existing keypair._

Yes, you saw that right: Docker Machine will remove the pre-existing SSH keypair when you remove the Docker Machine-generated instance. Then, at some point later when you use something that expects that keypair to be there---the `nova` command line, Vagrant with an OpenStack provider, a Heat template, or a Terraform configuration---you'll get an error because the keypair is gone. Fun, huh?

Based on a quick review of the [GitHub issues for Docker Machine][link-4], this is "expected" and "normal," and the expectation is for users to simply re-provision the keypair into OpenStack again via manual means. Personally, I find this behavior to run completely counter to what I would expect. However, until this behavior is changed/fixed in a future release, please plan accordingly when using Docker Machine with OpenStack.

**UPDATE:** Nathan LeClaire, one of the Docker Machine maintainers, has opened [a new GitHub issue][link-5] to track this behavior.



[link-1]: https://www.docker.com/products/docker-machine
[link-2]: http://www.openstack.org/
[link-3]: https://docs.docker.com/machine/drivers/openstack/
[link-4]: https://github.com/docker/machine/issues/
[link-5]: https://github.com/docker/machine/issues/3261
[xref-1]: {{< relref "2015-09-24-using-docker-machine-with-openstack.md" >}}
