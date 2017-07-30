---
author: slowe
categories: Liveblog
comments: true
date: 2015-06-23T10:45:00Z
tags:
- Docker
- DockerCon2015
title: 'Liveblog: DockerCon 2015 Day 2 General Session'
url: /2015/06/23/dockercon-day2-general-session/
---

This is a liveblog from the day 2 general session at DockerCon 2015. I was running late from some early morning meetings (sorry folks), so I wasn't able to catch the first part of the general session (about the first 15 minutes or so). Here's what I was able to capture.

Chris Buckley, Director of DevOps at Business Insider, took the stage to provide an overview of how Business Insider (BI) started using Docker. Buckley provides some "lessons learned":

1. Building for production first was grandiose was misguided.
2. Porting it back to development wasn't the right approach.

This led BI to Fig (now Docker Compose), which led to a decrease in the time it took to get a development environment up and running. With the combination of Vagrant and Docker, BI was able to reduce that to just a couple of hours. When BI revisited production apps, they turned to use Upstart/SysV scripts for containers, but this wasn't quite the right fit. BI turned back to Puppet, building a parameterized Puppet class to create containers, links, set environment options, and define dependencies on other containers/services starting first.

Before Docker, the workflow was developers to GitHub to Jenkins, which then pushed to AWS in production. Operations pushed to GitHub, which pushed to Puppet and then moved to AWS in production. Adding Docker to this mix now allows the developer and operations streams to cross/mix as needed.

What's the future for BI with Docker?

* Exploring lots of orchestration options (Machine/Swarm/Compose, DCOS, Fleet)
* Moving deeper into automated builds

In summary, Buckley recommends using what you already know and have---you don't need to reinvent the wheel. BI's use of Puppet before and after is a good example of this. He also says that there's no wrong way to experiment, go find what works for you.

At this point Buckley turns it back over to Marianna Tessel from Docker. She starts talking about Docker Hub, and where it stands today. The one word that sums up Docker Hub, for her, is "Quality". For example, Docker Hub is using the v2 Registry protocol, which results in 80% fewer requests and 60% less bandwidth, both of which mean faster pulls of Docker images. Improvements in Docker Hub have also resulted in 2.0x speedup in the Dashboard, as another example. She also highlights the reduced number of errors in the Docker Hub as an indicator of increased reliability. Additionally, Docker is working to improve the security of Docker Hub (this is probably a reflection of the recent study pointing out the number of images that have security flaws) through a variety of mechanisms. Tessel also goes through some UI changes in Docker Hub, including some new features/functionality.

Some of these changes to Docker Hub are already available, but Tessel announces a public beta of the new Docker Hub (available at https://hub-beta.docker.com), starting today, and extends an offer for 5 private repos for 90 days for DockerCon attendees.

At this point, Tessel brings Scott Johnston, VP of Product Management, to the stage. Johnston refers back to this year's DockerCon theme (running Docker in production), and shares some of the feedback that Docker has gotten from Docker users:

1. Support
2. On-premises registry
3. Networking
4. Security
5. Directory integration

Johnston points out that one of the most popular images on the Docker Hub is the open source Docker registry (6.5 million downloads). Johnston announces Docker Trusted Registry (DTR), which is a commercial product aimed at answering customers' demands for an on-premises registry solution. DTR is an on-premises registry that integrates with LDAP/Active Directory, offers role-based access control, and provides audit/event logging for compliance. Finally, DTR is easy to deploy, upgrade, and rollback (continues Docker's focus on ease of use). Johnston offers a few previews of the user interface for DTR. Over 800+ participants participated in the private beta and the early access program for DTR---this included companies like GE, Capital One, Disney, and others. At this point, Johnston introduces Nirmal Mehta from Booz Allen Hamilton. Mehta is the senior lead technologist, who works on the US General Services Administration (GSA) project. The US GSA is considered "customer #1" for DTR.

Mehta starts out with an overview of the US GSA project, and makes some jokes about how "Washington DC loves their monoliths". The current GSA applications look like stovepipes that often implement replicated services using different technologies and solutions (different RDMS solutions, different load balancers, duplicate identity/access management solutions). There's a lot of redundancy here, and Booz Allen was engaged to create an "open source first cloud-based" common platform that would modernize and streamline these stovepiped applications. Mehta shares that Booz Allen will be using Docker and open source technologies to build the common platform, and leads into a quick demo of what they've built so far. This solution uses tools like Consul, Jenkins, Docker & Docker Swarm, HAProxy, Chef, and AWS.

After the demo (which showed how a pull request flows from GitHub Enterprise to Jenkins to Docker/Docker Swarm and then to AWS for deployment), Mehta reviews the benefits GSA will realize:

* Improved customer-centric services
* Decreased time-to-market (Mehta used "increased time to market", but that seems backward to me)
* Reduced cost
* Creates opportunities for new business
* Decrease time for security review

What's next? Mehta talks about Notary and how that can be used for image governance, using Keywhiz (https://square.github.io/keywhiz) for inserting secrets in containers, working on container networking (a big one for lots of folks), and using Interlock (a build of HAProxy) plugins and APIs for deeper integration.

Mehta turns it back over to Johnston. Going back to the focus of running Docker in production, Johnston talks about support. Johnston talks about a new commercial support offering from Docker. A subscription offering bundles together support for Docker Engines, image registry (both DTR and Docker Hub), scaling to various levels based on response time and the number of Docker Engines in production. The subscription starts at $150/month for 10 Docker Engines, and outlines some partners that are helping bring support offerings and solutions to market.

Johnston introduces folks from Microsoft, IBM, and Booz Allen to talk about the future of distributed applications (Mark Russinovich, Jason McGee, and Michael Farber, respectively). The panel discusses the forces that are leading to microservices-based architectures and how this is driving evolution in development, organizations, applications, architectures, and culture.

As the panel wraps up, Johnston keeps Russinovich on stage, and transitions into a discussion about the use of both Linux and Windows for Docker containers. Johnston and Russinovich talk about some of the Docker-Microsoft collaborations that have occurred over the last year, one of which is Windows Server containers. This leads to a demo from Russinovich on various Microsoft integrations with Docker.

Russinovich shows Docker integration through Visual Studio, both on Windows and on OS X, and shows how Visual Studio on both platforms can build and deploy Docker containers to remote hosts (he used a remote Ubuntu host in the first part of his demo). Russinovich also shows how Visual Studio Online can perform continuous integration testing that leverages Docker in various ways, which includes use of Docker Swarm and Docker Compose. In the later part of his demo, he uses the Docker client on OS X to push a Docker image to a Windows Server host running Windows Server containers. His "one last thing" is a demonstration of Microsoft's particpation in the Docker hackathon over the weekend, which was Cortana integration with Docker for voice commands.

Johnston takes the stage again. Johnston sets the context for this last part, stating that this isn't a product, this is futures, and is coming from a different part of the organization: the sysadmins instead of the developers. He talks about "Project Orca," which has a vision of providing a top-to-bottom integrated stack that takes all the tools and plumbing (Docker Engine, Docker Swarm, Networking, GUI, Docker Compose, security, plus tools for installation, deployment, configuration, etc.). This is an "opinionated" stack, according to Johnston. Project Orca fits into the "run" portion of the "build-ship-run" triangle on which Docker focuses. Johnston introduces Evan Hazlett, the "chief hacker" for Project Orca.

The Orca demo starts out with a log in to a web-based UI. Hazlett shows how Orca integrates into directory services and provides role-based access controls, and shows the logging/auditing functionality, so that users can see what has happened. Orca will provide lists of images, as well as what's _inside_ the images (such as showing layers, and the sizes of layers). Orca has integration into Docker Swarm, to show the nodes in a Swarm cluster and manage the nodes in a Swarm cluster. Hazlett also shows how to see the images that are present in a cluster, pull updated images, and push them across the entire Swarm cluster. Naturally, Orca also shows the images that are running across the cluster, the images used by each container, and display the details for running containers (the node on which it's running, the ports that are exposed, the resources that are in use, the volumes associated with the container). Viewing the logs from a Docker container is also readily accessible from within Orca, as is real-time streaming statistics for the containers. Orca introduces the idea of "stacks," which correspond to a Docker Compose configuration (sounds a lot like OpenStack Heat to me). Deploying a stack is like `docker-compose up`, and application-specific (stack-specific) metrics are available within Orca. Orca also integrates the ability to scale the number of containers associated with a stack. Developers can update their application definitions (via changes to the Compose YAML definition), and Orca will allow those changes to be dynamically deployed to production without adverse affect to operations' ability to scale up or scale down capacity.

Hazlett wraps up the demo with a thanks to his colleagues at Docker, and Johnston takes the stage again to re-iterate the early/alpha/concept status of Orca. Johnston issues a call for feedback (sign up at http://bit.ly/project-orca).

At this point, Johnston starts to wrap up the general session by recapping the various key points of the general session: the Docker Hub, Docker Trusted Registry, commercial support offerings, and Project Orca. Johnston again thanks Docker's partners for their support, and finally thanks the users before closing out the session.