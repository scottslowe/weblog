---
author: slowe
categories: Information
comments: true
date: 2018-08-21T23:00:00Z
tags:
- Linux
- CLI
- Kubernetes
title: A Simple Kubernetes Context Switcher
url: /2018/08/21/a-simple-kubernetes-context-switcher/
---

I recently needed to find a simple way of switching between [Kubernetes][link-2] contexts. I already use `powerline-go` (here's [the GitHub repo][link-1]), which allows me to display the Kubernetes context in the prompt so I always know which context is the active (current) context. However, switching between contexts using `kubectl config set-context <name>` isn't the easiest approach; not to mention it requires merging multiple config files into a single file (which is itself a bit of a task). So, I set out to create a simple Kubernetes context switcher---and here's the initial results of my efforts.<!--more-->

Before I go any further, I'd like to stress 2 important points. First, I'm not a programmer, so keep that in mind. Second, this is a _simple_ Kubernetes context switcher---it's not meant to address any and every possible use case out there, nor do I claim any sort of sophistication in the code.

With those disclaimers out of the way, allow me to introduce `kcs`: the simple Kubernetes context switcher. `kcs` is built on the idea that it's easiest to manage Kubernetes contexts in their own files, rather than trying to merge config files. So, it makes the assumption that you'll store your Kubernetes contexts in separate config files. To add a new context, just copy the config file into the `~/.kube` directory (the directory `kubectl` uses by default).

Once you have your config files in the `~/.kube` directory, then using `kcs` is a piece of cake:

* To select a particular context, just use `kcs <context-name>` (where `<context-name>` will equal a filename in the `~/.kube` directory). If you're using a prompt tool that shows the active Kubernetes context, you'll see the newly-selected context in your prompt.
* To list the available contexts, type `kcs list`. You'll get a space-delimited list of the available contexts (again, where a context is equivalent to a file in `~/.kube`).
* To specify that you don't want _any_ active context, specify `kcs none`.

Now, if you haven't figured it out yet, there's a **giant** problem with the admittedly very simplistic approach I've taken here---the name of the context _inside_ a config file may not match the name of the file itself, and it's the name of the context that most prompt tools (like powerline-go, for example) will use, not the filename itself. Clearly, this is a problem, and one that I hope to be able to programmatically address at some point, but for now the workaround is to simply edit the config file and make the context name match the filename.

`kcs` is written entirely in Bash, so it should run unmodified on most Linux distributions as well as macOS. (Maybe at some point as my Golang skills develop I'll re-code it in Golang.) If you'd like to give `kcs` a try, feel free to grab it from [my GitHub "scripts" repository][link-3]. As I mentioned before, I'm not a programmer (yet), so feel free to submit pull requests for improvements to `kcs` that might improve error checking, performance, portability, etc.

In the meantime, I hope this small (and quite simple) utility is helpful to folks.

**UPDATE:** Within days of publishing this post, someone pointed me to [`ktx`][link-4], which does pretty much the same thing (only better). I've completely abandoned `kcs` in favor of `ktx`.

[link-1]: https://github.com/justjanne/powerline-go
[link-2]: https://kubernetes.io/
[link-3]: https://github.com/scottslowe/scripts/
[link-4]: https://github.com/heptiolabs/ktx
