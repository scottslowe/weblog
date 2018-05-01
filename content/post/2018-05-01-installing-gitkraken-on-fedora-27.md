---
author: slowe
categories: Tutorial
comments: true
date: 2018-05-01T12:00:00Z
tags:
- Fedora
- Linux
- Git
title: Installing GitKraken on Fedora 27
url: /2018/05/01/installing-gitkraken-on-fedora-27/
---

[GitKraken][link-4] is a full-featured graphical Git client with support for multiple platforms. Given that I'm trying to live a multi-platform life, it made sense for me to give this a try and see whether it is worth making part of [my (evolving and updated) multi-platform toolbelt][xref-1]. Along the way, though, I found that GitKraken doesn't provide an RPM package for [Fedora][link-3], and that the installation isn't as straightforward as one might hope. I'm documenting the procedure here in the hope of helping others.<!--more-->

First, download the latest release of GitKraken. You can do this via the terminal with this command:

    curl -LO https://release.gitkraken.com/linux/gitkraken-amd64.tar.gz

Extract the contents of the GitKraken download into its own directory under `/opt` using this command (you can use a different directory if you like, but I prefer to install third-party applications like this under `/opt`):

    sudo tar -C /opt -xvf gitkraken-amd64.tar.gz

This will extract everything into `/opt/gitkraken`.

Next, you'll create a symbolic link to an existing library to fix an error with GitKraken when running on Fedora (this is documented here):

    sudo ln -s /usr/lib64/libcurl.so.4 /usr/lib64/libcurl-gnutls.so.4

Once this is done, you _could_ just run `/opt/gitkraken/gitkraken` from the Terminal and GitKraken would fire up.

However, you may find it easier to create a desktop launcher for GitKraken. Before you do that, download an icon for the app (there is no icon file distributed in the download). One suggested icon can be found [here][link-2]; just copy that image into `/opt/gitkraken` and make note of the filename you use (for the purposes of this article, I'll assume it's named `icon.png`. Yes, I know, I'm very creative).

Now create the file `/usr/share/applications/gitkraken.desktop` and put these contents into the file:

``` text
[Desktop Entry]
Name=GitKraken
Comment=Graphical Git client
Exec=/opt/gitkraken/gitkraken
Icon=/opt/gitkraken/icon.png
Terminal=false
Type=Application
Encoding=UTF-8
Categories=Utility;Development;
```

If you named the icon you downloaded something _other_ than `icon.png`, be sure to adjust the "Icon=" line in the file accordingly. Save that file and within a few moments you should have a GitKraken desktop launcher ready to roll.

A couple additional notes of which to be aware:

* GitKraken requires that you create a GitKraken account or link GitKraken to your GitHub account. This is an annoyance, but fortunately a minor one.
* I noted that GitKraken has some issues connecting to GitHub, GitLab, and other online services with a very generic and non-descriptive error. I haven't yet found a workaround for the issue.

## Credit

The source for the `gitkraken.desktop`, the suggested icon, and other useful information was taken from [this GitHub Gist][link-1].

[link-1]: https://gist.github.com/aelkz/17528d2f6a5db73185c7dfbd28e49d18
[link-2]: http://img.informer.com/icons_mac/png/128/422/422255.png
[link-3]: https://getfedora.org/
[link-4]: https://www.gitkraken.com/
[xref-1]: {{< relref "2018-04-30-updated-look-at-multi-platform-toolbelt.md" >}}
