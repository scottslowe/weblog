---
author: slowe
categories: Tutorial
comments: true
date: 2016-11-23T00:00:00Z
tags:
- Linux
- Git
- CLI
- Fedora
title: Using GNOME Keyring for Git Credentials on Fedora 25
url: /2016/11/23/gnome-keyring-git-credential-fedora/
---

In this post, I'm going to show you how to use the [GNOME Keyring][link-2] on [Fedora 25][link-4] as a credential helper for [Git][link-1]. This post is very closely related to my earlier post on [using GNOME Keyring as a Git credential helper on Ubuntu 16.04][xref-1]. As with the earlier Ubuntu-related post, what I'm including here isn't new or ground-breaking information; I'm posting it primarily to make the information easier to find for others.

Like Ubuntu 16.04, Fedora 25 already has the basis for integrating GNOME Keyring into Git as a credential helper already installed into the `/usr/share/doc/git-core-doc/contrib/credential/gnome-keyring` directory.

Unlike Ubuntu 16.04, though, Fedora already has a compiled credential helper installed. This Git credential helper is found at `/usr/libexec/git-core/git-credential-gnome-keyring`. This credential helper is ready to use.

To get GNOME Keyring support for storing Git credentials, then, all one has to do is simply configure Git appropriately (no need to install additional packages or compile anything). You can configure Git via a couple of different ways:

1. You can use the `git config` command, like this:

        git config --global credential.helper /usr/libexec/git-core/git-credential-gnome-keyring

2. You can edit `~/.gitconfig` directly, using the text editor of your choice. Add this text:

        [credential]
        helper = /usr/libexec/git-core/git-credential-gnome-keyring

Once you add this directive to your Git configuration, then the next time you need to supply credentials, Git will store them in the GNOME Keyring automatically. You can verify this using [Seahorse][link-3], the GUI front-end to the GNOME Keyring.



[link-1]: https://git-scm.com/
[link-2]: https://wiki.gnome.org/action/show/Projects/GnomeKeyring
[link-3]: https://wiki.gnome.org/Apps/Seahorse
[link-4]: https://www.getfedora.com/
[xref-1]: {{< relref "2016-11-21-gnome-keyring-git-credential-helper.md" >}}
