---
author: slowe
categories: Education
comments: true
date: 2015-05-06T15:30:00Z
tags:
- Linux
- Ubuntu
- LXC
- CLI
title: A Quick Introduction to LXD
url: /2015/05/06/quick-intro-lxd/
---

With the recent release of Ubuntu 15.04, aka "Vivid Vervet", the Ubuntu community has also unveiled an early release of [LXD][link-4] (pronounced "lex-dee"), a new project aimed at revitalizing the use of [LXC][link-3] and LXC-based containers in the face of application container efforts such as [Docker][link-1] and [rkt][link-2]. In this post, I'll provide a quick introduction to LXD.

To make it easier to follow along with some of the examples of using LXD, I've created an `lxd` directory in [my GitHub "learning-tools" repository][link-7]. In that directory, you'll find a `Vagrantfile` that will allow you to quickly and easily spin up one or more VMs with LXD.

## Relationship between LXD and LXC

LXD works in conjunction with LXC and is not designed to replace or supplant LXC. Instead, it's intended to make LXC-based containers easier to use through the addition of a back-end daemon supporting a REST API and a straightforward CLI client that works with both the local daemon and remote daemons via the REST API. You can get more information about LXD via [the LXD web site][link-4]. Also, if you're unfamiliar with LXC, check out [this brief introduction to LXC][link-5]. Once you've read that, you can browse [some other LXC-related posts][link-6] that I've published.

## Installing LXD

You can install LXD on Ubuntu 14.04 or later with a very simple set of commands:

    sudo add-apt-repository ppa:ubuntu-lxc/lxd-stable
    sudo apt-get update
    sudo apt-get install lxd

That's it. You'll probably need to log out and log back in again so that the group memberships are updated (you'll need to be member of the "lxd" group to be able to interact with the lxd daemon on the back end).

## Working with LXD

Once LXD is installed on the system, then you're ready to start working with containers. In order to launch a container, you'll first need an image upon which the container can be based. So, one of the first things you'll do is list the images on your system with the command `lxc image list`. The first time you run that command it will generate a client certificate, ostensibly to secure the communications between the `lxc` CLI client and the back-end `lxd` daemon. Naturally, the first time you run this there won't be any images listed.

To get a container image onto your system, first add a _remote_, which is a pointer to a remote repository of images. The generic command looks like this:

    lxc remote add <local name> <remote URL/FQDN>

For example, to add the public LXD server, you'd use this command:

    lxc remote add lxc-org images.linuxcontainers.org

Here, `lxc-org` is the local alias (name) for the repository, and `images.linuxcontainers.org` is the URL/FQDN for the remote repository. Once you've added a remote repository in this sort of way, you can refer to this remote repository using the alias (name) you specified.

Once you have an image repository available, you'd then copy an image down to your system with this command:

    lxc image copy <remote name>:/path/to/image local: --alias=<image name>

So, to copy down a 64-bit Debian Jessie image, you'd run this command:

    lxc image copy lxc-org:/debian/jessie/amd64 local: --alias=jessie-amd64

The `local:` part here is necessary---this is what tells the `lxc` client to copy the image to your local system (in other words, both the source and destination are needed in this command).

Once you have an image on your system, you can run `lxc image list` again, and this time you'll see one or more images listed (depending on how many images you copied down).

With an image in place, you're ready to launch a container with a command like this:

    lxc launch <image name> <container name>

If you had an image named "jessie-amd64", you could run a command like this:

    lxc launch jessie-amd64 jessie-01

Within a few seconds, you'll have a running container based on the 64-bit Debian Jessie image. Or, let's say you wanted to run a 32-bit version of Ubuntu on top of a 64-bit Ubuntu system. If your 32-bit image was called "trusty-i386", then the command to launch it might look like this:

    lxc launch trusty-i386 trusty32

Again, within a few seconds, you'll have a container running 32-bit Ubuntu "Trusty Tahr" 14.04 on your LXD-equipped host system.

To get into the system, you'd run a command like this:

    lxc exec <container name> <command>

For example, to get a shell in the 32-bit Ubuntu container you launched earlier, you'd run this command:

    lxc exec trusty32 /bin/bash

This will drop you at a shell prompt inside the specified container. From there, if you ran `file /bin/ls` you'd see that `/bin/ls` is reported as a 32-bit ELF executable. Press Ctrl-D to get out of the shell (then run `file /bin/ls` and compare the output to the command you ran inside the container).

Done with the container? Simply run `lxc stop <container>` to stop it. Don't worry---the container remains intact, and you can start it again easily with `lxc start <container>`. Don't need the container anymore? Get rid of it with `lxc delete <container>`.

By default, these containers are _persistent_---changes made within the container persist over time (until the container is deleted with `lxc delete`). However, you can make LXD behave more like Docker by using the `--ephemeral` flag to the `lxc launch` command, like this:

    lxc launch trusty-i386 trusty32 --ephemeral

When you use this flag, the container goes away as soon as you run `lxc stop <container>`.

## Initial Thoughts

Although both call themselves "containers," LXD is very different from Docker or rkt. LXD is more like an "OS container" while Docker and rkt are more like "application containers." Thus, if you're coming from a VM-centric background, LXD will probably make more sense to you: it will run multiple processes and "look/feel" like a lightweight VM. Is one better than the other? Not necessarily; they're just _different._ For some use cases and users, LXD may make more sense (for example, [read][link-9] what Michael DeHaan---creator of Ansible and Cobbler---had to say about using LXC.)

That being said, while LXC is stable LXD is still undergoing rapid development. Some features haven't been implemented yet, and the documentation is still a bit on the light side. The contributors behind LXD are going to need to really make this super-easy to use and adopt in order for them to get the same kind of mindshare that Docker (and, to a lesser extent, rkt) is getting right now.

## Additional Resources

I recommend reviewing [this article][link-8], which covers a lot of the same information presented here as well as some additional tasks (like copying files into or out of a container with `lxc file`). In addition, I encourage you to use the Vagrant environment in the `lxd` directory of [my GitHub "learning-tools" repository][link-7] to easily spin up your own LXD-equipped VMs; this will make it easy to get some hands-on time with the technology.



[link-1]: https://www.docker.com
[link-2]: https://github.com/coreos/rkt/
[link-3]: https://linuxcontainers.org/lxc/introduction
[link-4]: https://linuxcontainers.org/lxd/introduction
[link-5]: {{< relref "2013-11-25-a-brief-introduction-to-linux-containers-with-lxc.md" >}}
[link-6]: /tags/lxc/
[link-7]: https://github.com/scottslowe/learning-tools
[link-8]: https://insights.ubuntu.com/2015/04/28/getting-started-with-lxd-the-container-lightervisor/
[link-9]: http://michaeldehaan.net/post/111599240017/skipping-docker-for-lxc-for-local-development
