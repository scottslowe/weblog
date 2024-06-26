---
author: slowe
categories: Introduction
comments: true
date: 2014-08-18T09:00:00Z
slug: coreos-continued-etcd
tags:
- CLI
- JSON
- Linux
- OSS
- CoreOS
- etcd
title: 'CoreOS Continued: etcd'
url: /2014/08/18/coreos-continued-etcd/
wordpress_id: 3499
---

In this post, I'm going to build on [my earlier introduction to CoreOS][1] by taking a slightly more detailed look at [etcd](https://github.com/coreos/etcd/). etcd is a distributed key-value store (more on that in a moment) and is one of the key technologies that I feel distinguishes [CoreOS](http://coreos.com/) from other Linux distributions.

etcd is not, of course, the only distributed key-value store in town. [Zookeeper](http://zookeeper.apache.org) is another very well-known distributed information store. I'm focusing on etcd here for two reasons. First, it's distributed as part of CoreOS, which means that it isn't necessarily a separate piece of software that you have to install and configure (although some configuration is needed, as I'll attempt to show). Second, future blog posts will talk about tools like [fleet](https://github.com/coreos/fleet/) and others that are being built to leverage etcd, so I want to provide this overview here as a foundation for later posts.

## Data Organization in etcd

As I've mentioned already, etcd is a distributed key-value store. The data within etcd is organized hierarchically, somewhat like a file system. The top of the hierarchy is "/" (root), and from there users can create paths and keys (with values). Later in this post, I'll show you how that's done using both the RESTful etcd API as well as using the `etcdctl` command-line client.

First, though, let's look at an example. Let's say that you wanted to store information about IP endpoints that were running a particular service. You could organize the data within etcd like this:

/ (root, already exists when etcd starts)  

/svclocation (path or directory within etcd)  

/svclocation/instance1 = 10.1.1.20 (a key in etcd and it's associated value)  

/svclocation/instance2 = 10.1.1.21 (another key and associated value)

Because etcd is a distributed key-value store, both the organization of the information as well as the actual information is distributed across all the members of an etcd cluster. If you're interested in more details on exactly how the data is propagated within an etcd cluster, see [here](https://github.com/coreos/etcd/blob/master/Documentation/optimal-cluster-size.md).

## Configuration of etcd

etcd can be configured via command-line flags, environment variables, or configuration file. By default, etcd on CoreOS uses a configuration file generated by cloud-init to set environment variables that affect how etcd operates. My earlier post on [deploying CoreOS on OpenStack with Heat][2] provides a brief example of using cloud-init to configure etcd on CoreOS during deployment.

For example, when you configure the `addr`, `peer-addr`, or `discovery` properties via cloud-init, CoreOS puts those values into a file named `20-cloudinit.conf` in the `/run/systemd/system/etcd.service.d/` directory. That file takes those values and assigns them to the ETCD_ADDR, ETCD_PEER_ADDR, and ETCD_DISCOVERY environment variables, respectively. etcd will read those environment variables when it starts, thus controlling the configuration of etcd. (By the way, the use of the `etcd.service.d` directory for configuration files is a systemd thing.)

## Interacting with etcd

Like most projects these days, etcd provides a RESTful API by which you can interact with it. You can use the command-line utility `curl` to interact with etcd, using some of the techniques I outlined in this post on [interacting with RESTful APIs][3]. Here's a very simple example: using `curl` to recursively list the keys and values in a path within etcd:

```bash
curl -X GET http://10.1.1.7:4001/v2/keys/?consistent=true&recursive;=true&sorted;=false
```

If you want to store some data in etcd (let's say you wanted to store the value "10.1.1.20:80" at the key "/svclocation"), then you'd do something like this:

```bash
curl -L http://10.1.1.7:4001/v2/keys/svclocation -X PUT -d value="10.1.1.20:80"
```

The JSON response (read [this][4] if you're unfamiliar with JSON) will provide confirmation that the key and value were set, along with some additional information.

To read this value back, you'd use `curl` like this:

```bash
curl -L http://10.1.1.7:4001/v2/keys/svclocation
```

etcd would respond with a JSON-formatted response that provides the current value of the specified key.

However, to make it easier, there is a command-line client called `etcdctl` that you can use to interact with etcd. There are both Linux and OS X versions available from [the etcd GitHub page](https://github.com/coreos/etcd/); just make sure you download the version that corresponds to the version of etcd that's running on your CoreOS instance(s). (To determine the version of etcd running on a CoreOS instance, log into the CoreOS instance via SSH and run `etcd --version`.)

Then, to perform the same listing of keys action as the `curl` command I provided earlier, you would run this command:

```bash
etcdctl --peers 10.1.1.7:4001 ls / --recursive
```

Similarly, to set a value at a particular key within etcd:

```bash
etcdctl --peers 10.1.1.7:4001 mk /svclocation 10.1.1.20:80
```

And to get a value:

```bash
etcdctl --peers 10.1.1.7:4001 get /svclocation
```

A couple of points to note:

* By default, `etcdctl` works against a local instance of etcd. In other words, it defaults to connecting to 127.0.0.1:4001. You'll have to use the `--peers` flag if you're running it external to a CoreOS instance. However, if you're running it directly from a CoreOS instance, you can omit the `--peers` flag.

* You can point `etcdctl` against _any_ CoreOS instance in an etcd cluster, because the information stored in etcd is shared and distributed across all the members of the cluster. (That's kind of the whole point behind it.) Note that the `--peers` option supports listing multiple peers in an etcd cluster.

By now, you might be thinking, "OK, this is interesting, but what does it really do for me?" By itself, not necessarily a whole lot. Where etcd starts to become quite useful is when there are other systems that start to leverage information stored in etcd. A couple of examples come to mind:

1. Distributed micro-service architectures---where applications are made up of a group of containerized services distributed across a compute farm---need mechanisms for service registration (registering that a particular service is available) and service discovery (finding out where a particular service is running). etcd could be helpful in assisting with both of these tasks (writing key-value pairs into etcd for registration, reading them back for discovery).

2. "Converting" etcd information into traditional configuration files allows applications that are not etcd-aware to still take advantage of etcd's distributed architecture. For example, there is a project called [confd](https://github.com/kelseyhightower/confd) that takes information stored in etcd and turns it into "standard" configuration files. This means, for example, that you could store dynamic configuration details in etcd, and then use confd to update static configuration files. (A common example of using this is updating an HAProxy configuration file via confd as back-end web server containers start up and shut down. Astute readers will recognize this as a form of service registration and service discovery.)

I'll be building on some of these concepts in future posts. In the meantime, feel free to post any questions, clarifications, or corrections in the comments below.

[1]: {{< relref "2014-08-01-a-quick-introduction-to-coreos.md" >}}
[2]: {{< relref "2014-08-13-deploying-coreos-on-openstack-using-heat.md" >}}
[3]: {{< relref "2014-02-19-using-curl-to-interact-with-a-restful-api.md" >}}
[4]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
