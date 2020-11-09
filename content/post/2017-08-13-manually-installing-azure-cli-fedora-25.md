---
author: slowe
categories: Tutorial
comments: true
date: 2017-08-13T14:10:00Z
tags:
- Microsoft
- Azure
- CLI
- Fedora
- Linux
- macOS
title: Manually Installing Azure CLI on Fedora 25
url: /2017/08/13/manually-installing-azure-cli-fedora-25/
---

For various reasons that we don't need to get into just yet, I've started exploring [Microsoft Azure][link-1]. Given that I'm a command-line interface (CLI) fan, and given that I use [Fedora][link-2] as my primary laptop operating system, this led me to installing the Azure CLI on my Fedora 25 system---and that, in turn, led to this blog post.<!--more-->

## Some Background

First, some background. Microsoft has [instructions for installing Azure CLI on Linux][link-3], but there are two problems with these instructions:

1. Official packages that can be installed via a package manager are only provided for Ubuntu/Debian. Clearly, this leaves Fedora/CentOS/RHEL users out in the cold.

2. Users of other Linux distributions are advised to use `curl` to download a script and pipe that script directly into Bash. ("Danger, Will Robinson!") Clearly, this is _not_ a security best practice, although I am glad that they didn't recommend the use of `sudo` in the mix.

Now, if you dig into #2 a bit, you'll find that the `InstallAzureCli` script you're advised to download via `curl` really does nothing more than download a Python script named `install.py`. The `install.py` Python script really just uses `pip` and `virtualenv` to install the Azure CLI.

This left me wondering---why not just advise users to use `virtualenv` and `pip` directly, instead of writing a shell script that calls a Python script that calls `virtualenv` and `pip`? I posted a message on the Azure Forums to that effect; I'll update this post when I learn more about the rationale.

Since Microsoft's install script uses `virtualenv` and `pip`, I figured I'd just do that myself manually.

## Manually Installing the Azure CLI

With that background in mind, here are the steps I followed to install the Azure CLI.

First, on Fedora 25:

1. Make sure that the "gcc", "libffi-devel", "python-devel", and "openssl-devel" packages are installed (use `dnf` to take care of this). On my primary system, these were already installed, so I used a clean Fedora 25 Cloud Base Vagrant image to test. Only these four packages are prerequisites.

2. Install Pip using `sudo dnf install python-pip`.

3. Once Pip is installed, install virtualenv with `pip install virtualenv`. (I did a `sudo -H pip install virtualenv` to make virtualenv available to all users on the system, but as far as I know that's not required.)

4. Create a new virtual environment with `virtualenv azure-cli` (feel free to use a different name).

5. Activate the new virtual environment (typically accomplished by sourcing the `<virtualenv>/bin/activate` script).

6. Install the Azure CLI with `pip install azure-cli`.

On macOS, the process is very similar:

1. If Pip isn't already installed, install it with `sudo easy_install pip`. (I'd already installed Pip on my macOS systems, so I didn't need this step.)

2. Use Pip to install virtualenv (with `pip install virtualenv`).

3. Create a new virtual environment (`virtualenv azure-cli`).

4. Activate the new virtual environment (source the `activate` script).

5. Install the Azure CLI with `pip install azure-cli`.

Note that I did have the XCode command-line tools installed---in order to have `git`---so that may affect things. I tested this on both El Capitan (10.11.6) and Sierra (10.12.5), and the process was identical on both systems.

So there you have it: how to install the Azure CLI using `virtualenv` and `pip` on Fedora 25 and macOS. Stay tuned for more Azure-related posts later this year.



[link-1]: https://azure.microsoft.com/en-us/
[link-2]: https://getfedora.org/
[link-3]: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
