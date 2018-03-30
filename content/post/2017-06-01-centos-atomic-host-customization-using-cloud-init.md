---
author: slowe
categories: Tutorial
comments: true
date: 2017-06-01T00:00:00Z
tags:
- CentOS
- AWS
- Linux
- CLI
title: CentOS Atomic Host Customization Using cloud-init
url: /2017/06/01/centos-atomic-host-customization-using-cloud-init/
---

Back in early March of this year, I wrote a post on [customizing the Docker Engine on CentOS Atomic Host][xref-1]. In that post, I showed how you could use systemd constructs like drop-in units to customize the behavior of the Docker Engine when running on CentOS Atomic Host. In this post, I'm going to build on that information to show how this can be done using `cloud-init` on a public cloud provider (AWS, in this case).<!--more-->

Although I haven't really blogged about it, I'd already taken the information in that first post and written some Ansible playbooks to do the same thing (see [here][link-1] for more information). Thus, one _could_ use Ansible to do this when running CentOS Atomic Host on a public cloud provider. However, much like the original post, I wanted to find a very "cloud-native" way of doing this, and `cloud-init` seemed like a pretty good candidate.

All in all, it was pretty straightforward---with one significant exception. As I was testing this, I ran into an issue where the Docker daemon wouldn't start after `cloud-init` had finished. Convinced I'd done something wrong, I kept going over the files, testing and re-testing (I've been working on this, off and on, since early March). Finally, I turned to the `#atomic` IRC channel on Freenode, and after some help debugging the scenario we discovered that there was an unexpected interaction between how I'd configured Docker and an Atomic Host-specific script named `docker-storage-setup`. As it turns out, `docker-storage-setup` calls `docker` in order to gather some version information. Well, `docker` waits on `docker-storage-setup` to finish...and thus you can see the problem. With the help of the folks in the `#atomic` IRC channel, I submitted [this bug][link-2] to track the problem.

Fortunately, there's a workaround (credit to Dusty Mabe for the workaround). The final version of the `cloud-init` configuration looks like this (I'll explain the workaround after this code):

``` yaml
#cloud-config

groups:
  - docker: [centos,root]
write_files:
  - content: |
      [Unit]
      Description=UNIX Socket for the Docker API

      [Socket]
      ListenStream=/var/run/docker.sock
      SocketMode=0660
      SocketUser=root
      SocketGroup=docker
      Service=docker.service

      [Install]
      WantedBy=sockets.target
    path: /etc/systemd/system/docker.socket
    owner: root:root
    permissions: '0644'
  - content: |
      [Unit]
      Description=TCP Socket for the Docker API

      [Socket]
      ListenStream=2375
      BindIPv6Only=both
      Service=docker.service

      [Install]
      WantedBy=sockets.target
    path: /etc/systemd/system/docker-tcp.socket
    owner: root:root
    permissions: '0644'
  - content: |
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
    path: /etc/systemd/system/docker.service.d/docker-socket.conf
    owner: root:root
    permissions: '0644'
runcmd:
  - [ systemctl, start, docker-storage-setup ]
  - [ systemctl, mask, docker-storage-setup ]
  - [ systemctl, daemon-reload ]
  - [ systemctl, enable, docker.service ]
  - [ systemctl, enable, docker.socket ]
  - [ systemctl, enable, docker-tcp.socket ]
  - [ systemctl, stop, docker.service ]
  - [ systemctl, start, docker.socket ]
  - [ systemctl, start, docker-tcp.socket ]
  - [ systemctl, start, docker.service ]
```

For the most part, this is all pretty straightforward stuff---create a group, add some users to it (although it won't add the "centos" user for some reason; still working on a resolution for that), create some systemd units, etc. The workaround is at the end:

1. First, we'll explicitly call the `docker-storage-setup` unit, so that it goes ahead and runs. Now, because the system hasn't yet reloaded the systemd units, the _old_ Docker configuration is still in place, so we don't run into the loop condition described earlier.

2. Then we mask the `docker-storage-setup` unit so that it can't be inadvertently called/launched elsewhere.

3. Finally, we reload the systemd units, stop the Docker service, and then enable and restart the Docker socket units and Docker service.

There was some discussion as to whether the use of `--no-block` on the final `systemctl` commands was necessary (see [Dusty's post here][link-3]), but additional testing showed that in this case it was not needed.

So there you have it: a way to use `cloud-init` to configure Docker Engine on CentOS Atomic Host running a public cloud provider.

## Additional Resources

If you'd like to play around with this yourself, check out the `centos-atomic/docker-cloudinit` directory in [my GitHub "learning-tools" repository][link-4]. The `cloud-init` configuration is there, as is a simple Bash shell script to launch an instance on AWS to try this out.

Enjoy!



[link-1]: https://github.com/scottslowe/learning-tools/tree/master/centos-atomic/docker-tcp-ansible
[link-2]: https://bugzilla.redhat.com/show_bug.cgi?id=1457978
[link-3]: http://dustymabe.com/2015/08/03/installingstarting-systemd-services-using-cloud-init/
[link-4]: https://github.com/scottslowe/learning-tools/
[xref-1]: {{< relref "2017-03-02-customizing-docker-centos-atomic-host.md" >}}
