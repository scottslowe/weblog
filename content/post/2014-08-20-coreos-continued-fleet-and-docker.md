---
author: slowe
categories: Education
comments: true
date: 2014-08-20T09:00:00Z
slug: coreos-continued-fleet-and-docker
tags:
- CLI
- Docker
- Linux
- OSS
- CoreOS
title: 'CoreOS Continued: Fleet and Docker'
url: /2014/08/20/coreos-continued-fleet-and-docker/
wordpress_id: 3501
---

This post is the third in a series of posts on [CoreOS](http://coreos.com/), this time focusing on the use of [fleet](https://github.com/coreos/fleet) and [Docker](http://docker.com/) to deploy containers across a cluster of systems. This post builds on [my earlier introduction to CoreOS][1] and [the subsequent more in-depth look at etcd][2].

I'm assuming that you're already reasonably familiar with CoreOS, etcd, and Docker. If you aren't familiar with CoreOS or etcd, have a look at the links in the previous paragraph. If you need a quick introduction to Docker, check out this [quick introduction to Docker][3]. While the example I'm going to provide here is fairly simple, it should serve as a reasonable basis upon which to build later.

## An Overview of Fleet

The GitHub page for fleet describes it as a "distributed init system" that operates across a cluster of machines instead of on a single machine. It leverages [etcd](https://github.com/coreos/etcd), the distributed key-value store that ships with CoreOS, as well as [systemd](http://freedesktop.org/wiki/Software/systemd/). Fleet combines etcd and systemd to allow users to deploy containers (configured as systemd units) across a cluster of CoreOS systems.

Using fleet, users can deploy a single container anywhere on the cluster, deploy multiple copies of the same container, ensure that containers run on the same machine (or different machines), or maintain a certain number of instances of a service (thus protecting against failure).

Note that even though fleet helps with scheduling containers across a cluster of systems, fleet doesn't address some of the other significant challenges that arise from an architecture based on distributed micro-services in containers. Namely, fleet does not address inter-container communications, service registration, service discovery, or any form of advanced utilization-based scheduling. These are topics I hope to be able to explore here in the near future.

Now that you have an idea of what fleet does, let's take a closer look at actually using fleet.

## Interacting with Fleet

By default, the `fleetctl` command-line client that is provided to interact with fleet assumes it will be interacting with a local etcd endpoint on the loopback address. So, if you want to run `fleetctl` on an instance of CoreOS in your cluster, no further configuration is needed.

However, it may be easier/more effective to use `fleetctl` from outside the cluster. There are a couple of different ways to do this: you can tell `fleetctl` to use a specific endpoint, or you can tunnel the traffic through SSH. Each approach has advantages and disadvantages; I'll leave it to the readers to determine which approach is the best approach for their specific configurations/situations.

### Using a Custom Endpoint

This method is pretty straightforward and simple. Just set an environment variable named `FLEETCTL_ENDPOINT`, like this:

    export FLEETCTL_ENDPOINT=http://10.1.1.7:4001

Obviously, you'd want to make sure that you have the correct IP address (can be any node in the etcd cluster) and port (4001 is the default, I believe). With this environment variable set, now anytime you use `fleetctl` it will direct traffic to the endpoint you specified. If that specific node in the etcd cluster becomes unavailable, then `fleetctl` will stop working, and you'll need to point it to a different node in the cluster.

### Tunneling Through SSH

The second way of using `fleetctl` remotely is to tunnel the traffic through SSH. This method may be a bit more complicated, but naturally offers a bit more security.

To make `fleetctl` tunnel its communications with etcd through SSH, set an environment variable called `FLEETCTL_TUNNEL` to the IP address of any node in the etcd cluster, like this:

    export FLEETCTL_TUNNEL=10.1.1.7

However, the configuration involves more than just setting the environment variable. The `fleetctl` doesn't expose any options to configure the SSH connection, and it assumes you'll be using public key authentication. This means you'll need access to a public key that will work against the nodes in your etcd cluster. If you followed my instructions on [deploying CoreOS on OpenStack via Heat][4], then you can review the Heat template to see which key was specified to be injected when the instances were spawned. Once you know which key was used, then you'll need to either:

* place that key on the system where `fleetctl` is installed, or

* install `fleetctl` on a system that already has that key present.

There's still at least one more step required (possibly two). Because `fleetctl` doesn't expose any SSH options, you're going to need to run an SSH agent on the system you're using. OS X provides an SSH agent by default, but on Linux systems you will probably have to manually run an SSH agent and add the appropriate SSH key:

    eval `ssh-agent -s`
    ssh-add ~/.ssh/keyfile.pem

Once the SSH agent is running and the appropriate key is loaded (you'd clearly need to make sure the path and filename are correct in the command listed above), then the last step is to configure your `~/.ssh/config` file with options for the CoreOS instances. It's possible you might be able to get by without this step; I haven't conducted enough testing to say with absolute certainty one way or another. I suspect it will be needed.

In the `~/.ssh/config` file, add a stanza for the system through which you'll be tunneling the `fleetctl` traffic. The stanza will need to look something like this:

    Host node-01
      User core
      Hostname 10.1.1.7
      IdentityFile ~/.ssh/keyfile.pem

This configuration stanza ensures that when the system you're using attempts to communicate with the IP address listed above, it will use the specified username and public key. Since the SSH agent is loaded, it won't prompt for any password for the public key (even if the public key doesn't have a password associated, you'll still need the SSH agent), and the SSH connection will be successful _without any user interaction._ That last point is important---`fleetctl` doesn't expose any SSH options, so the connection needs to be completely automatic.

Once you have all these pieces in place, then you can simply run `fleetctl` with the appropriate commands (described in the next section), and the connection to the etcd cluster will happen over SSH via the specified host. Naturally, if that node in the cluster goes away or is unavailable, you'll need to point your connection to a different node in the etcd cluster.

## Using Fleet

Once you have access to the etcd cluster via `fleetctl` using one of the three methods described above (direct access via a CoreOS instance, setting a custom endpoint, or tunneling over SSH), then you're ready to start exploring how fleet works.

First, you can list all the machines in the cluster with this command:

    fleetctl list-machines

Note the "METADATA" column; this allows you to do some custom scheduling by associating systemd units with specific metadata parameters. Metadata can be assigned either via cloud-config parameters passed when the instance is spawned, or via modifications to the fleet config files.

To see the units about which the cluster knows, use this command:

    fleetctl list-units

If you're just getting your etcd cluster up and running, the output of this command is probably empty. Let's deploy a unit that spawns a Docker container running the popular Nginx web server. Here's a (very) simple unit file that will spin up an Nginx container via Docker:

```
[Unit]
Description=Nginx web front-end
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --rm --name nginx -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx
```

(Click [here](https://gist.github.com/scottslowe/a0777d789d91464441fd) to see the code excerpt as a GitHub Gist.)

With this file in place on the system where you are running `fleetctl`, you can submit this to the etcd cluster with this command:

    fleetctl submit nginx.service

Then, when you run `fleetctl list-units`, you'll see the new unit submitted (but not started). Start it with `fleetctl start nginx.service`.

Where fleet becomes _really_ useful (in my opinion) is when you want to run multiple units across the cluster. If you take the simple Nginx unit I showed you earlier and extend it slightly, you get this:

```
[Unit]
Description=Nginx web front-end
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --rm --name nginx-01 -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx-01

[X-Fleet]
X-Conflicts=nginx.*.service
```

(Click [here](https://gist.github.com/scottslowe/dc3cadbfbfd3ae3ebe08) for the code block above as a GitHub Gist.)

Note the difference here: the Docker container name is changed (to `nginx-01`) and the filename is different (now `nginx.1.service`). If you make multiple copies of this file, changing the Docker container name and the unit filename, you can submit all of the units to the etcd cluster at the same time. For example, let's say you wanted to run 3 Nginx containers on the cluster. Make three copies of the file (`nginx.1.service`, `nginx.2.service`, and `nginx.3.service`), modifying the container name in each copy. Make sure that you have the "X-Conflicts" line in there; that tells fleet not to place two Nginx containers on the same system in the cluster. Then submit them with this command:

    fleetctl submit nginx.*.service

And start (launch) them with this command:

    fleetctl start nginx.*.service

Give it a few minutes to download the latest Nginx Docker image (assuming it isn't already downloaded), then run `fleetctl list-units` and you should see three Nginx containers distributed across three different CoreOS instances in the etcd cluster, all listed as "loaded" and "active". (You can then test connectivity to those Nginx instances using something like `curl`.) Congratulations---you've just deployed multiple containers automatically across a cluster of systems!

(Want to see some of the magic behind fleet? Run `etcdctl --peers _<IP address of cluster node>_:4001 ls /_coreos.com --recursive` and see what's displayed. You're welcome.)

Admittedly, this is a very simple example. However, the basic architecture I've shown you here can be extended. For example, by using additional fleet-specific properties like "X-ConditionMachineOf" in your unit file(s), you can run what is known as a "sidekick container." These containers do things like update an external load balancer, or register the presence of the "primary" container in some sort of service discovery mechanism. (In fact, as I alluded to in [my etcd post][2], you could use etcd as that service discovery mechanism.)

Naturally, `fleetctl` includes commands for stopping units, destroying units, etc., as well as submitting and starting units. You can use `fleetctl help` to get more information, or visit [the fleet GitHub page](https://github.com/coreos/fleet).

I hope you've found this post to be helpful. Feel free to post any questions, corrections, clarifications, or thoughts in the comments below. Courteous comments are always welcome.

[1]: {{< relref "2014-08-01-a-quick-introduction-to-coreos.md" >}}
[2]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
[3]: {{< relref "2014-03-11-a-quick-introduction-to-docker.md" >}}
[4]: {{< relref "2014-08-13-deploying-coreos-on-openstack-using-heat.md" >}}
