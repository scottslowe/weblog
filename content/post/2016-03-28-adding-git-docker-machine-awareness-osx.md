---
author: slowe
categories: Tutorial
comments: true
date: 2016-03-28T00:00:00Z
tags:
- Git
- Docker
- CLI
- macOS
title: Adding Git and Docker Machine Awareness to OS X
url: /2016/03/28/adding-git-docker-machine-awareness-osx/
---

In this post I'm going to share how to add some [Git][link-1] and [Docker Machine][link-2] "awareness" to your OS X Bash prompt. This isn't anything new; these tricks are things that Bash users have been employing for years, especially on Linux. For most OS X users, though, I think these are tricks/tools that aren't particularly well-known so I wanted to share them here.

I'll divide this post into two sections:

1. Adding Git awareness to your Bash prompt
2. Adding Docker Machine awareness to your Bash prompt

Please note that I've only tested these on El Capitan (OS X 10.11), but it _should_ work similarly for most recent versions of OS X.

Before I get started, allow me to explain what I mean by "awareness":

* For Git, it's the ability to show the currently checked-out Git branch in your Bash prompt as well as tab completion for Git commands, branches, and remotes.
* For Docker Machine, it's the ability to show the currently-active machine (made active via `eval $(docker-machine env <name>)`) in your Bash prompt as well as tab completion for most Docker Machine commands and machines.

Ready? Let's get started!

## Adding Git Awareness to your Bash Prompt

To add some Git awareness to your Bash environment, you need two things:

* The Bash code to add the checked out Git branch in your prompt (obviously this will only be displayed when you are currently in a Git repository)
* The Bash code to add tab completion

If you install Git on your OS X system using the XCode command-line tools (see [this article for instructions][link-3]), then everything you need to add Git awareness to your Bash prompt is already installed on your system. You'll find them in `/Library/Developer/CommandLineTools/usr/share/git-core` as `git-prompt.sh` and `git-completion.bash`. Despite what a lot of other articles indicate, you do _not_ need to install special Bash completion packages in order to make this Git "awareness" work.

The `git-prompt.sh` file is what adds the checked out Git branch to your Bash prompt. When you're using the functionality in this file, your prompt will look something like this (this is my prompt on my system named "relentless"):

    relentless:learning-tools slowe (master)$

The `git-completion.bash` file is what enables tab completion for Git commands, branches, and remotes.

To leverage the functionality in these two files, you only need to edit the `.bashrc` in your home directory. Add these lines to your `.bashrc`:

``` bash
source /Library/Developer/CommandLineTools/usr/share/git-core/git-prompt.sh
source /Library/Developer/CommandLineTools/usr/share/git-core/git-completion.bash
```

Then, add `$(__git_ps1)` to your PS1 definition. For example, the PS1 for my prompt above looks like this:

    \h:\W \u$(__git_ps1)\$

Once you add this and restart your shell, then the name of the checked-out branch in Git will be added to the prompt whenever you're in a Git repository. Cool, eh?

## Adding Docker Machine Awareness to your Bash Prompt

This one is a bit trickier, mostly because the files you need don't come bundled with Docker Machine. (To be fair, they might get installed via Docker Toolbox, but as I don't use Toolbox I can't verify.)

The files you need are found in [the Docker Machine GitHub repository][link-4]. Specifically, you'll need the `docker-machine-prompt.bash` and `docker-machine.bash` files.

The `docker-machine-prompt.bash` file is---as you've probably guessed---the file that adds information to your Bash prompt. The `docker-machine.bash` file, on the other hand, is what enables tab completion for Docker Machine commands and machine names.

Like the Git awareness, all you need to do is source these files from your `.bashrc`, like this (obviously, you'll want to make sure the paths below are correct for your system---these paths are customized to my system):

``` bash
source /usr/local/docker/share/docker-machine-prompt.bash
source /usr/local/docker/share/docker-machine.bash
```

And then modify your PS1 definition to include `$(__docker_machine_ps1)`, like so (this definition includes both the Git and Docker Machine additions):

    \h:\W \u$(__git_prompt_ps1)$(__docker_machine_ps1)\$

After you've restarted your shell and you have an active Docker host, you'll see this in your prompt:

    relentless:ubuntu-wily slowe [docker-01]$

The functionality is cumulative, of course:

    relentless:docker-macvlan (master) [docker-01]$

I don't know about you, but I find this added functionality to be extraordinarily helpful. I hope you do too!

[link-1]: https://git-scm.com/
[link-2]: https://www.docker.com/products/docker-machine
[link-3]: http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/
[link-4]: https://github.com/docker/machine/tree/v0.6.0/contrib/completion/bash