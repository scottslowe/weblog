---
author: slowe
categories: Explanation
comments: true
date: 2018-09-13T22:00:00Z
tags:
- Google
- Cloud
- Docker
- CLI
- Linux
title: Running the gcloud CLI in a Docker Container
url: /2018/09/13/running-gcloud-cli-in-a-docker-container/
---

A few times over the last week or two I've had a need to use the `gcloud` command-line tool to access or interact with [Google Cloud Platform (GCP)][link-3]. Because working with GCP is something I don't do very often, I prefer to not install the Google Cloud SDK; instead, I run it in a [Docker][link-4] container. However, there is a trick to doing this, and so to make it easier for others I'm documenting it here.<!--more-->

The `gcloud` tool stores some authentication data that it needs every time it runs. As a result, when you run it in a Docker container, you must take care to store this authentication data _outside_ the container. Most of the tutorials I've seen, like [this one][link-1], suggest the use of a named Docker container. For future invocations after the first, you would then use the `--volumes-from` parameter to access this named container.

There's only one small problem with this approach: what if you're using another tool that also needs access to these GCP credentials? In my case, I needed to be able to run [Packer][link-2] against GCP as well. If the authentication information is stored inside a named Docker container (and then accessed using the `--volumes-from` parameter), that information won't be accessible to commands _not_ running in a Docker container.

The fix for this is to bind mount a host path into the container instead of using a named volume. First, create the `~/.config/gcloud` directory on your system. Then you'll initialize and authenticate with this command:

    docker run --rm -ti -v $HOME/.config/gcloud:/root/.config/gcloud \
    google/cloud-sdk gcloud init

This will take you through the initialization/authentication process, and will store the authentication information outside the container (so that tools like Packer can still access them). From there, just include the bind mount for future invocations of the Docker image. For example, to see a list of your GKE clusters:

    docker run --rm -ti -v $HOME/.config/gcloud:/root/.config/gcloud \
    google/cloud-sdk gcloud container clusters list

You could then make this easier for yourself with a Bash alias:

    alias gcloud="docker run --rm -ti \
    -v $HOME/.config/gcloud:/root/.config/gcloud \
    google/cloud-sdk gcloud"

Nothing terribly new or revolutionary here, but I hope it's useful to someone nevertheless.

[link-1]: https://adilsoncarvalho.com/using-gcloud-in-a-docker-container-dd5f9eea5bbc
[link-2]: https://www.packer.io/
[link-3]: https://cloud.google.com/
[link-4]: https://www.docker.com/
