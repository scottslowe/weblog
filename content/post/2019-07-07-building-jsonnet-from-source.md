---
author: slowe
categories: Information
comments: true
date: 2019-07-07T23:00:00Z
tags:
- JSON
- Linux
- CLI
- Fedora
- CentOS
title: Building Jsonnet from Source
url: /2019/07/07/building-jsonnet-from-source/
---

I recently decided to start working with `jsonnet`, a data templating language and associated command-line interface (CLI) tool for manipulating and/or generating various data formats (like JSON, YAML, or other formats; see [the Jsonnet web site][link-1] for more information). However, I found that there are no prebuilt binaries for `jsonnet` (at least, not that I could find), and so I thought I'd share here the process for building `jsonnet` from source. It's not hard or complicated, but hopefully sharing this information will streamline the process for others.<!--more-->

As some readers may already know, my primary OS is [Fedora][link-3]. Thus, the process I share here will be specific to Fedora (and/or CentOS and possibly RHEL).

To keep my Fedora installation clean of any unnecessary packages, I decided to use a CentOS 7 VM---instantiated and managed by [Vagrant][link-4]---for the build process. If you don't want to use a build VM, you can omit the steps involving Vagrant. You'll also need to modify the commands used to install the necessary packages (on Fedora, you'd use `dnf` instead of `yum`, for example). Different distributions may also use different package names for some of the dependencies, so keep that in mind.

1. Run `vagrant up` in a directory with a `Vagrantfile` configured to instantiate a CentOS 7 VM. The CentOS 7 box I used was the Libvirt-formatted "centos/7" box, version 1902.01.

2. Log into the VM using `vagrant ssh`.

3. Install the necessary prerequisites with `sudo yum install gcc gcc-c++ git`.

4. Clone [the GitHub repository][link-2] for `jsonnet` with `git clone https://github.com/google/jsonnet.git`. This clones the repository into a directory named "jsonnet" in the current directory.

5. Switch into the directory for the cloned repository with `cd jsonnet`.

6. Run `make` to build Jsonnet. Two binaries will result: `jsonnet` and `jsonnetfmt`.

At this point, you should have functioning binaries, but they're inside the CentOS build VM. To get them copied outside the VM, it is a simple matter of just a few quick commands:

1. First, create an SSH configuration file with `vagrant ssh-config > config`. To know the hostname that Vagrant uses for the VM, you can `cat config` afterward and look at the `Host` line.

2. Copy the `jsonnet` binary with `scp -F config centos-7:/home/vagrant/jsonnet/jsonnet .` (replace `centos-7` with whatever hostname Vagrant is using for the VM, and adjust the path as needed).

3. Copy the `jsonnetfmt` binary with `scp -F config centos-7:/home/vagrant/jsonnet/jsonnetfmt .` (replace `centos-7` with whatever hostname Vagrant is using for the VM, and adjust the path as needed).

At this point, you can destroy the VM with `vagrant destroy` and then move the binaries into a directory in the PATH.

And that's it! As I said, the process isn't hard or difficult, but I did want to share the information nevertheless. Although it didn't take me long to figure out what dependencies were needed to build the Jsonnet binaries, having them spelled out here may still save someone else some precious time.

[Hit me up on Twitter][link-5] if you find that I missed something in the instructions above, or if you have any questions.

[link-1]: https://jsonnet.org/
[link-2]: https://github.com/google/jsonnet
[link-3]: https://getfedora.org/
[link-4]: https://www.vagrantup.com/
[link-5]: https://twitter.com/scott_lowe
