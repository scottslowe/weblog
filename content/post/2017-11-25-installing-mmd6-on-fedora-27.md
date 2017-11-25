---
author: slowe
categories: Tutorial
comments: true
date: 2017-11-25T12:00:00Z
tags:
- Linux
- CLI
- Writing
- Fedora
title: Installing MultiMarkdown 6 on Fedora 27
url: /2017/11/25/installing-mmd6-on-fedora-27/
---

Long-time readers are probably aware that I'm a big fan of [Markdown][link-2]. Specifically, I prefer the MultiMarkdown variant that adds some additional extensions beyond "standard" Markdown. As such, I've long used Fletcher Penny's MultiMarkdown processor (the latest version, version 6, is [available on GitHub][link-1]). While Fletcher offers binary builds for Windows and macOS, the Linux binary has to be compiled from source. In this post, I'll provide the steps I followed to compile a MultiMarkdown binary for [Fedora][link-3] 27.<!--more-->

The ["How to Compile" page on the MMD-6 Wiki][link-4] is quite sparse, so a fair amount of trial-and-error was needed. To keep my main Fedora installation as clean as possible, I used [Vagrant][link-5] with the Libvirt provider to create a "build VM" based on the "fedora/27-cloud-base" box.

Once the VM was running, I installed the necessary packages to compile the source code. It turns out only the following packages were necessary:

    sudo dnf install gcc make cmake gcc-c++

Then I downloaded the source code for MMD-6:

    curl -LO https://github.com/fletcher/MultiMarkdown-6/archive/6.2.3.tar.gz

Unpacking the archive with `tar` created a `MultiMarkdown-6-6.2.3` directory. Changing into that directory, then the instructions from the Wiki page worked as expected:

    make
    cd build
    make

I did _not_ run `make test`, though perhaps I should have to ensure the build worked as expected. In any case, once the second `make` command was done, I was left with a `multimarkdown` binary that I copied out to my Fedora 27 host system via `scp`. Done!



[link-1]: https://github.com/fletcher/MultiMarkdown-6/
[link-2]: https://daringfireball.net/projects/markdown/
[link-3]: https://getfedora.org
[link-4]: https://github.com/fletcher/MultiMarkdown-6/wiki/How-to-Compile
[link-5]: https://www.vagrantup.com/
