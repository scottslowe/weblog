---
author: slowe
categories: Tutorial
comments: true
date: 2020-07-08T11:45:00-07:00
tags:
- Docker
- Kubernetes
- macOS
- CLI
title: "Working Around Docker Desktop's Outdated Kubernetes Version"
url: /2020/07/08/working-around-docker-desktops-outdated-kubernetes-version/
---

As of the time that I published this blog post in early July 2020, [Docker Desktop][link-1] for macOS was at version 2.2.0.4 (for the "stable" channel). That version includes a relatively recent version of the Docker engine (19.03.8, compared to 19.03.12 on my [Fedora][link-2] 31 box), but a quite outdated version of [Kubernetes][link-3] (1.15.5, which isn't supported by upstream). Now, this may not be a problem for users who _only_ use Kubernetes via Docker Desktop. For me, however, the old version of Kubernetes---specifically the old version of `kubectl`---causes problems. Here's how I worked around the old version that Docker Desktop supplies. (Also, see the update at the bottom for some additional details that emerged after this post was originally published.)<!--more-->

First, you'll note that Docker Desktop automatically symlinks its version of `kubectl` into your system path at `/usr/local/bin`. You can verify the version of Docker Desktop's `kubectl` by running this command:

    /usr/local/bin/kubectl version --client=true

On my macOS 10.14.6-based system, this returned a version of 1.15.5. [According to GitHub][link-4], v1.15.5 was released in October of 2019. Per [the Kubernetes version skew policy][link-5], this version of `kubectl` would work with with 1.14, 1.15, and 1.16. What if I need to work with a 1.17 or 1.18 cluster? Simple---it just won't work. In my case, I regularly need to work with newer Kubernetes versions, hence the issue with this old bundled version of `kubectl`.

Unfortunately, you can't just delete or rename the symlink that Docker Desktop creates; it will simply re-create the symlink the next time it launches.

So what's the fix? Because `/usr/local/bin` is typically pretty early in the system search path (use `echo $PATH` to see), you'll need to create a symlink _earlier in the search path._ Here's one way to do that:

1. Create a `$HOME/bin` or `$HOME/.local/bin` directory (I prefer the latter, since it mimics my Linux system).
2. Modify your shell startup files to include this new directory in your search path.
3. Create a symlink to a newer version of `kubectl` in that directory.

The steps above should be pretty straightforward, but I'll expand just a bit on item #2. I'm using `bash` as my shell on macOS (again, for similarity across macOS and Linux; I've installed the latest `bash` version via `homebrew`), and so I added this snippet to my `~/.bash_profile`:

```bash
# Set PATH so it includes user's personal bin directories
# if those directories exist
if [ -d "$HOME/.local/bin" ]; then
    PATH="$HOME/.local/bin:$PATH"
fi

if [ -d "$HOME/bin" ]; then
    PATH="$HOME/bin:$PATH"
fi
```

With this in `~/.bash_profile`, _if_ a `$HOME/bin` or `$HOME/.local/bin` directory exists, it gets added to the _start_ of the search path---thus placing it before `/usr/local/bin`, and thus making sure that any executables or symlinks placed there will be found _before_ those in `/usr/local/bin`. Therefore, when I place a symlink to a newer version of `kubectl` in one of these directories, it will get found _before_ Docker Desktop's outdated version. Problem solved! (You can, of course, still access the older Docker Desktop version by using the full path.)

Folks who are familiar with UNIX/Linux are probably already very familiar with this sort of approach. However, I suspect there are a fair number of my readers who may be using macOS-based systems but aren't well-versed in the intricacies of manipulating the `$PATH` environment variable. Hopefully, this article helps those folks. Feel free to contact me on Twitter if you have any questions or feedback on what I've shared here.

**UPDATE 2020-07-09:** Several folks contacted me on Twitter (thank you!) to point out that a slightly newer version of Docker Desktop may be available in the stable channel, although I have not been able to update my installation thus far. The new version, version 2.3.0.3, comes with Kubernetes 1.16.5, and therefore may not be as problematic for folks as the older version I have on my system. Regardless, the workaround remains valid. Thanks to the readers who responded!

[link-1]: https://www.docker.com/products/docker-desktop
[link-2]: https://getfedora.org/
[link-3]: https://kubernetes.io/
[link-4]: https://github.com/kubernetes/kubernetes/releases/tag/v1.15.5
[link-5]: https://kubernetes.io/docs/setup/release/version-skew-policy/
[link-6]: https://twitter.com/scott_lowe
