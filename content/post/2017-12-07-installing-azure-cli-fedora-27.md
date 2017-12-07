---
author: slowe
categories: Tutorial
comments: true
date: 2017-12-07T21:00:00Z
tags:
- Linux
- CLI
- Azure
- Fedora
- Microsoft
title: Installing the Azure CLI on Fedora 27
url: /2017/12/07/installing-azure-cli-fedora-27/
---

This post is a follow-up to a post from earlier this year on [manually installing the Azure CLI on Fedora 25][xref-1]. I encourage you to refer back to that post for a bit of background. I'm writing this post because the procedure for manually installing the Azure CLI on [Fedora][link-1] 27 is slightly different than the procedure for Fedora 25.<!--more-->

Here are the steps to install the Azure CLI into a Python virtual environment on Fedora 27. Even though they are _almost_ identical to the Fedora 25 instructions (one additional package is required), I'm including all the information here for the sake of completeness.

1. Make sure that the "gcc", "libffi-devel", "python-devel", "openssl-devel", "python-pip", and "redhat-rpm-config" packages are installed (you can use `dnf` to take care of this). Some of these packages may already be installed; during my testing with a Fedora 27 Cloud Base Vagrant image, these needed to be installed. (The change from Fedora 25 is the addition of the "redhat-rpm-config" package.)

2. Install virtualenv either with `pip install virtualenv` or `dnf install python2-virtualenv`. I used `dnf`, but I don't think the method you use here will have any material effects.

3. Create a new Python virtual environment with `virtualenv azure-cli` (feel free to use a different name).

4. Activate the new virtual environment (typically accomplished by sourcing the `azure-cli/bin/activate` script; substitute the name you used when creating the virtual environment if you didn't name it `azure-cli`).

5. Install the Azure CLI with `pip install azure-cli`. Once this command completes, you should be ready to roll.

That's it!



[link-1]: https://getfedora.org/
[xref-1]: {{< relref "2017-08-13-manually-installing-azure-cli-fedora-25.md" >}}
