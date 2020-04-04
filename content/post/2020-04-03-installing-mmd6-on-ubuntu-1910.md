---
author: slowe
categories: Tutorial
comments: true
date: 2020-04-03T18:15:00-07:00
tags:
- Linux
- CLI
- Writing
- Ubuntu
- Markdown
title: "Installing MultiMarkdown 6 on Ubuntu 19.10"
url: /2020/04/03/installing-mmd6-on-ubuntu-1910/
---

[Markdown][link-2] is a core part of many of my workflows. For quite a while, I've used Fletcher Penny's MultiMarkdown processor (available on [GitHub][link-1]) on my various systems. Fletcher offers binary builds for Windows and macOS, but not a Linux binary. Three years ago, I wrote a post on [how to compile MultiMarkdown 6 for a Fedora-based system][xref-1]. In this post, I'll share how to compile it on an Ubuntu-based system.<!--more-->

Just as in the Fedora post, I used [Vagrant][link-3] with the Libvirt provider to spin up a temporary build VM.

In this clean build VM, I perform the following steps to build a `multimarkdown` binary:

1. Install the necessary packages with this command:

        sudo apt install gcc make cmake git build-essential

2. Clone the source code repository:

        git clone https://github.com/fletcher/MultiMarkdown-6

3. Switch into the directory where the repository was cloned and run these commands to build the binary:

        make
        cd build
        make

4. Once the second `make` command is done, you're left with a `multimarkdown` binary. Copy that to the host system (`scp` works fine). Use `vagrant destroy` to clean up the temporary build VM once you've copied the binary to your host system.

And with that, you're good to go!

[link-1]: https://github.com/fletcher/MultiMarkdown-6/
[link-2]: https://daringfireball.net/projects/markdown/
[link-3]: https://www.vagrantup.com/
[xref-1]: {{< relref "2017-11-25-installing-mmd6-on-fedora-27.md" >}}
