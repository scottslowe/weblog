---
author: slowe
categories: Information
comments: true
date: 2018-03-06T12:00:00Z
tags:
- Linux
- Fedora
- Debian
- Vagrant
- Terraform
- Ansible
title: 'Recent Changes in my "Learning Tools" Repository'
url: /2018/03/06/recent-changes-learning-tools-repository/
---

A couple years ago, I created [a "learning-tools" repository on GitHub][link-1] with the goal of creating environments/tools that would help others learn new technologies. At first, the contents of the repository were almost exclusively leveraging [Vagrant][link-2], but over time I've extended the environments to also leverage [Ansible][link-3] and to use tools such as [Terraform][link-4]. Over the past month or so, I've made a few additional (albeit relatively minor) updates that I also wanted to share.<!--more-->

As I said, the updates are relatively minor:

* I've added environments for running generic versions of Fedora Atomic Host (26 and 27), Ubuntu 16.04, and Debian 9.x. These environments are probably of limited value by themselves, but in the future I may use them as the basis for more complex environments based on these operating systems. Of course, others may leverage them as the basis for projects of their own.
* I've added [Libvirt][link-6] support for a number of the Vagrant-based environments, based on [my experience with the Vagrant Libvirt provider][xref-1]. This support is limited to areas where I was able to find Libvirt-formatted Vagrant boxes, so you'll find Libvirt support for the environments using CentOS Atomic Host, Fedora Atomic Host, and Debian. The generic Ubuntu 16.04 environment also supports Libvirt, but most other Ubuntu-based environments do not (yet).
* Wherever possible, I've removed references to Vagrant boxes that I personally built/maintained (like the `slowe/ubuntu-trusty-x64` Vagrant box). I simply don't have the time/bandwidth to maintain those boxes properly, and feel like users of the repository are best served by me pointing them to boxes that are kept more up-to-date. There are a few exceptions where these boxes are still referenced; just be aware that I'm no longer updating those boxes (I recommend you switch to a box that _is_ maintained).

In the coming months, I plan to expand the repository to include more content on [Kubernetes][link-5] and related projects/technologies, so stay tuned for that. Until then, if there's additional stuff you'd like to see---feel free to fork the repository, contribute your changes, and submit a pull request! Alternately, you're welcome to open an issue on the repository with requests for additional technologies (or scenarios/use cases) you'd like to see.

[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: https://www.vagrantup.com/
[link-3]: https://www.ansible.com/
[link-4]: https://www.terraform.io/
[link-5]: https://kubernetes.io/
[link-6]: https://libvirt.org/
[xref-1]: {{< relref "2017-12-06-using-vagrant-with-libvirt-on-fedora.md" >}}
