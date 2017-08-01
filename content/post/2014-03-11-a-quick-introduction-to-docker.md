---
author: slowe
categories: Education
comments: true
date: 2014-03-11T09:00:00Z
slug: a-quick-introduction-to-docker
tags:
- Docker
- Linux
- LXC
- Virtualization
title: A Quick Introduction to Docker
url: /2014/03/11/a-quick-introduction-to-docker/
wordpress_id: 3418
---

If you've been following trends in the "cloud" world, you've probably heard of [Docker](http://www.docker.io/). In this post I'm going to provide a quick introduction to Docker, which might be helpful if you're wondering what Docker is and why it's garnered so much attention.

The best way to describe Docker is to use the phrase from the Docker web site---Docker is "an open source project to pack, ship and run any application as a lightweight container." The idea is to provide a comprehensive abstraction layer that allows developers to "containerize" or "package" any application and have it run on any infrastructure. The use of _container_ here refers more to the consistent, standard packaging of applications rather than referring to any underlying technology (a distinction that will be important in a moment). The most common analogy used to help people understand Docker is saying that Docker containers are like shipping containers: they provide a standard, consistent way of shipping just about anything. Docker containers provide a standard, consistent way of packaging just about any application. (At least, that's the idea.)

So what are the underlying technologies that make up Docker?

* Docker leverages LXC (Linux Containers), which encompasses Linux features like cgroups and namespaces for strong process isolation and resource control. If you're not familiar with LXC, I suggest having a look at my [introductory post on LXC][1]. Docker's use of LXC is why I mentioned earlier that Docker's use of _container_ is more about consistent/standard packaging than any specific technology. While Docker currently leverages LXC, the Docker architecture isn't necessarily limited to LXC. In theory, Docker could leverage KVM to do the same things it does today with LXC. _(Update: While in the process of finishing this post, Docker released version 0.9, which does offer the ability to replace LXC with a different back-end "engine" called libcontainer.)_

* Docker leverages a copy-on-write filesystem (currently AUFS, but other filesystems are being investigated). This allows Docker to instantiate containers (using that term to refer to Docker's construct, not the LXC construct) very quickly---instead of having to make full copies of whatever files comprise a container, Docker can basically use "pointers" back to existing files. It seems to me this is a big part of what makes Docker so useful.

* Perhaps due in part to the use of a copy-on-write file system, Docker containers are easily "linked" to other containers. Some people refer to this as stacking containers (continuing the shipping container analogy); to me, it makes more sense to talk about layering one container on top of another. For example, you might create a container that is based on a base Ubuntu image, and then in turn create another container that is based on the first container.

* Docker uses a "plain text" configuration language (I suppose you could think of it as a domain-specific language, or DSL) to control the configuration of an application container, such as what files should (or should not) be included, what network ports should be open, and what processes/applications should be running inside the container. (I'll provide an example of a Docker file in just a few moments.)

Now that you have an idea of the basics, let's take a deeper look. For the purposes of this article, I'm using Ubuntu 12.04 LTS as the Linux platform. Please keep in mind that different distributions may use different commands.

## Installing Docker

To install Docker on Ubuntu, it's a pretty straightforward process. (Note that I've line-wrapped some of the commands in this section, denoted by backslashes.)

First, you'll want to make sure you are running at least the 3.8 kernel. (Newer versions of 12.04 LTS, such as 12.04.4, appear to be installing the 3.11 kernel by default, so you may not have to do this step.) To install the 3.8 kernel (in case you're running something older), just run these two commands and reboot:

    sudo apt-get update
    sudo apt-get install linux-image-generic-lts-raring \
    linux-headers-generic-lts-raring

Next, add the Docker repository key to your local Apt keychain:

    sudo apt-key adv --keyserver keyserver.ubuntu.com \
    --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

Add the Docker repository to your list of repositories:

    sudo sh -c "echo deb http://get.docker.io/ubuntu docker main \
    > /etc/apt/sources.list.d/docker.list"

Finally, install Docker with an `apt-get` combo:

    sudo apt-get update
    sudo apt-get install lxc-docker

Note that this automatically installs LXC, but an "older" version (0.7.5). I haven't tested to see if Docker will work with the newer alpha release of LXC 1.0.0 that is available in the precise-backports repository (see [this post][2] for more information).

## Launching a Simple Docker Container

Now that Docker is installed, you can launch a quick Docker container like this:

    sudo docker run -i -t ubuntu /bin/bash

If everything is working, the first time you run this command it will automatically download an Ubuntu image (as specified by "ubuntu" on that command line) and create a simple Docker container that just runs the bash shell. You'll get dropped into an odd prompt that will look something like this (the numbers will change from system to system):

    root@4a2f737d6e2e:/#

You're now running in an Ubuntu environment, but it's a clean environment---there are no daemons running, nothing listening on various network ports, nothing else except the bash shell. If you run `ifconfig`, you'll see that eth0 has an IP address, but that's it. When you type `exit`, the container will terminate and you'll be returned to the prompt for your host system.

Now, let's take a look at what happened as a result of running that command. If you run `sudo docker images`, you will see a list of available Docker images that were downloaded to your local system. They'll all be listed as "ubuntu" in the first column, and it's the second column that will differentiate them---you'll see listings for 12.04, 12.10, 13.04, 13.10, etc. You could run a specific Ubuntu image with a command like this:

    sudo docker run -i -t ubuntu:13.10 /bin/bash

If you run this command, you'll note that you're launched into this new container _very_ quickly. This is one of the strengths of Docker: once an image has been downloaded, launching containers based on that image is very fast.

One other thing you'll note about Docker is that the containers you start up are ephemeral---meaning that changes you make to the container aren't persistent. Try this: start up a container (you can use a simple container like the examples I've provided already), create a file, and then exit the container. Start the container again, and you'll see that the file you created previously no longer exists. There are workarounds for this, but for the sake of keeping this post to a manageable size I'll save discussing those workarounds for another time.

## Launching More Complex Docker Containers

So what if you need a Docker container that _does_ need to have a daemon running and listening on a network port? It's great that you can run a container for the bash shell, but that's not particularly useful. What if you wanted to run a MySQL container, an SSH container, or a web server container?

To do something like that, you'll need to utilize a _Dockerfile._ A Dockerfile is a plain text file that tells Docker exactly how to construct a Docker image. (I mentioned earlier that Docker had its own DSL; this is what I'm talking about.) Here's an example Dockerfile:

    FROM ubuntu:12.04
    MAINTAINER Joe Shmoe "jshmoe@domain.com"
    RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
    RUN apt-get update
    RUN apt-get install -y nginx
    EXPOSE 80
    CMD ["nginx"]

The syntax for a Dockerfile is `INSTRUCTION arguments`, where "INSTRUCTION" refers to commands like FROM, RUN, or CMD. Here are a few quick notes:

* The FROM command is what enables Docker containers to be based on other Docker containers. In this case, the Docker container is based on the Ubuntu image; specifically, the Ubuntu 12.04 image.

* The RUN commands execute various commands within the new container. You'd use RUN instructions to install packages, modify files, etc.

* The EXPOSE command exposes a listening port on a Docker container.

* The CMD instruction tells what command/daemon to run when the container launches.

I don't have room here for a full Dockerfile tutorial, but Docker themselves has [a great one](http://docs.docker.com/userguide/dockerimages/#building-an-image-from-a-dockerfile). Suffice it to say that if you want anything more complex than a simple bash shell, you're probably going to need to use a Dockerfile.

Once you have a working Dockerfile (just as a heads-up, the above example doesn't actually work---sorry), you can create a Docker image from that Dockerfile using the `docker build` command. Once you have a working image, you can then base other Docker containers on _that_ image (using the FROM instruction in a subsequent Dockerfile).

## Exploring Other Docker Commands

There are also some other Docker commands you'll find useful:

* The `docker images` command lists all the Docker images available on the system. This includes images that you've downloaded from elsewhere as well as images you've created.

* As I mentioned earlier, `docker build` allows you to create your own images from a Dockerfile.

* To see which Docker containers are running, use `docker ps`.

* The `docker rmi` command removes Docker images.

* Finally, just run `docker help` if you need to see what other commands are available.

## Trying Out Docker

You don't need a lot to try out Docker; a simple Ubuntu 12.04 LTS VM running on VirtualBox, VMware Fusion, or VMware Workstation will work fine. Nested virtualization support isn't required when using LXC, and the Docker containers themselves are pretty lightweight. To try out Docker, you only need to create a VM and follow the instructions provided earlier for installing Docker. Once you've got that, you're ready to explore it for yourself.

For additional information or another perspective on Docker, I recommend taking a look at [this presentation on SlideShare](http://www.slideshare.net/jamtur01/introduction-to-docker-30285720) by James Turnbull (formerly with Puppet Labs, now with Docker).

As always, all courteous questions, thoughts, suggestions, or corrections are welcome.

[1]: {{< relref "2013-11-25-a-brief-introduction-to-linux-containers-with-lxc.md" >}}
[2]: {{< relref "2014-02-10-installing-ubuntu-packages-from-a-specific-repository.md" >}}
