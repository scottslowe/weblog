---
author: slowe
categories: Tutorial
comments: true
date: 2015-03-06T09:07:00Z
tags:
- Docker
- Linux
- CLI
- Vagrant
- Fusion
- Virtualization
title: Running a Small Docker Swarm Cluster
url: /2015/03/06/running-own-docker-swarm-cluster/
---

In this post, I'm going to show you how to set up and run your own [Docker Swarm][link-5] cluster. Docker Swarm is a relatively new orchestration tool from [Docker][link-1] (the company) that allows you to create a cluster of hosts running Docker (the open source project) and schedule containers across the cluster. However, just scheduling and running containers across a cluster isn't enough, so I'll show you how to add service registration and service discovery to this environment using [Consul][link-2].

In the event you're interested in following along, I've created a set of files that will allow you to use [Vagrant][link-3] to run this Docker Swarm cluster (on your laptop, if so desired). You can find all these files in the "docker-swarm" folder of [my GitHub learning-tools repository][link-4].

The Docker Swarm cluster I'm going to show you how to build has 3 major components:

* A cluster of systems running Consul. In this case, Consul serves a dual purpose. First, it's used as the discovery service for the Docker Swarm cluster itself. Second, it provides service registration and service discovery functionality for the Docker containers launched on the Swarm cluster.
* A set of hosts running the Docker daemon (version 1.4.0 or higher, as required by Swarm). In this case, I'm using [CoreOS][link-6] Stable 557.2.0, which has Docker 1.4.1.
* A few containers running on the CoreOS hosts: a Consul client, [Registrator][link-7] (for dynamically registering and unregistering Docker containers), and Swarm.

Ready? Let's get started!

## Setting up Consul

If you're unfamiliar with Consul, I recommend having a look at [my quick introduction to Consul][xref-1]. Since that post provides an overview of getting Consul installed (which is really simple), I'll focus here on bootstrapping the Consul cluster. I'm not running the Consul cluster as containers on the CoreOS systems (which works fine) in order to sidestep the issue of dealing with the CoreOS automatic updates (and subsequent reboots) and to simplify the overall environment a bit.

I'm using two pieces to handle bootstrapping the Consul cluster. First, I have a Consul configuration file that provides the necessary configuration details to Consul. Here's the JSON-formatting Consul configuration file:

```json
{
    "bootstrap_expect": 3,
    "server": true,
    "data_dir": "/var/consul",
    "log_level": "INFO",
    "enable_syslog": false,
    "retry_join": ["192.168.1.101", "192.168.1.102", "192.168.1.103"],
    "client_addr": "0.0.0.0"
}
```

The Consul web site provides documentation for all these parameters, but let's walk through this real quick:

* The `bootstrap_expect` line tells Consul to form a cluster when at least 3 nodes are present. This allows us to start Consul nodes independently, but none of them will start working as expected until 3 are online and communicating.
* The `server` line instructs the Consul agent (running as a daemon) to operate as a server.
*  The `data_dir` directory instructs Consul to store its working files in `/var/consul`. Note you'll need to create that directory (and assign appropriate permissions).
* The `log_level` and `enable_syslog` lines configure logging (which, by default, will go to `/var/log/consul.log`).
* The `retry_join` line provides a list of addresses for other expected members of the Consul cluster. I use the `retry_join` line instead of `join` so that we can independently start the Consul nodes. You'll want/need to edit this line to use IP addresses that are applicable/correct for your environment.
* Finally, the `client_addr` line tells Consul to listen on all interfaces (not just loopback, which is the default).

You can store this anywhere you'd like, but if you want to use the scripts and such that I've created, put this configuration file in `/etc/consul.d/server` as a file named `config.json`.

Next, I have an Upstart script to start and run the Consul agent as a daemon using the above configuration file. Here's the script:

```text
description "Consul server process"

start on runlevel [2345]
stop on runlevel [!2345]

respawn

script
  if [ -f "/etc/service/consul" ]; then
    . /etc/service/consul
  fi

export GOMAXPROCS=`nproc`

exec /usr/local/bin/consul agent \
  -config-dir="/etc/consul.d/server" \
  ${CONSUL_FLAGS} \
  >>/var/log/consul.log 2>&1
end script
```

You can see that this script assumes the Consul binary is in `/usr/local/bin`; if you store it elsewhere, be sure to adjust this script accordingly. Store this file as `consul.conf` in `/etc/init`; then you can use these commands to start and stop Consul, respectively:

	sudo service consul start
	sudo service consul stop

Ideally, you'll also want to create a dedicated user for Consul, and assign ownership/permissions on the `/etc/consul.d` and `/var/consul` directories to that user.

Setting up the Consul cluster therefore looks something like this:

1. Get an Ubuntu 14.04 system (or possibly earlier versions) up and running.
2. Install the Consul binary to `/usr/local/bin`.
3. Create the Consul user.
4. Create the `/etc/consul.d/server` and `/var/consul` directories, and assign ownership/permission for those directories to the Consul user.
5. Put the configuration file listed above onto the Ubuntu system as `config.json` in the `/etc/consul.d/server` directory.
6. Put the Upstart script listed above onto the Ubuntu system as `consul.conf` in the `/etc/init` directory.
7. Start the Consul daemon using `sudo service consul start`.
8. Repeat steps 1-7 for two more systems to establish the minimum of three systems running Consul.

Once your Consul cluster is up and running (you can test this by running `consul members`; you should see a list of three systems in the cluster), you're ready to move on to setting up the Swarm cluster.

## Setting up the Swarm Cluster

I won't go into any detail here on setting up some systems to run the Docker Engine (daemon); I'll leave that as an exercise for the reader. Since Consul will serve as the back-end discovery service for the Swarm cluster I'm not immediately aware of any minimum number of Docker Engine nodes, but let's assume you'll need to start at three. If you want to use CoreOS, be sure to use a version of CoreOS that containers Docker >= 1.4.0. CoreOS Stable 557.2.0 includes Docker 1.4.1, and therefore will work with Docker Swarm.

You'll also need to ensure that Docker Engine on each node is configured to listen on a network port. I'll assume you've configured Docker Engine to listen on TCP 2375, which is the official IANA registered port for Docker (TCP 2376 if you're using TLS).

Setting up the Swarm cluster is pretty straightforward. On each Docker Engine node (CoreOS in my example), launch a Docker container with the following command line:

	docker run -d swarm join --addr=<node IP address>:2375 consul://<IP address of Consul server in cluster>:8500/swarm

Substitute the IP address of the Docker Engine node for the `--addr` parameter, and substitute the IP address of a member of the Consul cluster for the `consul://` discovery URL. Therefore, if your Docker Engine node was at 192.168.1.104 and one of the Consul servers was at 192.168.1.101, the command would look like this:

	docker run -d swarm join --addr=192.168.1.104:2375 consul://192.168.1.101:8500/swarm

Note that the "/swarm" on the end of the discovery URL is an arbitrary path; just be sure to use the same value everywhere in the Swarm cluster.

Repeat this process on every node that should be a part of the Docker Swarm cluster.

Finally, on a system running Docker Engine (it can be a separate system or a system that is part of the cluster), run a Docker container that will enable managing the Swarm cluster:

	docker run -d -p <Swarm port>:2375 swarm manage consul://<<IP address of Consul server in cluster>:8500/swarm

Replace `<Swarm port>` with a port number, and specify the IP address of one of the servers in the Consul cluster. If you used a path other than "/swarm", be sure to use the same path here. This command sets up an endpoint against which you'll run Docker commands---only in this case those Docker commands will be executed against the entire cluster.

So, let's say you want to run an Nginx container named "www" somewhere on the Swarm cluster. Assuming that you used 8333 as the Swarm port when launching the Swarm manager and that the Swarm manager was running on a system with the IP address 192.168.1.104, you'd do that with this command:

	docker -H tcp://192.168.1.104:8333 run -d --name www -p 80:80 nginx

If you wanted to see a list of the containers running across all the systems in the Swarm cluster, you'd run this command:

	docker -H tcp://192.168.1.104:8333 ps

If you wanted to see information about the cluster, the nodes in the cluster, and the containers running across the cluster, you'd run this command:

	docker -H tcp://192.168.1.104:8333 info

Of course, you could also set the `DOCKER_HOST` environment variable, and then you could skip the `-H tcp://...` portion of the commands above.

That's it! You've just set up your own Docker Swarm cluster. But wait, there's more...

## Setting up Service Registration and Discovery

Docker Swarm will take care of scheduling and running containers on Docker Engine nodes within the cluster, but how in the world does one connect to services running in those containers? How does one know which IP addresses and ports are in use? That's where service registration and service discovery come into play. Since Consul is already running in this environment, I'll leverage Consul to also provide service registration and service discovery functionality.

Adding this functionality to the existing Consul-backed Docker Swarm cluster is pretty straightforward. There are two pieces involved: running a Consul client container and running a Registrator container on each Docker Engine node in the Swarm cluster. The Registrator container monitors the Docker UNIX socket for events, and registers/de-registers containers and services upon start and stop. The Consul client receives the registration/de-registration events from Registrator and forwards them to the Consul cluster, where other systems can query to find and locate containers and services (even through DNS). Cool, eh?

To run a Consul client as a container on a Docker Engine node in the Swarm cluster, use this command (executed against the Docker Engine directly, not through Swarm):

	docker run -d -p 8300:8300 -p 8301:8301 -p 8301:8301/udp -p 8302:8302 -p 8302:8302/udp -p 8400:8400 -p 8500:8500 -p 8600:8600/udp --name <container-name> -h <hostname> progrium/consul -rejoin -advertise <external IP address> -join <Consul server IP address>

OK, that's a hefty command, so let's break it down:

* All the various `-p` parameters are exposing the necessary Consul ports for communication.
* The specific container image you're using is `progrium/consul`, which is a Dockerized Consul agent.
* Be sure to specify a unique value both for the container name (via `--name`) and a unique hostname (via the `-h` parameter).
* The `-advertise` parameter needs to advertise the external IP address of the Docker Engine node, not the private IP address assigned to the container behind the Docker bridge.
* Finally, the `-join` parameter should specify the IP address of one of the servers in the Consul cluster.

After launching this container, you can verify that it is working as expected via two methods:

1. Use the `docker logs` command to see the logs from the container and see that it is communicating with the Consul cluster.
2. Use the `consul members` command from one of the Consul servers to show that there is now a "client" node participating in Consul. (Client nodes forward requests to server nodes.)

Almost done! Next, launch a Registrator container on each Docker Engine node (directly, not using Swarm) with this command:

	docker run -d --name <container name> -h <hostname> -v /var/run/docker.sock:/tmp/docker.sock progrium/registrator consul://<Consul server IP address>:8500

As with the Consul client container, be sure to use a unique container name and hostname, and substitute the IP address of one of the Consul servers in the `consul://` URL.

You can use `docker logs` to verify Registrator's operation; you should see a series of registration events in the output.

You can also query Consul directly to see if Registrator registered the services in Consul by using this command:

	curl http://<Consul server IP address>:8500/v1/catalog/services | python -m json.tool

You should see JSON output listing the services (unique TCP and UDP ports) from the containers across the Docker Swarm cluster. To get more information about a particular service, modify the previous command to include the service name. For example, if you'd launched an Nginx container on the cluster the command would look like this:

	curl http://<Consul server IP address>:8500/v1/catalog/service/nginx-80 | python -m json.tool

That will provide JSON-formatted output that lists all the instances of this service across the cluster, the IP addresses and nodes associated with the service, and any metadata (which probably won't exist).

All set---you've just built a Consul cluster, used Consul as the discovery service to back-end a Docker Swarm cluster, use Docker Swarm to launch a container, and then provided service registration and service discovery functionality via Consul and Registrator. Good job!

## Additional Resources

You can find copies of all the scripts, configuration files, and a `Vagrantfile` to follow along using Vagrant in the "docker-swarm" folder of [my GitHub "learning-tools" repository][link-4].

[link-1]: https://www.docker.com
[link-2]: http://www.consul.io
[link-3]: http://www.vagrantup.com
[link-4]: https://github.com/scottslowe/learning-tools
[link-5]: https://docs.docker.com/swarm/
[link-6]: https://coreos.com
[link-7]: https://github.com/gliderlabs/registrator
[xref-1]: {{< relref "2015-02-06-quick-intro-to-consul.md" >}}
