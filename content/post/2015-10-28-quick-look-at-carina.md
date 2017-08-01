---
author: slowe
categories: Education
comments: true
date: 2015-10-28T00:00:00Z
tags:
- Docker
- CLI
title: A Quick Look at Carina
url: /2015/10/28/quick-look-at-carina/
---

Today at the OpenStack Summit in Tokyo, Rackspace announced [Carina][link-1], a new containers-as-a-service offering that is currently in beta. I took a few minutes to sign up for Carina today and work with it for a little while, and here is a quick introduction.

First, if you're at all unfamiliar with Docker and/or Docker Swarm, have a look at some of these articles off my site. They'll help provide some baseline knowledge:

[A Quick Introduction to Docker][xref-2]  
[Running a Small Docker Swarm Cluster][xref-1]  

I point out these articles because Carina essentially implements hosted Docker Swarm clusters. You can use the Carina CLI tool (as I will in this article) to create one or more clusters, each of which will expose a Docker API endpoint (just like your own homegrown Docker Swarm cluster) against which you can run the Docker client.

Let's take a quick look. These instructions assume that you've already created an account and downloaded [the CLI tool from GitHub][link-2]. I'm assuming you're running Linux or OS X; the commands for Windows would be quite different than what I'll show below.

First, you'll need to set some environmental variables. I prefer to do this in a file that you can then source, so I created a file named `carinarc` and placed this content in the file (note that the username and API key have been changed to protect the innocent):

    export CARINA_USERNAME="bob.jones@bobjones.com"
    export CARINA_APIKEY="96afde175e3b6b09bceba132aa4092af"

This allows you to quickly set the environment variables that the `carina` CLI tool needs with a quick `source carinarc` (or the more succinct form, `. carinarc`).

Once the environment variables are set (note that you can also include these values on the command line, but I find setting environment variables to be much easier), create a cluster like this:

    carina create --nodes=2 foo

The `--nodes=2` specifies how many nodes should be in the cluster (you can change this later), and the `foo` is the name of the cluster. Give it a couple of minutes, then run `carina list`. You should get back a list of the current Carina clusters (in this case, there will be only one cluster, unless you've also created some other clusters).

Once the "Status" column in the output of `carina list` shows "Active", you're able to start running commands against it. First, though, you need to configure the Docker client to know how to communicate with your Carina-hosted Docker Swarm cluster. Do that with this command:

    eval "$(carina credentials foo)"

Replace `foo` with the name of your cluster, naturally. This will download a set of files (into the `.carina/clusters/<username>/<clustername>` directory) and source them into your current environment. (Note that at any point later you can simply `source` the `docker.env` file in the appropriate cluster directory).

Once you've source the Docker environment files, you can run the Docker client against your Carina cluster:

* `docker ps` will show you the current running containers on your cluster
* `docker images` will show the images that are present on your cluster (you'll note the Swarm image is there by default, as would be fully expected for a Docker Swarm cluster)
* `docker info` will show you more information about your cluster
* `docker run` will schedule a container to run on one of the nodes in the cluster (assuming you created a cluster with more than 1 node)

At this point in time, Carina is leveraging the standard Docker network model with the Linux bridge (that may change following the official release of libnetwork with Docker 1.9).

When you're done with the cluster, simply run `carina delete foo` (substituting the correct cluster name, obviously) and you're all set. `carina help` will show you other options you can run against your cluster.

## Why Should I Care?

As I've stated on a few different occasions, having a clean "consumer/provider" model with easy consumption is a key point of building infrastructures that your users (consumers) _want_ to use---as opposed to your users actively trying to find ways of not using your infrastructure. I think that Carina does a good job of giving folks who want to use Docker as a vehicle for packaging/deployment an easy, fairly seamless way to consume Rackspace infrastructure (at a cost, naturally) without having to jump through a bunch of hoops.



[link-1]: http://getcarina.com
[link-2]: https://github.com/getcarina/carina/releases/
[xref-1]: {{< relref "2015-03-06-running-own-docker-swarm-cluster.md" >}}
[xref-2]: {{< relref "2014-03-11-a-quick-introduction-to-docker.md" >}}
