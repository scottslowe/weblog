---
author: slowe
categories: Tutorial
comments: true
date: 2016-11-21T00:00:00Z
tags:
- Linux
- Git
- CLI
- Ubuntu
title: Using GNOME Keyring as Git Credential Helper
url: /2016/11/21/gnome-keyring-git-credential-helper/
---

In this post, I'm going to show you how to use the [GNOME Keyring][link-2] on Ubuntu 16.04 as a credential helper for [Git][link-1]. This post stems from my work in transitioning to Linux as my primary OS, an effort I've ratcheted up significantly in the last few weeks. What I'm including here isn't new or ground-breaking information; I'm posting it primarily to make the information easier to find for others.

On Ubuntu 16.04, the basis for integrating GNOME Keyring into Git as a credential helper is already installed into the `/usr/share/doc/git/contrib/credential/gnome-keyring` directory. However, if you try to simply run `sudo make` in that directory, it will fail. In order to make it work, you must first install some additional development libraries:

    sudo apt install libgnome-keyring-dev

Once you've installed this additional package, running `sudo make` in that directory will quickly compile a binary named `git-credential-gnome-keyring`. Once you have that binary, then you can configure Git to use GNOME Keyring as a credential helper. You can do this a couple of different ways:

1. You can use the `git config` command, like this:

        git config --global credential.helper /usr/share/doc/git/contrib/credential/gnome-keyring/git-credential-gnome-keyring

2. You can edit `~/.gitconfig` directly, using the text editor of your choice. Add this text:

        [credential]
        helper = /usr/share/doc/git/contrib/credential/gnome-keyring/git-credential-gnome-keyring

Once you add this directive to your Git configuration, then the next time you need to supply credentials, Git will store them in the GNOME Keyring automatically. On Ubuntu 16.04, you can verify this using [Seahorse][link-3], the GUI front-end to the GNOME Keyring.



[link-1]: https://git-scm.com/
[link-2]: https://wiki.gnome.org/action/show/Projects/GnomeKeyring
[link-3]: https://wiki.gnome.org/Apps/Seahorse
[link-4]: https://www.ubuntu.com/
