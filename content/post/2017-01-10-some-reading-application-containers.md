---
author: slowe
categories: Information
comments: true
date: 2017-01-10T00:00:00Z
tags:
- Linux
- Docker
- Security
title: Some Reading on Application Containers
url: /2017/01/10/some-reading-application-containers/
---

One aspect of my pending migration to [Ubuntu Linux][link-6] on my primary laptop has been the opportunity to explore "non-traditional" uses for Linux containers. In particular, the idea of using [Docker][link-7] (or `systemd-nspawn` or `rkt`) to serve as a sandbox (of sorts) for GUI applications really intrigues me. This isn't a use case that many of the container mechanisms are aiming to solve, but it's an interesting use case nevertheless (to me, anyway).

So, in no particular order, here are a few articles I found about using Linux containers as application containers/sandboxes (mostly focused around GUI applications):

[A Docker-Like Container Management using systemd][link-1]  
[Running containers without Docker][link-2]  
[Containerizing Graphical Applications on Linux with systemd-nspawn][link-3]  
[Debian Containers with systemd-nspawn][link-4]  
[Using your own containers with systemd-nspawn and overlayfs][link-5]  
[Using systemd-nspawn for some containerization needs][link-9]

I was successful in using Docker to containerize Firefox (see [my "dockerfiles" repository on GitHub][link-8]), and was also successful in using `systemd-nspawn` in the same way, including the use of overlayfs. My experiments have been quite helpful and informative; I have some ideas that may percolate into future blog posts.



[link-1]: http://blog.exppad.com/article/a-docker-like-container-management-using-systemd
[link-2]: http://jvns.ca/blog/2016/10/26/running-container-without-docker/
[link-3]: https://ramsdenj.com/2016/09/23/containerizing-graphical-applications-on-linux-with-systemd-nspawn.html
[link-4]: https://lindenberg.io/blog/post/debian-containers-with-systemd-nspawn/
[link-5]: https://insecure.ws/linux/systemd_nspawn.html#using-your-own-containers-with-systemd-nspawn-overlayfs
[link-6]: https://www.ubuntu.com/
[link-7]: https://www.docker.com/
[link-8]: https://github.com/scottslowe/dockerfiles
[link-9]: http://blog.fntlnz.wtf/post/systemd-nspawn/
