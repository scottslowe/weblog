---
author: slowe
categories: Tutorial
comments: true
date: 2016-09-23T00:00:00Z
tags:
- Docker
- Ubuntu
- CLI
- Ansible
title: Installing a Specific Version of Docker Engine
url: /2016/09/23/installing-specific-version-docker-engine/
---

In this post, I'm going to show you how to install a specific version of the [Docker Engine][link-2] package on [Ubuntu][link-3]. While working on a side project (one that will hopefully bear fruit soon), I found myself in need of installing a slightly older version of Docker Engine (1.11 instead of 1.12, to be specific). While this task isn't hard, it also wasn't clearly spelled out anywhere, and this post aims to help address that shortcoming.

If you've followed the instructions to add the Docker Apt repos to your system as outlined [here][link-1], then installing the Docker Engine (latest version) would be done something like this:

    apt-get install docker-engine

If you do an `apt-cache search docker-engine`, though, you'll find that the "docker-engine" package is a metapackage that refers to a variety of different versions of the Docker Engine. To install a specific version of the Docker Engine, then, simply append the version (as described by the results of the `apt-cache search docker-engine` command) to the end, like this:

    apt-get install docker-engine=1.11.2-0~trusty

This will install version 1.11.2 of the Docker Engine.

You'll use the same syntax when you need to install a specific version of the Docker Engine via [Ansible][link-4] (which is what I was trying to do). For example, this snippet of an Ansible playbook would install version 1.11.2 of the Docker Engine:

``` yaml
- name: Install Docker packages
  apt:
    name: "docker-engine=1.11.2-0~trusty"
    state: "present"
    update_cache: "yes"
    cache_valid_time: "3600"
```

As I said, it's not terribly difficult or complex, but it also wasn't spelled out plainly anywhere that I found in my Internet searches. Hopefully this post will make it easier for others who find themselves in a similar situation.



[link-1]: https://docs.docker.com/engine/installation/linux/ubuntulinux/
[link-2]: https://www.docker.com/products/docker-engine
[link-3]: http://www.ubuntu.com/
[link-4]: https://www.ansible.com/
