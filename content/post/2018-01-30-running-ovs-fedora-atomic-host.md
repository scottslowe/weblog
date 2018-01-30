---
author: slowe
categories: Tutorial
comments: true
date: 2018-01-30T21:00:00Z
tags:
- OVS
- Fedora
- Linux
- Networking
- Docker
title: Running OVS on Fedora Atomic Host
url: /2018/01/30/running-ovs-fedora-atomic-host/
---

In this post, I'd like to share the results of some testing I've been doing to run [Open vSwitch][link-1] (OVS) in containers on a container-optimized Linux distribution such as [Atomic Host][link-2] (Fedora Atomic Host, specifically). I'm still relatively early in my exploration of this topic, but I felt like sharing what I've found so far might be helpful to others, and might help spark conversations within the relevant communities about how this experience might be improved.<!--more-->

The reason for the use of Docker containers in this approach is twofold:

1. Many of the newer container-optimized Linux distributions---CoreOS Container Linux (soon to be part of Red Hat in some fashion), Project Atomic, etc.---eschew "traditional" package management solutions in favor of containers.
2. Part of the reason behind my testing was to help the OVS community better understand what it would look like to run OVS in containers so as to help make OVS a better citizen on container-optimized Linux distributions.

In this post, I'll be using Fedora 27 Atomic Host (via [Vagrant][link-4] with [VirtualBox][link-5]). If you use a different version or release of Atomic Host, your results may differ somewhat. For the OVS containers, I'm using the excellent [keldaio/ovs][link-3] Docker containers.

As it turns out, running OVS on Fedora Atomic Host using the Kelda Docker images is really straightforward. As stated in [Kelda's README for the OVS Docker images][link-6], you just have to launch a couple of containers. First, you'd launch a container for the OVSDB server:

    docker run -itd --net=host --name=ovsdb-server keldaio/ovs ovsdb-server

Next, you'd run a container for the ovs-vswitchd daemon:

    docker run -itd --net=host --name=ovs-vswitchd --volumes-from=ovsdb-server --privileged keldaio/ovs ovs-vswitchd

That gets the core, essential parts of OVS up and running, but you're not fully functional yet. The final ingredient is to load the OVS kernel module, which is part of the upstream Linux kernel. Normally, starting OVS-related daemons would initiate loading the kernel module, but with OVS encapsulated in Docker containers it won't happen automatically. However, a quick `sudo modprobe openvswitch` will easily remedy that and get the OVS kernel module loaded.

From this point, running `ovs-vsctl` to configure OVS involves appending your command to a `docker exec` command, like this:

    docker exec -it ovs-vswitchd ovs-vsctl show

This will run `ovs-vsctl show` in the "ovs-vswitchd" container, which is where the ovs-vswitchd daemon is running. All the standard `ovs-vsctl` commands should work here, such as adding a bridge (`add-br`), adding a port (`add-port`), deleting a port (`del-port`), and deleting a bridge (`del-br`).

This is all pretty cool, but there's a problem. OVS' configuration is stored in the OVSDB database, which is itself found in the "ovsdb-server" container (hence why the vswitchd container needs to mount the same volumes as the "ovsdb-server" container via `--volumes-from`). What if the "ovsdb-server" container goes away? **The OVS configuration goes away, too.** (Don't believe me? Stop, remove, and relaunch the "ovsdb-server" container using the command line above and see for yourself.)

Obviously, that's not ideal. One possible solution is to use Docker volumes to store the data that OVSDB needs _separate_ from the actual OVSDB server container.

Here's how you'd make this work. First, you'd create some Docker volumes:

    docker volume create var-lib-ovs
    docker volume create var-log-ovs
    docker volume create var-run-ovs
    docker volume create etc-ovs

Then, you'd amend the command line for launching the "ovsdb-server" container to look like this instead:

    docker run -itd --net=host --name=ovsdb-server -v var-lib-ovs:/var/lib/openvswitch -v var-log-ovs:/var/log/openvswitch -v var-run-ovs:/var/run/openvswitch -v etc-ovs:/etc/openvswitch keldaio/ovs ovsdb-server

The command line for launching the ovs-vswitchd container remains unchanged, since it just mounts the same volumes as the OVSDB server container.

With this approach, the OVS configuration is now stored separate from the containers where the OVSDB server and ovs-vswitchd processes run, which means we can easily kill and restart those containers _without_ negatively impacting OVS configuration. This configuration also brings us one step closer to using systemd to manage the OVS containers since we can now persist data across container instances.

I'm still exploring this sort of configuration, so I'll share additional information I uncover in future blog posts. In the meantime, if I've made an error in this post or if there is a suggestion you'd like to make to improve the post, feel free to [contact me on Twitter][link-7].



[link-1]: http://openvswitch.org/
[link-2]: https://www.projectatomic.io/
[link-3]: https://hub.docker.com/r/keldaio/ovs/
[link-4]: https://www.vagrantup.com/
[link-5]: https://virtualbox.org/
[link-6]: https://github.com/kelda/kelda/tree/master/ovs
[link-7]: https://twitter.com/scott_lowe
