---
author: slowe
categories: Tutorial
comments: true
date: 2015-02-20T10:02:00Z
tags:
- Linux
- CLI
- NFS
- Docker
- Storage
- OpenStack
- CoreOS
title: Enabling and Mounting NFS on CoreOS
url: /2015/02/20/config-mount-nfs-coreos/
---

I've written about [CoreOS][link-1] a fair amount (see [here][xref-1], [here][xref-2], and [here][xref-3]), but one of the things that is both good and bad about CoreOS is the automatic update mechanism. It's good because you know your systems will stay up to date, but it's bad if you haven't taken the time to properly address how automatic updates will affect your environment (for example, you've manually started some [Docker][link-5] containers instead of using [systemd][link-6] unit files---when the CoreOS system reboots after an update, your Docker containers will no longer be running). Re-architecting your environment to fully account for this change in architecture and behavior is a larger discussion than can be addressed in a single blog post, but in this post I want to at least tackle one small part of the discussion: separating your persistent data. In this post, I'll show you how to mount an NFS share on a CoreOS instance deployed on [OpenStack][link-2] (or any cloud that leverages cloud-init).

Now, you could probably go into your CoreOS instance and manually make these changes, but that's still thinking the old way. In addition to thinking about keeping persistent data separate, we (data center/cloud architects) also need to think about how we can keep configuration separate from instantiation. We _don't want_ configuration details tied into an instance of CoreOS; we want that configuration applied automatically. This is why I'm using cloud-init to make these changes---this allows you to just re-deploy your CoreOS instances and they'll pick up the new configuration.

CoreOS has some pretty good [documentation][link-7], but as I set out to figure out how to mount an NFS share from a CoreOS instance there were a few missing details. This post, while focused on [GlusterFS][link-4], gave me the missing details I needed. It turns out that two pieces are required to make this work:

1. Creating an NFS environment file that enables the rpc-statd daemon to start.
2. Creating a systemd mount unit file to mount the NFS share.

Fortunately, both of these tasks are easily handled via cloud-init. Here is a sample cloud-config file, written in YAML, that you could pass to CoreOS via cloud-init, that will take care of both the configuration tasks listed above:

```yaml
#cloud-config

write-files:
  - path: /etc/conf.d/nfs
    permissions: '0644'
    content: |
      OPTS_RPC_MOUNTD=""
coreos:
  units:
    - name: rpc-statd.service
      command: start
      enable: true
    - name: mnt-data.mount
      command: start
      content: |
        [Mount]
        What=nfshost.domain.com:/vol2/data
        Where=/mnt/data
        Type=nfs
```

Let's walk through this real quick:

* The `write-files` section creates the NFS environment file that is needed to allow the rpc-statd daemon to start.
* In the `units` section, the first portion enables and starts the rpc-statd service, which is stopped and disabled by default.
* The second part of the `units` section creates a systemd mount unit file. This unit file specifies the remote host and share that should be mounted as well as the location on the local filesystem where it should be mounted. (Obviously, you'd need to edit the YAML listed above to specify the correct NFS host and correct filesystem location.)

If you were deploying a CoreOS instance on OpenStack, you'd simply take the YAML code listed above (with your site-specific details, of course) and paste that into the "Post-Creation" section of the Launch Instance wizard (or you'd supply it via the command-line to the `nova` client). If you're deploying CoreOS via Vagrant (as I described [here][xref-4]), you'd include this content in the `user-data` file that is referenced by the `Vagrantfile`. In both cases, it's cloud-init that will take this information and automatically configure CoreOS appropriately upon deployment. Naturally, you could also combine this information with other cloud-init directives to configure etcd, fleet, Docker, etc.

I know this doesn't seem too complicated (and it isn't, to be honest), but I hope that this information will be useful to someone out there.

[link-1]: https://coreos.com
[link-2]: http://www.openstack.org
[link-3]: http://www.ulabs.uservers.net/howtos/glusterfs-coreos.php
[link-4]: http://www.gluster.org
[link-5]: http://www.docker.com
[link-6]: http://freedesktop.org/wiki/Software/systemd/
[link-7]: https://coreos.com/docs/
[xref-1]: {{< relref "2014-08-01-a-quick-introduction-to-coreos.md" >}}
[xref-2]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
[xref-3]: {{< relref "2014-08-20-coreos-continued-fleet-and-docker.md" >}}
[xref-4]: {{< relref "2015-02-05-vagrant-coreos-etcd-fleet-docker.md" >}}
