---
author: slowe
categories: Education
comments: true
date: 2017-03-02T00:00:00Z
tags:
- CentOS
- Docker
- Linux
- Vagrant
title: Customizing Docker Engine on CentOS Atomic Host
url: /2017/03/02/customizing-docker-centos-atomic-host/
---

I've been spending some time recently with CentOS Atomic Host, the container-optimized version of CentOS (part of [Project Atomic][link-3]). By default, the [Docker Engine][link-4] on CentOS Atomic Host listens only to a local UNIX socket, and is not accessible over the network. While CentOS has its own particular way of configuring the Docker Engine, I wanted to see if I could---in a very "systemd-like" fashion---make Docker Engine on CentOS listen on a network socket as well as a local UNIX socket. So, I set out with an instance of CentOS Atomic Host and [the Docker systemd docs][link-1] to see what I could do.

The default configuration of Docker Engine on CentOS Atomic Host uses a systemd unit file that references an external environment file; specifically, it references values set in `/etc/sysconfig/docker`, as you can see from this snippet of the `docker.service` unit file:

```
ExecStart=/usr/bin/dockerd-current \
          --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current \
          --default-runtime=docker-runc \
          --exec-opt native.cgroupdriver=systemd \
          --userland-proxy-path=/usr/libexec/docker/docker-proxy-current \
          $OPTIONS \
          $DOCKER_STORAGE_OPTIONS \
          $DOCKER_NETWORK_OPTIONS \
          $ADD_REGISTRY \
          $BLOCK_REGISTRY \
          $INSECURE_REGISTRY
```

The `$OPTIONS` variable, along with the other variables at the end of the ExecStart line, are defined in `/etc/sysconfig/docker`. That value, by default, looks like this:

```
OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false'
```

I _could_, therefore, simply modify `/etc/sysconfig/docker` to make it listen on a network socket. However, I wanted to stretch my knowledge and try to do this in a "systemd-friendly" fashion; in other words, I wanted to more fully take advantage of systemd's functionality and architecture. Exploring [the systemd documentation][link-5], Docker's systemd docs, and building upon what I've seen in other distributions, I came up a three-pronged approach.

First, I'd use a systemd socket file for the local UNIX socket. This systemd socket file looks like this:

```
[Unit]
Description=UNIX Socket for the Docker API
PartOf=docker.service

[Socket]
ListenStream=/var/run/docker.sock
SocketMode=0660
SocketUser=root
SocketGroup=docker

[Install]
WantedBy=sockets.target
```

Next, I'd use a second systemd socket file to listen on a TCP port (the default non-SSL port of 2375). This socket file looks like this:

```
[Unit]
Description=TCP Socket for the Docker API

[Socket]
ListenStream=2375
BindIPv6Only=both
Service=docker.service

[Install]
WantedBy=sockets.target
```

Third and finally, I'd use a systemd drop-in to modify the default systemd unit for the Docker Engine _without_ having to modify the default unit file. This drop-in looks like this:

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd-current -H fd:// \
          --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current \
          --default-runtime=docker-runc \
          --exec-opt native.cgroupdriver=systemd \
          --userland-proxy-path=/usr/libexec/docker/docker-proxy-current \
          $OPTIONS \
          $DOCKER_STORAGE_OPTIONS \
          $DOCKER_NETWORK_OPTIONS \
          $ADD_REGISTRY \
          $BLOCK_REGISTRY \
          $INSECURE_REGISTRY
```

This systemd drop-in modifies the value of the "ExecStart" line from the default unit file. First, it clears the value; then, it provides its own value. If you look carefully, you'll see that the _only_ change to the ExecStart line from the original is the addition of `-H fd://`, which tells the Docker Engine to listen to a file descriptor. Where does this file descriptor come from? The systemd socket files, which will pass the descriptor for the socket---either the local UNIX socket or the network socket---to the Docker Engine.

This solution means I don't have to edit the default Docker Engine systemd unit and I don't have to edit the `/etc/sysconfig/docker` file.

To put these files in place, I followed these steps:

1. First, stop the Docker Engine service.
2. Next, copy the socket files into place. They'll go into `/etc/systemd/system`.
3. Copy the systemd drop-in into place. This will go into the `/etc/systemd/system/docker.service.d` directory, which may need to be created (it probably won't already exist).
4. Run `systemctl daemon-reload` to reload the systemd unit files.
5. Create a "docker" group (this is needed by the UNIX socket).
6. Start the Docker sockets.
7. Start the Docker Engine service.

After completing this process, you'll be able to communicate with this instance of Docker Engine _either_ locally (via the UNIX socket) or remotely (via the TCP port).

Although I haven't tested it yet, this process should be reasonably easy to replicate via [cloud-init][link-6], meaning you could use this process to customize CentOS Atomic Host instances in an OpenStack cloud or running on a public cloud like AWS.

## Additional Resources

To experiment with this sort of thing yourself, I've added some content to [my GitHub "learning-tools" repository][link-2] specifically addressing this topic. Look in the `centos-atomic/docker-tcp` directory (or the `centos-atomic/docker-tcp-ansible` directory if you want Ansible support) for a Vagrant environment that models what I've described in this post.



[link-1]: https://docs.docker.com/engine/admin/systemd/
[link-2]: https://github.com/scottslowe/learning-tools/
[link-3]: https://www.projectatomic.io/
[link-4]: https://www.docker.com/
[link-5]: https://www.freedesktop.org/wiki/Software/systemd/
[link-6]: https://cloud-init.io/