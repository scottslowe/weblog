---
author: slowe
categories: Information
comments: true
date: 2022-05-27T16:00:00-06:00
tags:
- CLI
- Fedora
- Linux
- Git
title: Git Difftool and Meld as a Flatpak
url: /2022/05/27/git-difftool-and-meld-as-a-flatpak/
---

I've recently started migrating many of the applications on my [Fedora][link-1] 36 laptop to their [Flatpak][link-2] versions. For the most part, this has been pretty straightforward, although there isn't really any method for migrating configuration and data. Today I ran into a problem with [Meld][link-3], a graphical diff utility, and using it with the `git difftool` command. Below I'll share how I worked around this problem.<!--more-->

Normally, the integration between Git and Meld---which is what enables you to run `git difftool` and have the results show up in Meld---would look something like this (this is from `~/.gitconfig`):

```toml
[merge]
    tool = meld
[diff]
    tool = meld
[difftool]
    prompt = no
[difftool "meld"]
    cmd = /usr/bin/meld "$LOCAL" "$REMOTE"
[mergetool "meld"]
    cmd = /usr/bin/meld "$LOCAL" "$REMOTE"
```

However, when Meld is installed as a Flatpak, `/usr/bin/meld` doesn't exist. In order to continue using Meld with the `git difftool` command, you must change the Git configuration to look like this instead:

```toml
[merge]
    tool = meld
[diff]
    tool = meld
[difftool]
    prompt = no
[difftool "meld"]
    cmd = flatpak run org.gnome.Meld "$LOCAL" "$REMOTE"
[mergetool "meld"]
    cmd = flatpak run org.gnome.Meld "$LOCAL" "$REMOTE"
```

After you make these changes, using Meld with `git difftool` should work as expected again.

This is a pretty straightforward change, but hopefully documenting it here will prove helpful to someone. If you have any questions, feel free to contact [me on Twitter][link-4]. Thanks!

[link-1]: https://getfedora.org/
[link-2]: https://www.flatpak.org/
[link-3]: https://meldmerge.org/
[link-4]: https://twitter.com/scott_lowe
