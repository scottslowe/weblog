---
author: slowe
categories: Explanation
comments: true
date: 2026-03-02T08:00:00-05:00
tags:
- Azure
- CLI
- Docker
- Linux
title: Running the Azure CLI in a Container
url: /2026/03/02/running-azure-cli-in-container/
---

Like perhaps some readers, I am quite particular about what gets installed on my systems. I try to keep my systems as "clean" as possible, doing my best to avoid tools that have an extensive list of dependencies that must be installed and updated. Where that isn't possible---such as with the Azure CLI, which has a massive number of Python modules that are required in order for the tool to function---I will use various isolation mechanisms. For the Azure CLI, that's typically been a Python virtual environment. Somewhat recently, though, I had an idea to try using a container. In this post, I'll share what worked and what did not work when trying to run the Azure CLI in a container.<!--more-->

First, though, a disclaimer: I am not an Azure expert, nor am I a Python expert. I know enough to get by. If I share something here that's incorrect, please contact me and constructively show me my errors so that I can fix them.

Before I started down this path, I was sure this would be a slam dunk. I mean, this is what containers are for, right? If you do some web searches for running the [Azure CLI][link-1] in a container, you'll find articles that give you this command line:

```bash
docker run -it mcr.microsoft.com/azure-cli
```

(Note that this assumes you are using Docker instead of something like [Podman][link-2].)

This _does_ work, as long as you're willing to operate in an interactive shell in the container. I was looking for something a bit different: I wanted to create an alias for `az` that executed the container in my current shell instead of putting me into a separate shell inside the container. Something like this, for example:

```bash
alias az="docker container run --rm mcr.microsoft.com/azure-cli az"
```

I quickly found that you need to map in your Azure configuration directory from outside the container, or your configuration won't persist. (In theory you could remove the `--rm` parameter and keep the container around.) Now the `docker` command in your alias starts to look like this:

```bash
docker container run --rm -v ${HOME}/.azure:/root/.azure mcr.microsoft.com/azure-cli az
```

Oh, you need access to your SSH keys? You'll have to map those in, too:

```bash
docker container run --rm -v ${HOME}/.azure:/root/.azure \
-v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli az
```

Except that then I remember that I am running as root in the container, and so any files created by the Azure CLI will be owned as root outside the container, too. No worries; I'll just run it as a different user ID. Unfortunately, that involves building and maintaining my own container image to specify that the Azure CLI should run as something other than root. At this point, you'll likely come to the same conclusion I did, and decide that a Python virtual environment is fine.

I was about to classify this experiment as "Death by a Thousand Cuts", because every time I turned around I was finding some annoyance or blocker caused by Docker and containers. And then I remembered [Distrobox][link-3].

Distrobox uses a container, but the container is "tightly integrated" with the host. For example, a Distrobox container shares the home directory of the user. For this use case, this makes the situation appreciably simpler---I no longer need to map in volumes for the Azure configuration or for my SSH keys.

Getting this working with Distrobox was only a few steps:

1. Create a new Distrobox container (I chose to use Debian 13):

    ```bash
    distrobox create -n azcli -i debian:13
    ```

2. Enter the new container with `distrobox enter azcli` and install a few tools. (This is one oddity of Distrobox: it will use the `~/.bashrc` from your host, which may references tools that don't exist in the container. In my situation, I needed to install [Starship][link-6] and a few command-line tools.)
3. Install the Azure CLI in the Distrobox container by following [the Linux installation instructions][link-5]. (One side note here: I chose Debian 13, but technically Debian 13 isn't yet supported.)
4. In the Distrobox container, export the Azure CLI so it is accessible from the host system:

    ```bash
    distrobox-export -b /usr/bin/az
    ```

If you omit step 4, then you are _sort of_ back to where I was earlier in this article---you'll enter the Distrobox container and run the `az` commands from a separate shell. However, once you complete step 4 (which is done from within the Distrobox container), then **on the host** you will run `az`. That's it! All of the files, all the dependencies, etc., are all stored in the Distrobox container. Your system remains "clean."

## Wrapping Up and Additional Resources

After trying to use a "standard" Docker container, I was convinced that trying to run the Azure CLI in a container was more work than just falling back to a plain old Python virtual environment. With Distrobox, though, it's a piece of cake! Just create the Distrobox container, install the Azure CLI, export it, and you're done.

I hope you've found this article helpful. I encourage you to check out Distrobox, either via [the Distrobox web site][link-3] or via [the GitHub repository][link-4]. Feel free to find me online ([X/Twitter][link-7], [Mastodon][link-8], [Bluesky][link-9], or [LinkedIn][link-10]) and let me know what sort of cool things you end up doing with Distrobox!

[link-1]: https://github.com/Azure/azure-cli
[link-2]: https://podman.io/
[link-3]: https://distrobox.it
[link-4]: https://github.com/89luca89/distrobox
[link-5]: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux
[link-6]: https://starship.rs
[link-7]: https://twitter.com/scott_lowe
[link-8]: https://fosstodon.org/@scottslowe
[link-9]: https://bsky.app/profile/scottslowe.bsky.social
[link-10]: https://www.linkedin.com/in/scottslowe/
