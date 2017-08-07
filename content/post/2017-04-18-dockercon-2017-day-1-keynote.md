---
author: slowe
categories: Liveblog
comments: true
date: 2017-04-18T00:00:00Z
tags:
- Docker
- DockerCon2017
- Linux
- Windows
- Microsoft
title: 'Liveblog: DockerCon 2017 Day 1 Keynote'
url: /2017/04/18/dockercon-2017-day-1-keynote/
---

This is a liveblog of the day 1 keynote (general session) of DockerCon 2017 in Austin, TX.<!--more-->

At 9:05am, Ben Golub, CEO of Docker, Inc., takes the stage to kick off the general session and the conference. Golub starts the presentation by reviewing Docker's four-year history and all the things that have changed over the last three years since the very first DockerCon---from the size of Gordon (Docker's tortoise mascot) to the amount of growth in Docker usage (via statistics in the number of Docker hosts, the number of Docker-ized apps, the number of image pulls from Docker Hub, and so forth). 

Golub continues by mentioning some of the various use cases for Docker. One use case mentioned is Intuit's use of Docker, and Golub points out that the person responsible for running Intuit's systems is confident enough in their systems that they're attending DockerCon on Tax Day (when as many as 25 million tax returns are expected to be processed).

Shifting gears a bit, Golub talks a bit more about the changes over the last 3 years in regards to Docker (the open source project) itself. Stakeholders have changed, and the nature of the project (now projects) has shifted as well. Golub mentions that this is something that will be discussed in more detail shortly when Solomon Hykes takes the stage.

After briefly discussing a specific talk here at the conference (a talk by TGen), Golub thanks the contributors, maintainers, mentors, and community members who support Docker (the open source project). He also thanks the sponsors of the event, the speakers (and especially customer speakers), the members of the ecosystem, and the Docker Inc. team.

After a "sacrifice to the demo gods," Golub brings [Solomon Hykes][link-1], CTO and founder of Docker, Inc., to the stage.

Hykes kicks off his portion of the keynote by re-iterating Docker's mission: to build tools of mass innovation. As a result of a lot of thinking about tools, Hykes says they've derived some rules for building the "best tools" for mass innovation. These rules state that the best tools should:

1. Get out of the way
2. Adapt to you
3. Make the powerful simple

Hykes says these rules are the impetus behind the separation between Enterprise Edition and Community Edition, and why there are tools like Docker for Mac or Docker for Windows.

Turning to focus a bit more on the tools themselves, Hykes starts with tools for developers (arguably Docker's bread-and-butter audience). Docker, according to Hykes, strives to streamline and remove friction in the development cycle. How? Hykes says he uses "complaint-driven development." What is complaint-driven development? It's a three-step process:

1. Developer complains about problem.
2. Docker fixes detail complained about in step 1.
3. Repeat _ad infinitum_.

Hykes goes on to provide some examples of this process. The first example he mentioned is that container images are too big. The answer to this process is multi-state builds. Multi-stage builds---leveraging a single `Dockerfile` and a single build command---allow you to cleanly separate build-time environments/images from run-time environments/images.

The second example Hykes shares is the friction involved in moving apps from the desktop (using something like Docker for Mac) to the cloud (using something like Docker for AWS). The fix for this problem is called desktop-to-cloud, which allows you to connect to cloud-based swarms directly from the desktop UI (in Docker for Mac or Docker for Windows).

At this point, Hykes brings out the first demo team (Ryan Adams and Kristie Howard). The demo shows off multi-stage builds and the desktop-to-cloud integration from both Docker for Mac and Docker for Windows. The skit and dialogue were a bit contrived (as they often are), but the demo did a good job of showing off the new functionality.

Hykes re-takes the stage, and informs the audience that you can get the new features available _right now_ as part of Docker's Edge release. It will hit the stable release soon.

The focus now shifts from Docker for developers to Docker for operators. The first aspect that Hykes discusses is security, and how moving to production securely is difficult. Why? Part of this is the shift to distributed systems. Hykes says that you can't just continue to use the same tools for distributed systems; distributed systems are more than the sum of their parts, and therefore you need something more to secure a distributed system. According to Hykes, secure orchestration is an answer to securing distributed systems; this leads into a discussion of secure orchestration.

The core of secure orchestration within Docker is SwarmKit. To talk more about secure orchestration and SwarmKit, Hykes brings out Diogo Monica, the technical security lead at Docker Inc. Monica begins his discussion of secure orchestration with a review of Secure Node Introduction (which is how a node joins a swarm). This leads to a discussion of cryptographic node identity via a public key infrastructure (PKI) created automatically by SwarmKit. With the PKI and cryptographic node identity in place, this enables mutual TLS (MTLS) between all nodes (for control plane interaction). These identities can also be used for cluster segmentation, according to Monica. SwarmKit also offers encrypted networks (I was not aware of this functionality), and secure secret distribution in a platform-agnostic, least-privilege way.

Monica turns the stage back over to Hykes. Hykes talks about the second challenge to moving into production securely: the diversity of infrastructure and operating systems (OSes) underneath Docker. Hykes says that this causes teams to choose between security or portability. The answer? An orchestration layer that is not only secure, but also portable. The third challenge to moving into production securely is developer choice. Lots of different frameworks, lots of different applications, lots of different databases...all this freedom of choice creates challenges to security. You _could_ restrict developer choice, but Hykes believes that's not the answer. The answer is an orchestration system that is secure, portable, and usable.

Naturally, this leads to a second demo. Hykes brings out Kristie Howard, along with Nathan LeClaire and Diogo Monica. In a classic "Not my problem" scenario, Howard drops the new service to be deployed into the hands of the Ops team (LeClaire and Monica), who then run through the rest of the demo.

Retaking the stage, Hykes points out that a platform is only as secure as its weakest component. This means that you have to worry about every component, and according to Hykes that's exactly what Docker Inc. has been doing for the last year or so. To put this into context, Hykes discusses the work that Docker Inc. has been doing on the various "Docker for X" platforms. This led Docker to work with a number of other companies to build a secure, lean, portable Linux subsystem. The result of this effort is LinuxKit.

[LinuxKit][link-2] is a lean, portable, and secure Linux subsystem. It works _only_ with containers, which allows LinuxKit to sandbox system services and reduce the attack surface. This, in turn, provides an incubator for innovative new security solutions. LinuxKit was built using a community-first security process. Linux is too big (and too important, according to Hykes) for any one company to secure it; it has to be done via a community effort with other organizations and contributors. LinuxKit naturally has to be very lean, and it has to be portable. Other companies involved in LinuxKit include IBM, Microsoft, Intel, Hewlett Packard Enterprise, and the Linux Foundation.

Hykes now brings out John Gossman, Azure Architect from Microsoft, to take the stage. I expected Gossman to talk about LinuxKit on Windows, but the discussion was more about Hyper-V Isolation (adding hardware virtualization isolation for containers on Windows---for VMware folks, it's analogous to vSphere Integrated Containers). Gossman announces today that Microsoft is bringing Hyper-V Isolation to Linux containers running on Windows (previously Hyper-V Isolation supported only Windows-based containers). Gossman points attendees to a session later in the day where Microsoft will discuss Hyper-V Isolation in more details. Hyper-V Isolation will support LinuxKit at a future point.

Hykes re-takes the stage, and open sources LinuxKit live on the stage. After a bit of "technical difficulty" regarding his two-factor authentication to GitHub, Hykes finally gets into the LinuxKit repository and makes it public.

Shifting gears slightly, Hykes starts talking a bit more about the container ecosystem and Docker Inc.'s commitment to the container ecosystem. Docker believes that if the container ecosystem succeeds, Docker succeeds. (Docker the project or Docker the company?) This leads to a discussion of the separation of runC, containerD, SwarmKit, InfraKit, etc., into separate component projects. According to Hykes, the spinning out of component projects reflects a natural evolution of Docker Inc.'s participation in the container ecosystem as the ecosystem has grown over the last few years.

The sum of the discussion around the growth of the ecosystem and the evolution of Docker Inc.'s collaboration around component tools _and_ the common assemblies used to put these components together is moving the discussion around the assemblies into open source themselves. The result is the Moby project.

The Moby project is a framework to let you assemble specialized container systems without reinventing the wheel. It's a library of 80+ components that you can use and re-use with your own assemblies. The project has reference assemblies that you can use unmodified, or that you can build upon or customize to create your own custom container systems. Hykes vows that Docker Inc. will use Moby for all of its own open source development. Moby will will be community-run and will leverage a governance model inspired by other successful projects (like Fedora). Moby is designed to work well with existing projects; there's no requirement to donate code to Moby.

So what does Moby mean to you? According to Hykes:

* If you're a _Docker user_: Docker will better leverage the ecosystem to innovate faster for you
* If you're a _system build_: Moby will help you innovate without tying you to Docker (the project or the company?)

Hykes walks through a few examples of using Moby assemblies:

* Building a locked-down Linux system with remote attestation
* Leveraging this locked-down Linux system to create a custom CI/CD stack
* Rebuilding the custom CI/CD stack using Debian instead of LinuxKit and Terraform instead of InfraKit

Following up on this discussion, Hykes brings out Rolf Neugebauer to help him demo Moby. The first demo involves building "RedisOS," which is a minimal OS that only runs Redis, and booting "RedisOS" on a Mac, on Windows, and on bare metal. It looks like Moby uses a YAML file to define the components in a Moby assembly. Moby sort of feels a bit like HashiCorp's Packer tool in that it can produce "artifacts"---VM images, ISO images, etc.---just by modifying the Moby assembly. The second example in the demo builds an etcd cluster on Google Cloud, including the use of Prometheus to monitor the cluster. The third and final example is turning up Kubernetes using HyperKit, LinuxKit, containerD, etcd, SSH, and Kubernetes on a Mac.

With the end of the demo, Hykes closes out the general session.



[link-1]: https://twitter.com/solomonstre
[link-2]: https://github.com/linuxkit/linuxkit
