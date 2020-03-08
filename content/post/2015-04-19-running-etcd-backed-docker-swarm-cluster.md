---
author: slowe
categories: Tutorial
comments: true
date: 2015-04-19T07:05:00Z
tags:
- Docker
- Linux
- Vagrant
- CLI
- Fusion
- Virtualization
- etcd
title: Running an etcd-Backed Docker Swarm Cluster
url: /2015/04/19/running-etcd-backed-docker-swarm-cluster/
---

In this post, I'll build on a couple of previous posts and show you how to build your own [Docker Swarm][link-1] cluster that leverages [etcd][link-2] for cluster node discovery. This post builds on the information presented on [how to run an etcd 2.0 cluster on Ubuntu][xref-1] as well as the information found in [this post on running a Consul-backed Docker Swarm cluster][xref-2].

To help you follow along, I've created a [Vagrant][link-4] environment that you can use to turn up the configuration described in this blog post. These files are found in the "docker-swarm-etcd" directory of [my GitHub learning-tools repository][link-3]. Feel free to use the files in this directory/repository to help with the learning process.

There are 3 major components to this configuration:

1. A cluster of three [Ubuntu][link-5] 14.04 systems running etcd 2.0 (specifically, etcd 2.0.9, the last release as of this writing). This etcd cluster serves as a discovery back-end for Docker Swarm.
2. A set of hosts running the Docker daemon (version 1.4.0 or higher, as required by Swarm). In this particular instance, I'm using [CoreOS Linux][link-6] (version 557.2.0 from the stable branch).
3. A few containers running on the CoreOS hosts; specifically, the containers necessary to enable Swarm to function.

Let's take a look at each of these areas.

## Setting up the etcd Cluster

I won't spend a great deal of time here, as the specifics are all provided in my earlier blog post on [running etcd on Ubuntu][xref-1]. (Go read that post if you need more details.) The only semi-interesting thing is the use of static discovery for the etcd cluster, in which you use a set of environment variables to tell etcd how to discovery other cluster nodes. This is just one way of providing discovery for etcd; you could also use the hosted etcd discovery service provided by CoreOS, or DNS SRV records.

You'll want to verify that etcd is functioning correctly before proceeding with the building the other parts of this configuration. Test etcd using `etcdctl member list` and verifying that the list of member nodes returned is accurate on all the nodes.

Once etcd is up and running, then you can proceed with the next section: setting up your Docker Engine hosts.

## Setting up Docker Engine Hosts

This section is pretty straightforward. Aside from one specific configuration---which I'll cover in just a moment---this is really just a matter of setting up a few systems running the Docker Engine. As a result, you're free to use whatever Linux distribution you prefer as well as whatever Docker Engine provisioning method you like. Although I chose to use CoreOS Stable 557.2.0, you could just as easily use Ubuntu and install Docker via the provisioning method of your choice. (If you're only going to be using this environment via Vagrant, the Vagrant Docker provisioner might be a good choice.) In theory you could use [Docker Machine][link-7] to provision these instances of the Docker Engine, although I haven't yet tested that approach.

The one specific configuration that you will need to ensure is in place is to configure the Docker Engine to listen on a network port (the IANA-assigned network port is 2375, or 2376 if using TLS). This can be done a variety of ways:

* If deploying CoreOS via a cloud provider (such as OpenStack or Amazon), use [CoreOS-specific extensions to cloud-init][link-9] to configure a systemd unit that will cause the Docker Engine to listen on a network port.
* If deploying CoreOS via Vagrant, you can do the same thing using a `user-data` file. (See [this blog post][xref-3] for more details.)
* If deploying another Linux distribution via a cloud provider, you can also use [cloud-init][link-8] (but not as easily as with CoreOS Linux, in my opinion).
* If deploying another systemd-based Linux distribution via Vagrant, you can use the Vagrant file provisioner to install a systemd unit file that will cause the Docker Engine to listen on a network port.

Regardless of the exact means you take to perform this task, in the end you'll need to ensure that the Docker Engine is listening across the network.

Once you have your Docker Engine hosts running and configured correctly, then you're ready to turn up the Swarm cluster.

## Turning up the Swarm Cluster

If you've read the post on running a Consul-backed Swarm cluster, then you're probably already familiar with how this works. You'll have a small (~10 MB) Swarm container that will run on each Docker Engine host, along with an additional Swarm container that will serve as the Docker API endpoint for the entire Swarm cluster.

The first step, then, is to start the Swarm container on each of the individual Docker Engine hosts. This is accomplished with the following command:

	docker run -d swarm join --addr=<node IP address>:2375 etcd://<etcd cluster member IP address>:2379/<path in etcd>

If the Docker Engine host's IP address was 192.168.101.111, the IP address of one of the etcd cluster members was 192.168.101.101, and the path within etcd was "/swarm", then the command would look like this:

	docker run -d swarm join --addr=192.168.101.111:2375 etcd://192.168.101.101:2379/swarm

This will download (if needed) the small Swarm image and run it on the local Docker Engine host. If the Swarm container is, for whatever reason, unable to register itself in the etcd cluster, then it will exit (you can run `docker logs <container ID>` to get more details for troubleshooting).

If the Swarm container successfully registers itself in etcd, then you can run `etcdctl ls /swarm` on one of the etcd servers and you'll see an entry that corresponds to the node on which you just launched the Swarm container. (This is how the Swarm manager will use etcd to discover the Swarm nodes.)

Repeat this process on each Docker Engine host that you want to be part of the Swarm cluster.

Once you've joined all the Docker Engine hosts to the Swarm cluster (you can verify using `etcdctl ls <path in swarm>` to show that all the hosts are listed), you're ready to launch the Swarm manager container. You do that with a command like this:

	docker run -d -p <Swarm port>:2375 swarm manage etcd://<etcd cluster member IP address>:2379/<path in etcd>

Using the values as before (Docker Engine at 192.168.101.111, etcd at 192.168.101.101, and etcd path at "/swarm"), the command would look something like this:

	docker run -d -p 8333:2375 swarm manage etcd://192.168.101.101:2379/swarm

Note that the Docker Swarm manager container creates a Docker Engine API endpoint listening across the network. This creates a situation where Docker Engine (which is listening on a network port) and the Swarm manager container (which also needs to listen on a network port) could come into conflict. There are, as I see it, three possible workarounds:

1. Use a different IP interface on the host (i.e., configure Docker Engine to listen on TCP port 2375 on one IP address, and configure the Swarm manager container to listen on TCP port 2375 on a different IP address).
2. Use a different TCP port (as I've shown in the command above).
3. Run the Swarm manager container on a Docker Engine host where Docker Engine is _not_ configured to listen over the network (and is therefore _not_ part of the Swarm cluster).

The easiest method---although this may not be the best method---is to use an alternate port for the Swarm manager container, as I've done here. Just be aware that each approach has advantages and disadvantages.

Once you're done, you can run `docker info` against the Swarm manager endpoint, like this:

	docker -H tcp://<Swarm manager IP address>:<Swarm port> info

If the Swarm manager container was running on a host with the IP address 192.168.101.111 and listening on port 8333, then the command would be:

	docker -H tcp://192.168.101.111:8333 info

(You could set the `DOCKER_HOST` environment variable, if desired, so that you didn't have to explicity use the "-H" parameter.)

Other Docker commands, like `docker run`, `docker ps`, etc., also work in the same manner.

Congratulations---you've just set up an etcd-backed Docker Swarm cluster! If you're feeling really adventurous, you can review the "Setting up Service Registration and Discovery" portion of [this blog post][xref-2] and adapt it for your new etcd-backed Docker Swarm cluster.



[link-1]: https://docs.docker.com/swarm/
[link-2]: https://github.com/coreos/etcd
[link-3]: https://github.com/scottslowe/learning-tools/
[link-4]: http://www.vagrantup.com/
[link-5]: http://www.ubuntu.com/
[link-6]: https://coreos.com/
[link-7]: http://docs.docker.com/machine/
[link-8]: http://cloudinit.readthedocs.org/
[link-9]: https://coreos.com/docs/launching-containers/building/customizing-docker/
[xref-1]: {{< relref "2015-04-15-running-etcd-20-cluster.md" >}}
[xref-2]: {{< relref "2015-03-06-running-own-docker-swarm-cluster.md" >}}
[xref-3]: {{< relref "2015-02-05-vagrant-coreos-etcd-fleet-docker.md" >}}
