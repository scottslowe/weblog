---
author: slowe
categories: Tutorial
comments: true
date: 2016-03-25T00:00:00Z
tags:
- Docker
- Linux
- CLI
- AWS
title: Docker Swarm on AWS using Docker Machine
url: /2016/03/25/docker-swarm-aws-docker-machine/
---

In this post I'm going to talk about how to use [Docker Machine][link-2] to build a [Docker Swarm][link-3] cluster on [Amazon Web Services (AWS)][link-4]. This post is an adaptation of [this Docker documentation post][link-1] that shows how to build a Swarm cluster using [VirtualBox][link-5].

This post builds on the earlier post I wrote on [using Docker Machine with AWS][xref-1], so feel free to refer back to that post for more information or more details anywhere along the way.

At a high level, the process looks like this:

1. Obtain a Swarm cluster token.
2. Provision the Swarm master.
3. Provision the Swarm nodes.

Let's take a look at these steps in a bit more detail.

## Obtain a Swarm Cluster Token

There's at least a couple ways to do this, but they pretty much all involve a Linux VM using the Swarm Docker image. It's up to you exactly how you want to do this---you can use a local VM, or you can use an AWS instance. The Docker documentation tutorial uses a local VM with the VirtualBox driver:

    docker-machine create -d virtualbox local
    env $(docker-machine env local)
    docker run swarm create

The first command above creates a VirtualBox VM (named "local") and provisions Docker Engine on that VM. The second command configures the local Docker client to use this VM as its Docker host, and the final command runs `swarm create`, which pulls down the Docker Swarm image, contacts Docker Hub, and obtains a Swarm discovery token (which is output to the screen).

You could do the same thing with Docker Machine's [VMware Fusion][link-6] driver, which would change the first command to this:

    docker-machine create -d vmwarefusion local

Similarly, you could use an existing VM and use Docker Machine's generic driver. I described this process in an earlier post about [using Docker Machine and Vagrant together][xref-2].

Finally, you could use an AWS instance, which would change the first command to something like this:

    docker-machine create -d amazonec2 token

This command uses the default values of Docker Machine's Amazon EC2 driver (uses the us-east-1 region, Ubuntu 15.10, "ubuntu" user, and a generated SSH key). Because we're using a different name in this command, then the second command would need to reference this new name:

    eval $(docker-machine env token)

Once you have the output from the final `docker run swarm create` command, note that discovery token somewhere safe, as you'll need it in just a couple moments.

## Provision the Swarm Master

With the Swarm discovery token from Docker Hub safely in hand, you're now ready to provision the Swarm master. The idea here is to provision the entire Swarm cluster on EC2, so the command would look something like this:

    docker-machine create -d amazonec2 \
    --swarm --swarm-master \
    --swarm-discovery token://<Swarm discovery token> \
    master-node

This command uses the default values for Docker Machine's Amazon EC2 driver; refer back to [my earlier post on Docker Machine with AWS][xref-1] for some examples of how to customize this behavior (to provision into a specific region, for example).

When the Swarm master is done provisioning, you're ready to proceed with provisioning the Swarm nodes.

## Provision the Swarm Nodes

The command to provision the Swarm nodes will look pretty much like the command to provision the Swarm master, but without the `--swarm-master` flag:

    docker-machine create -d amazonec2 \
    --swarm --swarm-discovery token://<Swarm discovery token> \
    node-01

Repeat this command as needed (specifying a unique name for each node you add). This example command uses Docker Machine's default values for the Amazon EC2 driver; you might need to add extra flags to modify the behavior (to use a different AMI, a different SSH user, a custom SSH key, or a different AWS region).

After you're done provisioning the nodes, then there's just one thing left: connecting to the cluster using your local Docker client.

## Connecting to the Cluster

The `docker-machine` CLI has a small extra parameter needed when you connect to a Swarm cluster: the `--swarm` parameter. Here's an example:

    eval $(docker-machine env --swarm master-node)

This will populate the appropriate environment variables so that you can run commands like `docker info` and it will connect to the Swarm cluster running on AWS.

Note also that Docker Machine's Amazon EC2 driver will automatically create and populate a security group that allows in the appropriate Docker-related traffic.

## Decommissioning the Cluster

Unless you're running this cluster for production workloads, you're going to want to shut down the cluster (and the corresponding AWS instances) when you're done (to avoid a hefty AWS bill). Just run `docker-machine rm <name>` for each of the AWS instances listed in the output of `docker-machine ls`. Docker Machine will automatically shut down and terminate the AWS instances.

There you have it---now you can go create Swarm clusters running on AWS using Docker Machine. Have fun!



[link-1]: https://docs.docker.com/swarm/provision-with-machine/
[link-2]: https://www.docker.com/products/docker-machine
[link-3]: https://www.docker.com/products/docker-swarm
[link-4]: https://aws.amazon.com/
[link-5]: https://www.virtualbox.org/
[link-6]: http://www.vmware.com/products/fusion/
[xref-1]: {{< relref "2016-03-22-using-docker-machine-with-aws.md" >}}
[xref-2]: {{< relref "2015-08-04-using-vagrant-docker-machine-together.md" >}}
