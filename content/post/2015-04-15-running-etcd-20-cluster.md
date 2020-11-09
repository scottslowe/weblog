---
author: slowe
categories: Tutorial
comments: true
date: 2015-04-15T14:35:00Z
tags:
- Ubuntu
- Linux
- CLI
- Vagrant
- Fusion
- etcd
title: Running an etcd 2.0 Cluster on Ubuntu 14.04
url: /2015/04/15/running-etcd-20-cluster/
---

In this post, I'm going to show you how to set up a cluster of three nodes running [etcd][link-1] 2.0 (specifically, etcd 2.0.9). While [I've discussed etcd before][xref-1], that was in the context of using etcd with [CoreOS Linux][link-2]. In this case, I'll use Ubuntu 14.04 as the base OS, along with the latest released version of etcd.

To help you follow along, I've created a set of files that will allow you to use [Vagrant][link-4] to turn up an etcd 2.0 cluster on Ubuntu 14.04 (on your laptop, if so desired). You can find all these files in the "etcd-2.0" directory of [my learning-tools GitHub repository][link-3].

## Installing the Base OS

You don't need anything special when setting up etcd; a straightforward Ubuntu Server 14.04 x64 installation will work just fine. If you're using the files in [my learning-tools repository][link-3], you'll see that Vagrant simply turns up a VM based on a plain-jane Ubuntu 14.04 box. If you're building this from scratch (why?!), simply create a VM and install Ubuntu 14.04 into it. As long as it has Internet connectivity, that's all that's needed.

## Installing etcd

Installing etcd is very straightforward:

1. Download the etcd 2.0.9 release from [https://github.com/coreos/etcd/releases/tag/v2.0.9][link-5].

2. Unpack the downloaded `.tar.gz` file using `tar xvzf <filename>`.

3. Create the `/var/etcd` directory for etcd to use as its data directory.

4. Optionally, change into the newly-created directory (named `etcd-2.0.9-linux-amd64` by default) and use `mv` to move the `etcd` and `etcdctl` binaries to a more permanent location on your system. (These are statically linked binaries, so feel free to put them wherever it makes sense to you. I chose to put them in `/usr/local/bin`.)

That's it. Just make a note of where the `etcd` binary is stored, as you'll need to know that location later.

## Configuring etcd and Turning Up the Cluster

Earlier (pre-2.0) releases of etcd apparently supported a TOML-based configuration file, but that support was removed in the 2.0 release. Instead, you'll need to use either a) environment variables or b) command-line parameters. I chose to use environment variables, and---since I wanted to make this as "production quality" as possible, I chose to build an Upstart script for etcd. (If you don't want to use Upstart, I'll talk about that later in this section.)

Here's an Upstart script you can use for etcd:

```text
description "etcd 2.0 distributed key-value store"
author "Scott Lowe <scott.lowe@scottlowe.org>"

start on (net-device-up
          and local-filesystems
          and runlevel [2345])
stop on runlevel [016]

respawn
respawn limit 10 5

script
  if [ -f "/etc/default/etcd" ]; then
    . /etc/default/etcd
  fi

chdir /var/etcd
exec /usr/local/bin/etcd >>/var/log/etcd.log 2>&1
end script
```

This is a reasonably straightforward Upstart script, so I won't bother spending a great deal of time explaining it. Note that if you chose not to move the `etcd` binary to `/usr/local/bin`, you'll have to edit this Upstart script to point to the correct location of the binary. Also, if you choose to use something other than `/var/etcd` as the data directory, you'll need to edit this Upstart script appropriately. Consider the script above to be an example from which you can build one appropriate to your environment.

I did find that the `if [ -f "/etc/default/etcd" ]; then` test---and subsequently sourcing the contents of that file---doesn't work with Upstart (they should be removed from this Upstart script, but I haven't bothered yet). The original idea was to define the necessary environment variables in `/etc/default/etcd`, but no amount of effort could make it work. I _could_ embed the environment variables in the Upstart script itself, but that seemed like a hack (and not a good one). Finally, a couple folks pointed me to [override files][link-6], which worked perfectly.

The Upstart script itself (listed above) goes into `/etc/init` as `etcd.conf`. The override file also goes into `/etc/init` but is named (quite intuitively) `etcd.override`. Here's the contents of the override file:

```text
# Override file for etcd Upstart script providing some environment variables
env ETCD_INITIAL_CLUSTER="etcd-01=http://192.168.101.101:2380,etcd-02=http://192.168.101.102:2380,etcd-03=http://192.168.101.103:2380"
env ETCD_INITIAL_CLUSTER_STATE="new"
env ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster-01"
env ETCD_INITIAL_ADVERTISE_PEER_URLS="http://192.168.101.101:2380"
env ETCD_DATA_DIR="/var/etcd"
env ETCD_LISTEN_PEER_URLS="http://192.168.101.101:2380"
env ETCD_LISTEN_CLIENT_URLS="http://192.168.101.101:2379"
env ETCD_ADVERTISE_CLIENT_URLS="http://192.168.101.101:2379"
env ETCD_NAME="etcd-01"
```

By including all these environment variables in the override file, we're able to keep the Upstart script itself very clean and minimal. If you need to make changes to the configuration, then you edit the override file and just restart the service. Without the override file, all these parameters would have to be specified on the `etcd` command line itself, which has the following effects:

* You can't use the same Upstart script on multiple systems, since the script contains configuration data specific to a particular node (like the hostname and IP addresses).
* The Upstart script becomes unnecessarily complex and harder to debug and/or troubleshoot.

Moving the environment variables into the override file solves both these problems. Machine-specific override files can be supplied for each system and used with a single, consistent Upstart script.

If you're not really familiar with etcd, the environment variables defined in the override file deserve a bit of explanation. In this configuration, etcd is using _static discovery_ to build the cluster. In other words, you (the user) are explicitly providing the hostnames and/or IP addresses of all of the members of the cluster (specified in the `ETCD_INITIAL_CLUSTER` variable). As described [here][link-7], the `ETCD_INITIAL_*` variables are _only_ used on the first run of etcd; they will be ignored on subsequent runs. This allows you to easily bootstrap an etcd cluster, if you know the IP addresses that will be used in advance. (By the way, [this video on bootstrapping etcd][link-8] is great---highly recommended.) Naturally, the IP addresses specified in the override file for all the etcd nodes need to be correct.

If you choose not to use Upstart and just launch `etcd` manually, then you'll either need to a) manually define the appropriate environment variables, or b) specify those configuration parameters on the command line you use to launch `etcd`.

Assuming that you've chosen to use Upstart and have correctly configured the override file on each system that will be part of the cluster, you can then start etcd with a simple `sudo initctl start etcd`. Some error messages will be logged in the log file (`/var/log/etcd.log`) until all three nodes come online, but once all three nodes (as specified in the `ETCD_INITIAL_CLUSTER` variable) are up and running etcd will be running as expected.

You can verify the operation of etcd by running a command like `etcdctl member list`, which will return a list of the members of the etcd cluster. Congratulations, you've just turned up an etcd 2.0 cluster!

## Additional Resources

You can find the Upstart script, machine-specific override files, a provisioning script, and a Vagrantfile for replicating a three-node etcd 2.0 cluster on Ubuntu 14.04 in the "etcd-2.0" directory of [my learning-tools GitHub repository][link-3]. It was tested with Vagrant 1.7.2, the VMware plugin for Vagrant, and Fusion 6.0.5. It should work on VMware Workstation, but I haven't tested it there. (Feedback is welcome.)

[link-1]: https://github.com/coreos/etcd
[link-2]: https://coreos.com
[link-3]: https://github.com/scottslowe/learning-tools
[link-4]: http://www.vagrantup.com/
[link-5]: https://github.com/coreos/etcd/releases/tag/v2.0.9
[link-6]: http://upstart.ubuntu.com/cookbook/#override-files
[link-7]: https://github.com/coreos/etcd/blob/master/Documentation/clustering.md
[link-8]: https://www.youtube.com/watch?v=duUTk8xxGbU
[xref-1]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
