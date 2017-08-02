---
author: slowe
categories: Tutorial
comments: true
date: 2014-11-06T09:28:04Z
slug: setting-up-the-tools-for-contributing-to-openstack-documentation
tags:
- CLI
- Collaboration
- Linux
- OpenStack
- OSS
- Writing
- Git
title: Setting up the Tools for Contributing to OpenStack Documentation
url: /2014/11/06/setting-up-the-tools-for-contributing-to-openstack-documentation/
wordpress_id: 3558
---

For non-programmers, making a meaningful contribution to an open source project can be difficult; this is as true for OpenStack as for other open source projects. Documentation is a way to contribute, but in the case of OpenStack there is a non-trivial setup required in order to be able to contribute to the OpenStack documentation. In this post, I'm going to share how to set up the tools to contribute to OpenStack documentation in the hopes that it will help others get past the "barrier to entry" that currently exists.

I've long wanted to be more involved in supporting the OpenStack community, beyond my unofficial support via advocacy and [blogging about OpenStack](/tags/openstack/). I felt that documentation might be a way to achieve that goal. After all, I've written books and have been blogging for 9 years, so I should be able to add some value via documentation contributions. However, the toolchain that the OpenStack documentation uses requires a certain level of familiarity with development-focused tools, and the "how to" guides were less than ideal because of assumptions made regarding the knowledge level of new contributors. For these reasons, I felt that sharing how I (a non-programmer) set up the tools for contributing to OpenStack documentation might encourage other non-programmers to do the same, and thus get more people involved in the project.

This post is not intended to replace any official guides (like [this one](https://wiki.openstack.org/wiki/Documentation) or [this one](https://wiki.openstack.org/wiki/Documentation/HowTo)), but rather to supplement such guides.

## Using a Linux Environment

It's no secret that I use OS X, and the toolchain that the OpenStack documentation team uses is---like the rest of OpenStack---pretty heavily biased toward Linux. One of the deterring factors for me was the level of difficulty to get these tools running on OS X. While it _is_ possible to use the toolchain on OS X directly, it's not necessarily simple or straightforward. It wasn't until just recently that I realized: why not just use a Linux VM? Using a native Linux environment sidesteps all the messiness of trying to get a Linux-centric toolchain running on OS X (or Windows).

I initially started with a locally-hosted Linux VM on my MacBook Air, but after some consideration I decided to alter that approach and build my environment in a cloud VM running Ubuntu 12.04 on Amazon EC2 (via Ravello Systems).

The use of a cloud VM has some advantages and disadvantages:

* Because the toolchain is heavily biased toward Linux, using a Linux-based cloud VM does simplify some things.

* Cloud VMs will typically have much greater bandwidth, making it easier to install packages and pull down Git repositories. This was especially helpful this past week while I was traveling to the OpenStack Summit, where---due to slow conference wireless access---it took 2 hours to clone the GitHub repository for the OpenStack documentation (this is while I was trying to use a locally-hosted VM on my laptop).

* It provides a clean separation between my local workstation and the development environment, meaning I can access my development environment from any system via SSH. This prevents me from having to keep multiple development environments in sync, which is something I would have had to do when working from my home office (where I use my MacBook Air as well as a Mac Pro).

* The most significant drawback is that I cannot work on documentation patches when I have no network connectivity. For example, I'm writing this post as I'm traveling back from Paris. If I had a local development environment, I could assigned a few bugs to myself and worked on them while flying home. With a cloud VM-based environment, I cannot. I could have used a VM hosted on my local laptop (via VirtualBox or VMware Fusion), but that would have negated some of the other advantages.

In the end, you'll have to evaluate for yourself what approach works best for you, your working style, and your existing obligations. However, the use of a Linux VM---local or via a cloud provider---can simplify the process of setting up the toolchain and getting started contributing, and it's an approach I'd recommend for most anyone.

The information presented here assumes you are using an Ubuntu-based VM, either hosted with a cloud provider or on your local system. I'll leave it to the readers to worry about the details of how to turn up the Ubuntu VM via their preferred mechanism.

## Setting Up the Toolchain

Once you've established an Ubuntu Linux instance using the provider of your choice, use these steps to set up the documentation toolchain (note that, depending on the configuration of your Ubuntu Linux instance, you might need to preface these commands with `sudo` where appropriate):

1. Update your system using `apt-get update` and `apt-get -y upgrade`. Depending on the updates that are installed, you might need to reboot.

2. Generate an SSH keypair using `ssh-keygen`. Keep track of this keypair (and make note of the associated password, if you assigned one), as you'll need it later. You can also re-use an existing keypair, if you'd like; I preferred to generate a keypair specifically for this purpose. If you are going to re-use an existing keypair, you'll need to transfer them to this Linux instance via `scp` or your preferred mechanism.

3. Install the necessary prerequisite packages using a standard `apt-get` command, like this: `apt-get install maven git git-review python-pip libxml2-dev     libxslt1-dev python-dev gcc gettext zlib1g-dev`.

4. Configure Git using `git config` to set your name and e-mail address. The e-mail address, in particular, should match what you're using with the OpenStack Foundation. The commands to do this are `git config --global user.name` and `git config --global user.email`.

5. Clone the GitHub repository where the documentation is found using `git clone https://github.com/openstack/openstack-manuals.git`.

6. Install the components required to run tests against the documentation sources (this command assumes that you cloned the repository into a subdirectory named "openstack-manuals", which would be the default result of using the command above to clone the repository) using `pip install -r openstack-manuals/test-requirements.txt`.

7. If you haven't already established a Launchpad account, you'll need one. Go create one (probably best done outside of the Ubuntu Linux environment) and make note of the username. Use your Launchpad account to verify that you can log in to both bugs.launchpad.net as well as review.openstack.org.

8. While logged into review.openstack.org, go to the settings for your account and paste in the contents of the public key (not the private key) you generated earlier (or that you are re-using).

9. Back in the Ubuntu Linux instance, set your Launchpad username using `git config --global --add gitreview.username "<Launchpad username>"`.

10. Change into the directory where you cloned the "openstack-manuals" repository and run `git review -s`. Assuming you already added the "gitreview.username" configuration parameter and that you've already uploaded the public key to your account on review.openstack.org, this should work without any issues. If you forgot to set "gitreview.username", it should prompt you for your Launchpad username. If you forgot to upload the public SSH key, then it will probably error out.

11. If `git review -s` completes without any obvious errors, you can double-check that a Gerrit upstream for the repository was added by using the command `git remote -v`. If you don't see an upstream repo labeled gerrit, then something didn't work. If you do see the upstream repo labeled gerrit, then you should be fine.

Once you've completed these steps, then you're ready to start contributing to OpenStack documentation. I have a separate post planned that describes the process for actually contributing; stay tuned for that soon.

In the meantime, if anyone has any questions, corrections, or clarifications about the information in this post, please speak up in the comments below.
