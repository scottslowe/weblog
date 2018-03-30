---
author: slowe
categories: News
comments: true
date: 2016-03-24T00:00:00Z
tags:
- Linux
- Docker
- CLI
- Networking
title: Learning Tools Updates
url: /2016/03/24/learning-tools-updates/
---

One of the projects that I started last year was [my GitHub "learning-tools" repository][link-1], in which I store tools (of various sorts) to help with learning new technologies. Many of these tools are [Vagrant][link-2] environments, but some are sample templates for other tools like [Terraform][link-3]. I recently made some updates to a couple of the tools in this repo, so I wanted to briefly update my readers.

## Docker with IPVLAN L2 Interfaces

This area of the repository was already present, but I had a note in the repo's main `README.md` noting that it wasn't fully functional. After having to work through some other issues (issues that resulted in [this blog post][xref-1]), I was finally able to create the tools and assets to make this environment easily repeatable. So, if you'd like to work with Docker using IPVLAN interfaces in L2 mode, then have a look [in the `docker-ipvlan` folder][link-4] of the repository. The folder-specific `README.md` is pretty self-explanatory, but if you run into any problems or issues feel free to open a GitHub issue.

## Docker with IPVLAN L3 Interfaces

This is an entirely new area of the repo. Thanks in part to being able to complete the IPVLAN L2 environment described above, I was able to relatively quickly add an IPVLAN L3 environment (look [in the `docker-ipvlan-l3` folder][link-5]). This environment enables you to relatively easily spin up a Docker environment using IPVLAN L3 interfaces. (Side note: IPVLAN L3 is _very_ cool.) Have a look at the folder-specific `README.md` for more details, and if you have problems or questions please open a GitHub issue so that I can help you.

## Future Work

I have plans to add a great deal more content here throughout this year, as I continue to refine my expertise in specific areas based on [my 2016 project list][xref-2]. If you have specific things you'd like to see, a GitHub issue is probably the best way to make that request and enable me to track it.



[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: http://www.vagrantup.com
[link-3]: http://www.terraform.io
[link-4]: https://github.com/scottslowe/learning-tools/tree/master/docker-ipvlan
[link-5]: https://github.com/scottslowe/learning-tools/tree/master/docker-ipvlan-l3
[xref-1]: {{< relref "2016-03-21-vagrant-ubuntu-wily-networking-problem.md" >}}
[xref-2]: {{< relref "2016-01-21-looking-ahead-2016-projects.md" >}}
