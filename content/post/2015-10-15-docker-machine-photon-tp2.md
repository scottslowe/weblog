---
author: slowe
categories: Tutorial
comments: true
date: 2015-10-15T00:00:00Z
tags:
- Docker
- CLI
- VMware
- Photon
title: Using Docker Machine with Photon TP2
url: /2015/10/15/docker-machine-photon-tp2/
---

In this post, I'll show you how to use [Docker Machine][link-1] in conjunction with [VMware Photon OS][link-2] Technical Preview 2 (aka "TP2"). Given that Photon was designed to host containers, this is---for the most part---pretty straightforward. There are a couple of glitches that I need to point out, though, that might hang up new users.

First off, kudos to Fabio Rapposelli for taking some time at VMworld to help me work through the details. I really appreciate his time!

If you want to use Docker Machine with Photon, there are four major requirements today (stress the word "today," as all these products are rapidly evolving and these requirements may soon disappear or change):

1. You'll need to use [Fabio's special build of Docker Machine][link-3] that includes support for AppCatalyst and Photon OS. I anticipate that support for AppCatalyst and Photon OS (the latter of which is what's really needed in this case) will get rolled into Docker Machine (main branch) soon, but for now a different build is needed. (If you don't use Fabio's build, Docker Machine will report an "unrecognized OS" or similar).

2. You'll have to use Docker Machine's `generic` driver. Even Fabio's branch of Docker Machine doesn't yet (to my knowledge) have the ability to spin up Photon OS instances on-demand via Docker Machine.

3. You'll have to manually enable and start the Docker daemon on the Photon OS instance that Docker Machine's `generic` driver will consume. Docker comes preinstalled on Photon OS, but not enabled and not started. If you don't first enable and start Docker, Docker Machine will fail with a command along the lines of "Docker command not found" or similar. (The error message is actually quite un-helpful.)

    To enable Docker on Photon OS, you'll need to run this command:

        systemctl enable docker.service

    To start Docker on Photon OS, you'll need to run this command:

        systemctl start docker.service

4. You'll need Internet connectivity. (This is probably a given, but I wanted to point it out nevertheless.)

Once you've satisfied these 4 conditions, then it's super easy. Just run Docker Machine using a command that looks something like this (this command is for a Vagrant-managed Photon OS instance):

    docker-machine create -d generic \
    --generic-ssh-user vagrant \
    --generic-ssh-key ~/.vagrant.d/insecure_private_key \
    --generic-ip-address 192.168.100.101 \
    photon-01

Obviously, you'd need to substitute the correct values for your environment. Once Docker Machine completes successfully, then you can run `eval "$(docker-machine env photon-01)"` and your system will be ready to run the Docker client against the Docker daemon inside the Photon OS instance. All set!



[link-1]: http://docs.docker.com/machine/
[link-2]: https://vmware.github.io/photon/
[link-3]: https://github.com/frapposelli/machine
