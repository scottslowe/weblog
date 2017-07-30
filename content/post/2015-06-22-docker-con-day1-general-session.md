---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-22T11:00:00Z
tags:
- Docker
- Linux
- CLI
- DockerCon2015
title: 'Liveblog: DockerCon 2015 Day 1 General Session'
url: /2015/06/22/docker-con-day1-general-session/
---

This is a liveblog for the day 1 general session at DockerCon 2015, taking place this week (today and tomorrow, anyway) at the Marriott Marquis in San Francisco, CA. This is my first DockerCon, and I'm looking forward to picking up lots of new knowledge.

The general session starts with a video (cartoon) about something working in development but not in production, and how Solomon Hykes came up with the idea for containers and Docker. It's a humorous, tongue-in-cheek production. As the video wraps up, Docker CEO Ben Golub takes the stage.

Golub starts with a personal story about the various startups for which he's worked, and the importance of his "two fold test" (that it has global significance and that it is easy to explain when you go home for Thanksgiving). Maybe the Thanksgiving test didn't quite make it, but Golub does think (naturally) that Docker has global significance. Golub says that Docker has become a fundamental part of how companies build, ship, and run distributed applications, and that Docker is a key part of how industries and cultures are being transformed. He attributes this success to the Docker community and the Docker ecosystem. Rightfully so, Golub credits the "giants" on whose shoulders Docker is standing---the companies who created namespaces, LXC tools, Cgroups, and the rest of the technologies that Docker leverages. Golub goes on to credit the Docker contributors, the community and supporters, the actual employees of Docker Inc., and---perhaps most importantly---the users of Docker.

The session now shifts to a "state of the project": 183% growth in contributors, 515% growth in Docker-related projects on GitHub, 1720% increase in Docker jobs, 934% increase in Dockerized applications (versus 1456% increase in downloads of Boot2Docker), and 18082% increase in container downloads (this refers to images pulled from Docker Hub).

This leads Golub to talk about more qualitative attributes about Docker---how its grown from an interesting project, to a solution, then to a platform and finally a movement. Golub reviews the "5 steps in the future of distributed applications":

1. Create a lightweight container.
2. Make container standard interoperable and easy to use.
3. Create an ecosystem.
4. Enable a multi-container model.
5. Create a platform for managing it all.

Golub introduced this 5-step future last year at DockerCon 2014 and reviews it now to see how much progress has been made. Golub feels that everything up through #3 has been done, and now it's time to start tackling #4 and #5. According to Golub, there's still more to come in #3, and Golub hints at what Solomon Hykes (Docker Inc. CTO) will talk about later in the general session regarding #4 and #5.

Ultimately, though, Golub feels that this year's DockerCon is all about Docker in production. Docker needs to work for everyone, and it needs to run everywhere, it has to be extensible and pluggable, and it needs to have real solutions and roadmaps for how to bring Docker to production.

Golub wraps up with how he feels Docker can "move the earth," and hands the stage over to Solomon Hykes, Founder and CTO of Docker, Inc.

Hykes starts by thanking the contributors and the community, and then transitions into establishing a "frame" for the discussion by explaining why they (Docker Inc.) is building what they're building. The mission of Docker (the company) is to build tools of mass innovation. Hykes believes that Docker can be the connector, the bridge, between new technology and the billions of people around the world who want to build new ideas but lack the tools to do so. Hykes believes that programming (software) is the biggest innovation multiplier in the world, and launches into a discussion of how making the Internet "programmable" would be the ultimate achievement. Standing in the way, of course, are software walled gardens---and eliminating those software walled gardens is the goal that Docker is attempting to achieve. In summary, Hykes believes that the next 5 years or so are going to be spent making the Internet programmable. That's the context, the frame, for everything that Docker is working on and building.

According to Hykes, there are 4 big goals in which everything fits:

1. Reinvent the programmer's toolbox.
2. Build better plumbing.
3. Promote open standards.
4. Help organizations solve real-world problems in unique ways.

Hykes believes that goal #1 is important because building distributed applications is too hard. Why? Because the tools are not adequate. The reason they aren't adequate is because the tools were designed before distributed applications. Improving the developer experience (building better tools) is important. The principle for reinventing the developer's toolbox is "incremental revolution", which has three aspects:

1. Choose one fundamental problem.
2. Solve it in the simplest possible way.
3. Repeat.

Incremental revolution, according to Hykes, results in a big change but takes care of it one step at a time. Everything they've been doing over the last couple of years is a reflection of this idea of incremental revolution. The container runtime (the Docker Engine) is one example. Images, Docker Compose,  Docker Machine, and Docker Swarm are all examples of tackling one small problem at a time via "incremental revolution."

OK---what's the next problem that will be solved via incremental revolution?

This leads Hykes to announce Docker experimental releases, a way for companies and developers to test the "bleeding edge" Docker releases before they are officially released.

Next up, Hykes talks about solving "the next problem"---which, in his mind, is networking. Hykes believes that machines (infrastructure) and the network should be part of the application, and should conform to what the application needs. The result of the joint efforts of Docker and Socketplane is an experimental feature (tying back to the previous release) called Docker Network, which will support native multi-host networking out of the box. Hykes says that micro-segmentation is built in to Docker Network, which allows people to assemble virtual networks into any topology. It's all built on industry standards. Docker Network leverages DNS for service discovery. And, to show that Docker Network is pluggable and extensible, Hykes points out the 11 community-contributed networking and service discovery plugins.

Hykes now shifts into a demo of Docker Network, and Hykes gives the stage over to Ben Firshman to show off Docker Network. Firshman shows off the use of the VMware Fusion driver for Docker Machine so that he can deploy some containers defined via Docker Compose to run/test the application on his local laptop. Now that the application has been tested locally, Firshman wants to deploy this application into production using Docker Swarm and Docker Network. He creates the Swarm cluster using Docker Machine, and again shows what the application definition looks like using Docker Compose. Next, Firshman runs "docker-compose scale" to increase the number of web containers to allow greater scale. (The live demo doesn't quite work as expected.) Alvin Richards, a software engineer at Docker, now takes the stage to continue the demo. Unfortunately, the demo fails with a couple of different errors, and Richards' backup video also doesn't work, so he has to manually walk through what the demo would have looked like if everything had worked.

Hykes takes the stage again, re-iterating Docker Network's experimental status. The next problem in reinventing the programmer's toolbox is making the toolbox extensible, i.e., how do I add my own tools to the toolbox?

This leads to the next announcement, which is Docker Plugins (also part of Docker's experimental releases at this time). This allows programmers to modify behaviors by adding their own plugins via extension points, and extension points exist for networking, volumes, schedulers, and service discovery. Hykes hints that more extension points are coming. Plugins don't require patching Docker, and Docker doesn't need to restart to load plugins. Plugins are also multi-tenant, which allow you to load multiple plugins at the same time and assign different plugins for different applications.

Next Hykes takes a quick moment to again thank the Docker ecosystem.

Continuing the ecosystem theme, Hykes gives the stage to Deepak Singh from Amazon Web Services and Amazon ECS. Singh talks a little bit about Amazon's history with Docker, such as adding a Docker-enabled AMI and adding Docker support to Beanstalk. All this led up to the introduction of EC2 Container Service (ECS). This leads to an announcement from Singh that later in the year you'll be able to use Docker Swarm and Docker Compose natively with Amazon ECS.

That wraps up the discussion of reinventing the developer's toolbox, and Hykes now transitions to the second big goal: building better plumbing. Hykes reinforces the importance of infrastructure plumbing, and he reviews the rules of software plumbing:

1. Re-use existing plumbing.
2. Make new plumbing easy to use and improve.
3. Follow the UNIX principles (small simple tools).
4. Define standard interfaces for assembling small tools into larger systems.

Hykes reinforces how much software plumbing is re-used in Docker (Linux, namespaces, cgroups, SELinux, AppArmor, layered file systems, tar, SSH, OpenSSL, etc.). Hykes repeats his appreciation and thanks for these projects and how important they are to Docker. Hykes also points out that 50% of Docker's source code is plumbing. This leads Hykes to announcement that all of Docker's plumbing is going to be spun out of the project and given back to the community for everyone else to use/re-use/improve. Spinning the plumbing out via the Docker Plumbing Project has had some results already.

Hykes announces Notary, which is a trusted publishing system for any content. Notary comes out of the Docker Plumbing Project, which is the effort to spin out all the Docker plumbing. Notary is platform-agnostic---distribute any content over any transport, using industry-leading cryptographic research. Hykes introduces Diogo Monica to do a demo of Notary.

Unlike the previous demo, the Notary demo works perfectly, and Diogo hands the stage back over to Hykes. The next piece of plumbing that will be spun out via the Docker Plumbing Project pertains to OS containers, and that is runC. runC is described as a universal runtime for OS containers (more information at http://runc.io). runC supports all the security features of Linux (SELinux, AppArmor, cgroups, namespaces, seccomp, cap-drop, etc.), supports user namespaces, and live migration (via CRIU). Microsoft is contributing Windows support to runC. ARM support is underway. Intel is contributing DPDK and Secure Enclave. runC defines a standard, portable, runnable format, and it is usable from the command line or programmatically. The tagline that Hykes uses is "Just the runtime and nothing else."

At this point, Hykes shifts to big goal #3 (promote open standards). Hykes says that the real value of Docker isn't technology, it's getting people to agree on something---in this case, the container runtime. Hykes talks about how Docker has the responsibility for taking their _de facto_ standard and turn it into a proper standard. What is a proper standard? A proper standard needs:

1. A formal specification
2. Independent governance
3. A neutral reference implementation
4. Support from a broad coalition
5. An open door to fresh ideas

Hykes introduces OCF (Open Container Format?), a universal intermediary format for OS containers. OCF is compared to ELF, but for containers. runC is considered a perpetual commitment to supporting OCF. To provide independent governance, Docker is partnering with the Linux Foundation to create the Open Container Project, which will govern OCF. To provide the neutral reference implementation, Docker is donating all of runC to the Open Container Project. To demonstrate the broad support, Hykes reads off a list of vendors that are on board with OCF/OCP: AWS, Apcera, Cisco, CoreOS, EMC, Fujitsu, Goldman Sachs, Google, HP, Huawei, IBM, Intel, Joyent, Linux Foundation, Microsoft, Pivotal, Rancher, Red Hat, and VMware. To demonstrate an open door to fresh ideas, all of the appc maintainers are joining the Open Container Project as founding members. Hykes publicly calls out CoreOS and thanks them for their contributions to the community.

Hykes wraps up his talk with revisiting the 4 big goals, and talks about how the fourth goal (solving real-world problems), and provides a preview of how tomorrow's general session will talk about this big goal in more detail. And all this ties together to enable to people to build crazy stuff via tools of mass innovation.

At this point, Hykes wraps up the general session.