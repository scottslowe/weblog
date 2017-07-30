---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-22T15:20:00Z
tags:
- Docker
- Networking
- Storage
- DockerCon2015
title: 'Liveblog: Secret Session (Docker Plugins)'
url: /2015/06/22/top-secret-session-docker-plugins/
---

This is the "Top Secret Docker Session led by Gordon the Turtle," which is really a session on Docker Plugins. However, since Docker Plugins were only announced this morning during the general session, the title for this session had to be obscured. On stage are ClusterHQ (Luke Marsden), Glider Labs (Jeff Lindsay), and Weaveworks (Alexis Richardson).

Marsden starts the session with a brief history of the Docker Plugins project, and how it grew out of Powerstrip. Marsden reiterates that he said Powerstrip would be successful if they would "throw it away" in 6 months. Four months later, the Docker Plugins project is now officially announced, and Powerstrip is no longer necessary.

Marsden next turns the stage over to Jeff Lindsay. Lindsay talks about why the Docker Plugins project is so important---every customer is unique, and customers want/need the freedom to choose the right solution to use the tools that best solve their particular problem(s).

Jeff Lindsay turns it over to Alexis Richardson, who outlines the core requirements for Docker Plugins. Richardson outlines 3 requirements, but he doesn't have a slide that lists those requirements, so I couldn't capture them. Plugins today are limited to storage and networking, but that isn't the limit---in the future, plugins can be made available for Swarm scheduling, or for logging.

Marsden takes back over to explain what a plugin is. Plugins are chunks of new functionality that can run anywhere Docker can run. Users install the plugin and access the functionality via the regular Docker interface (this preserves the developer UX with Docker). Of course, the key point of plugins is that they enable new functionality that would not otherwise have been available (of course). This leads Marsden into a quick demo of Docker Plugins using the Flocker and Weave plugins, along with Docker Swarm and Docker Compose, and showing a container moving between container hosts. Fortunately, Murphy's Law (which has plagued so many of the demos and booths here) stays away, and Marsden's demo works as expected.

Richardson takes over and talks about how managing stage is an important part of making Docker "enterprise-ready". This leads into a sales pitch for Weave 1.0, which is followed by Marsden providing a sales pitch for Flocker 1.0 and Lindsay providing a sales pitch for Glider Labs.

Fortunately, the sales pitch ends relatively quickly, and Lindsay starts providing more technical details on how to write a Docker plugin. Plugins can be distributed as containers; they just have to listen on a UNIX socket that is also accessible to the Docker daemon. Richardson takes over from Lindsay to recap the benefits of Docker Plugins, and Marsden steps up to wrap up the session.
