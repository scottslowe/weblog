---
author: slowe
categories: News
comments: true
date: 2014-06-09T10:31:00Z
slug: docker-making-its-move
tags:
- Docker
- Linux
- LXC
- Virtualization
title: Docker Making Its Move
url: /2014/06/09/docker-making-its-move/
wordpress_id: 3462
---

Today marked the start of [Dockercon](http://dockercon.com), and Docker, Inc., the commercial entity behind the incredibly popular open source Docker project, is taking this opportunity to make its move. Already the focus of quite a bit of attention (some might say a disproportionate amount of attention), Docker is making several announcements today that indicate an intent to broaden its reach:

* Docker (also referred to as Docker Engine) 1.0 is released today, signaling the project's readiness to be officially used in production environments (although many firms were/are already using pre-1.0 releases in production)

* Docker is introducing a new Enterprise Support program to give enterprises the warm cuddlies that using Docker in production environments is safe. The new program aims to provide (in Docker's words) "training, expertise, and support necessary to stand-up mission-critical workloads built on the Docker platform."

* Docker is announcing a set of ten systems integrator partners to help drive commercial adoption.

* Docker is also unveiling Docker Hub, a cloud-based service (naturally!) for users, content, and workflows. Docker Hub will offer a registry for a comprehensive list of "Dockerized" applications (taking the place of the Docker Index, I assume), along with a management console, user authentication, an automated build service, the Docker Hub API, and collaboration tools to help users manage and share applications.

* A key part of Docker Hub is Official Repositories; these are "Dockerized" applications that are maintained, supported, and available to all Docker Hub users. This will initially include applications like MongoDB, MySQL, Nginx, Redis, and WordPress, but it is open to any community group or ISV. ()Obviously, there are some requirements around committing to maintenance of the application on an ongoing basis.)

I'm certainly a fan of using Docker (or other container technologies) where it makes sense. I fear, though, that the intense focus (some might say hype) around Docker will lead more than a few organizations down a path of trying to make Docker containers their "be all end all" solution. If that trend grows too large, that could be as damaging (if not more damaging) to Docker than being ignored. Time will tell. In the meantime, I'd strongly recommend getting a grasp on the basics of Docker so that you can better understand how it might fit into your overall solution. (My [introductory post on Docker][1], while a bit dated, might prove useful in that regard.)

[1]: {{< relref "2014-03-11-a-quick-introduction-to-docker.md" >}}
