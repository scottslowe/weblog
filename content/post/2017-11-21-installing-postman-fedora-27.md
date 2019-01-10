---
author: slowe
categories: Tutorial
comments: true
date: 2017-11-21T19:00:00Z
tags:
- Linux
- Fedora
- CLI
title: Installing Postman on Fedora 27
url: /2017/11/21/installing-postman-fedora-27/
---

I recently had a need to install the Postman native app on [Fedora][link-4] 27. The Postman site itself only provides [a link to the download][link-2] and [a rather generic set of instructions][link-3] for installing the Postman native app (a link to [these instructions for Ubuntu 16.04][link-1] is also provided). There were not, however, any directions for Fedora. Hence, I'm posting the steps I took to set up the Postman native app on my Fedora 27 laptop.<!--more-->

(Note that these instructions will probably work with other versions of Fedora as well, but I've only used them on Fedora 27.)

Here are the steps I followed:

1. Download the installation tarball, either via your browser of choice or via the command line. If you'd prefer to use the command line, this command should take care of you:

        curl -L https://www.getpostman.com/app/download/linux64 -O postman-linux-x64.tar.gz

2. Unpack the tarball into the directory of your choice. I prefer to put third-party applications such as this into the `/opt` directory; you can (obviously) put it wherever you prefer. This command should do the trick:

        sudo tar xvzf postman-linux-x64.tar.gz -C /opt

    If you prefer a directory _other_ than `/opt`, specify the appropriate directory in the command above.

3. In my particular case, `tar` created a directory with an uppercase character (`/opt/Postman`) and some odd permissions (a strange user and group for ownership). I fixed those with `mv` and `chown`, respectively. You may or may not need to do anything.

4. Create a symbolic link in a directory included in your PATH. In this example, I'm creating the symbolic link in `/usr/local/bin`, but you could use any directory included in your PATH:

        sudo ln -s /opt/postman/Postman /usr/local/bin/postman

5. At this point, you should be able to launch Postman by just running `postman` from the terminal. However, ideally you'll want to be able to use a graphical launcher. To do that, you need to create a "desktop launcher." Create a file named `postman.desktop` in `~/.local/share/applications` with these contents:

        [Desktop Entry]
        Name=Postman
        GenericName=API Client
        X-GNOME-FullName=Postman API Client
        Comment=Make and view REST API calls and responses
        Keywords=api;
        Exec=/opt/bin/postman
        Terminal=false
        Type=Application
        Icon=/opt/postman/resources/app/assets/icon.png
        Categories=Development;Utilities;

6. Log out and log back in, and after a few minutes you should be able to see a Postman icon in your list of applications. You can now launch Postman either by using the graphical launcher _or_ by running `postman` in the terminal.

Enjoy!

**UPDATE:** Newer versions of Postman apparently change some of the paths inside the tarball. Check out [this post on installing Postman 6.7.1 on Fedora 29][link-5] for some updated information. Credit to Jose Barahona for the post. Thanks Jose!

[link-1]: https://blog.bluematador.com/posts/postman-how-to-install-on-ubuntu-1604/
[link-2]: https://www.getpostman.com/app/download/linux64
[link-3]: https://www.getpostman.com/docs/postman/launching_postman/installation_and_updates
[link-4]: https://getfedora.org/
[link-5]: https://jrab66.com/2019/01/10/installing-postman-on-fedora-29/
