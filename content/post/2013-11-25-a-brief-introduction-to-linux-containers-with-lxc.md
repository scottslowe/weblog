---
author: slowe
categories: Education
comments: true
date: 2013-11-25T09:00:00Z
slug: a-brief-introduction-to-linux-containers-with-lxc
tags:
- CLI
- Linux
- LXC
- Ubuntu
- Virtualization
title: A Brief Introduction to Linux Containers with LXC
url: /2013/11/25/a-brief-introduction-to-linux-containers-with-lxc/
wordpress_id: 3336
---

In this post, I'm going to provide a brief introduction to working with Linux containers via [LXC](http://linuxcontainers.org/). Linux containers are getting a fair amount of attention these days (perhaps due to [Docker](http://www.docker.io/), which leverages LXC on the back-end) as a lightweight alternative to full machine virtualization such as that provided by "traditional" hypervisors like KVM, Xen, or ESXi.

Both full machine virtualization and containers have their advantages and disadvantages. Full machine virtualization offers greater isolation at the cost of greater overhead, as each virtual machine runs its own full kernel and operating system instance. Containers, on the other hand, generally offer less isolation but lower overhead through sharing certain portions of the host kernel and operating system instance. In my opinion full machine virtualization and containers are complementary; each offers certain advantages that might be useful in specific situations.

Now that you have a rough idea of what containers are, let's take a closer look at using containers with LXC. I'm using Ubuntu 12.04.3 LTS for my testing; if you're using something different, keep in mind that certain commands may differ from what I show you here.

Installing LXC is pretty straightforward, at least on Ubuntu. To install LXC, simply use `apt-get`:

    apt-get install lxc

Once you have LXC installed, your next step is creating a container. To create a container, you'll use the `lxc-create` command and supply the name of the container template as well as the name you want to assign to the new container:

    lxc-create -t <template> -n <container name>

You'll need Internet access to run this command, as it will download (via your configured repositories) the necessary files to build a container according to the template you specified on the command line. For example, to use the "ubuntu" template and create a new container called "cn-01", the command would look like this:

    lxc-create -t ubuntu -n cn-01

Note that the "ubuntu" template specified in this command has some additional options supported; for example, you can opt to create a container with a different release of Ubuntu (it defaults to the latest LTS) or a different architecture (it defaults to the host's architecture).

Once you have at least one container created, you can list the containers that exist on your host system:

    lxc-list

This will show you all the containers that have been created, grouped according to whether the container is stopped, frozen (paused), or running.

To start a container, use the `lxc-start` command:

    lxc-start -n <container name>

Using the `lxc-start` command as shown above is fine for initial testing of your container, to ensure that it boots up as you expect. However, you won't want to run your containers long-term like this, as the container "takes over" your console with this command. Instead, you want the container to run in the background, detached from the console. To do that, you'll add the "-d" parameter to the command:

    lxc-start -d -n <container name>

This launches your container in the background. To attach to the console of the container, you can use the `lxc-console` command:

    lxc-console -n <container name>

To escape out of the container's console back to the host's console, use the "Ctrl-a q" key sequence (press Ctrl-a, release, then press q).

You can freeze (pause) a container using the `lxc-freeze` command:

    lxc-freeze -n <container name>

Once frozen, you can unfreeze (resume) a container just as easily with the `lxc-unfreeze` command:

    lxc-unfreeze -n <container name>

You can also make a clone (a copy) of a container:

    lxc-clone -o <existing container> -n <new container name>

On Ubuntu, LXC is configured by default to start containers in `/var/lib/lxc`. Each container will have a directory there. In a container's directory, that container's configuration will be stored in a file named `config`. I'm not going to provide a comprehensive breakdown of the settings available in the container's configuration (this is a _brief_ introduction), but I will call out a few that are worth noting in my opinion:

* The `lxc.network.type` option controls what kind of networking the container will use. The default is "veth"; this uses virtual Ethernet pairs. (If you aren't familiar with veth pairs, see [my post on Linux network namespaces][1].)

* The `lxc.network.veth.pair` configuration option controls the name of the veth interface created in the host. By default, a container sees one side of the veth pair as eth0 (as would be expected), and the host sees the other side as either a random name (default) or whatever you specify here. Personally, I find it useful to rename the host interface so that it's easier to tell which veth interface goes to which container, but YMMV.

* `lxc.network.link` specifies a bridge to which the host side of the veth pair should be attached. If you leave this blank, the host veth interface is unattached.

* The configuration option `lxc.rootfs` specifies where the container's root file system is stored. By default it is `/var/lib/lxc/<container name>/rootfs`.

There are a great deal of other configuration options, naturally; check out `man 5 lxc.conf` for more information. You may also find [this Ubuntu page on LXC](https://help.ubuntu.com/lts/serverguide/lxc.html) to be helpful; I certainly did. 

I'll have more posts on Linux containers in the future, but this should suffice to at least help you get started. If you have any questions, any suggestions for additional resources other readers should consider, or any feedback on the post, please add your comment below. I'd love to hear from you (courteous comments are always welcome).

[1]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
