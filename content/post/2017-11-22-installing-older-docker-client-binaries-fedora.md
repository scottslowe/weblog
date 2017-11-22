---
author: slowe
categories: Tutorial
comments: true
date: 2017-11-22T12:00:00Z
tags:
- Docker
- Linux
- Fedora
- CLI
title: Installing Older Docker Client Binaries on Fedora
url: /2017/11/22/installing-older-docker-client-binaries-fedora/
---

Sometimes there's a need to have different versions of the [Docker][link-4] client binary available. On Linux this can be a bit challenging because you don't want to install a "full" Docker package (which would also include the Docker daemon); you only need the binary. In this article, I'll outline a process I followed to get multiple (older) versions of the Docker client binary on my [Fedora][link-5] 27 laptop.<!--more-->

The process has two steps:

1. Download the RPMs for the older releases from the Docker Yum repository (for Fedora, that repository is [here][link-1]).
2. Extract the files from the RPM without actually installing the RPM.

For step 1, you can use the `curl` program to download specific RPMs. For example, to download version 1.12.6 of the Docker client binary, you'd download the appropriate RPM like this:

    curl -LO https://yum.dockerproject.org/repo/main/fedora/24/Packages/docker-engine-1.12.6-1.fc24.x86_64.rpm

You'll note that the URL above appears to be tied to a particular Fedora version (24, in this case). However, that's only significant/applicable for the _entire_ RPM package; once you extract the specific binaries, you should have no issues running the binaries on a different version (I was able to run older versions of Docker---ranging from 1.9.1 to 1.13.1---on Fedora 27 with no issues).

Once you have the RPM, you can use `rpm2cpio` and `cpio` (as outlined in [this article][link-3]) to extract the files inside the RPM. For example, to extract the files from the RPM downloaded above, you'd use a command like this:

    rpm2cpio docker-engine-1.12.6-1.fc24.x86_64.rpm | cpio -idmv

This will extract all the individual files in the RPM package. In this specific example, you'll see two directories created: a `usr` directory and an `etc` directory. Digging into the `usr` directory, you'll find the `docker` client binary in a `bin` subdirectory. This client binary is really the only thing we need from the package, so we can copy it out:

    cp usr/bin/docker ./docker-1.12.6

You'll note that I "versioned" the file in the name, so as to make it easier for multiple versions of the Docker client binary to co-exist on the same Linux system.

With the Docker client binary extracted, we can remove the extracted files and the downloaded RPM package:

    rm -rf etc
    rm -rf usr
    rm docker-engine-1.12.6-1.fc24.x86_64.rpm

(Important warning: you'll note that my references to `usr` and `etc` lack a preceding forward slash, meaning I'm referencing directories named `usr` and `etc` _in the current directory._ **Be sure** you do not include the leading slash, or you'll be in a world of hurt.)

You can place the extracted binary wherever you'd like (I like to use `/opt/docker/bin`), and repeat the process for a different version. Whenever you need to run a particular version of the Docker client, you have (at least) three options:

1. Somewhere in your `PATH`, create a symbolic link named `docker` that points to the version you want to run. Then, just run `docker` like you normally would.
2. Specify the full path to the client binary you want to run.
3. Temporarily alias `docker` to the particular binary version you want.

If you plan on having a "full" Docker package installed on your Linux system (quite handy, by the way), then option #1 may be a bit more complicated; you may prefer option #2 or option #3. Option #3 probably provides the best balance between ease-of-use and flexibility. Your mileage may vary, of course.

Here's hoping others find this information useful as well!



[link-1]: https://yum.dockerproject.org/repo/main/fedora/
[link-2]: https://download.docker.com/linux/static/stable/x86_64/
[link-3]: https://www.cyberciti.biz/tips/how-to-extract-an-rpm-package-without-installing-it.html
[link-4]: https://www.docker.com
[link-5]: https://getfedora.org/
