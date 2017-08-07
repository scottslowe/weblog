---
author: slowe
categories: Liveblog
comments: true
date: 2017-04-18T00:00:00Z
tags:
- Docker
- DockerCon2017
title: 'Liveblog: Creating Effective Images'
url: /2017/04/18/creating-effective-images/
---

This is a liveblog for the DockerCon 2017 session titled "Creating Effective Images." The speaker is Abby Fuller, a Senior Technical Evangelist with Amazon Web Services. Abby is a former operations engineer who was an early consumer of Amazon's Elastic Container Service (ECS), and some of her learnings came about the "hard way." This session is from the "Using Docker" track.<!--more-->

Fuller starts with reviewing the agenda, and shares that she's intent on providing some practical tips that attendees can put to work immediately.

The first topic that Fuller tackles is the topic of container layers. A Docker container is made up of the read-only layers from the image itself, and a read/write layer at "the top" of the layers. Why do we care? Fewer layers means a smaller image, and smaller images means faster builds and faster deploys. (You may also see a reduced attack surface.)

The differences in making smaller images is important, Fuller explains, because the frequency of deployments is increasing (more deployments happening more quickly), and more containers are being deployed (sometimes at the behest of a CI/CD pipeline). This can result in significant amounts of disk space being consumed unnecessarily.

Some high-level tips for reducing the number of layers:

* Use shared base images where possible.
* Limit the data written to the ocntainer layer.
 
(Fuller advanced the presentation before I was able to capture the last two bullets from that list.)

In general, Fuller advises to use the cache whenever you can; it's a small step but it can save lots of time when building container images. She advises attendees to review the information available in the community on maximizing the use of cache (no specific resources provided or recommended).

Fuller starts with an example `Dockerfile` that is pretty "standard" for many images---it's based on the official Ubuntu image, running an `apt-get` to install a few packages, running `pip` to some Python modules, and then running an application. So how can we optimize this starting point?

The first step is to optimize the base image (specified on the `FROM` line). Simply by switching from Ubuntu to Alpine can save more than 350 MB of disk space (458 MB for Ubuntu versus 87 MB for Alpine). More examples include Debian coming in at 123 MB (compared to 458 MB for Ubuntu) or Fedora coming in at 230 MB (compared to 458 MB for Ubuntu). Pay attention to the base image selected---it may be beneficial to use a minimal base image, but there may also be specific use cases for a "full base OS". Fuller specifically mentions compliance or security as potential times when selecting a full base OS may be beneficial.

(Aside: Fuller also mentions ease of development as a use case for a full base OS; it strikes me that the use of multi-stage builds in Docker may provide a situation where you can use a full base OS for the initial build environments and a more minimal OS for the runtime environment.)

Choosing a more appropriate base image may also streamline the build process in other ways as well (perhaps you don't need to install some packages because the necessary files or libraries are already there).

Next, Fuller talks about how cache invalidations can affect image size. You can change the order of operations in the `Dockerfile` and use the `ONBUILD` keyword to help reduce image size. (Personally, I need to do some additional research to better understand these tips.)

Fuller now shifts to providing some recommendations for building Windows images for Docker. She points out the `ConvertTo-Dockerfile` PowerShell command to convert Windows images or VHD files into Docker containers. She also mentions to pay attention (still) to the base image; Windows Server Nano _may_ be a better choice for images unless you know you need a full base OS. Another "gotcha" is that you should _not_ use MSI packages to install applications on Windows. This is because MSI installations are not space-efficient. More Windows tips will be provided tomorrow (Wednesday) in a separate session on using containers on Windows.

Fuller transitions back to Linux now and takes a closer look at optimizing a `Dockerfile`. She shares several tips, such as using `&&` to combine commands into a single layer (which can reduce overall image size) and use the `--no-install-recommends` to tell the package manager _not_ to install recommended packages (this is in an Ubuntu/Debian context; you'd need to find the equivalent for environments that use `yum` or `dnf`). You can also build your own custom base container, which allows you to very tightly control what is or is not included in the base image (which can, in turn, help you optimize the size of the derived images).

Other optimizations to `RUN` commands include cleaning out package manager "leftovers" in the same layer (just appending some flavor of an `rm` command with an `&&` at the end of the package installation steps).

Switching users also creates a layer. Switch users (with the `USER` command in the `Dockerfile`) where needed, but don't switch users needlessly. Extensive use of the `USER` command may indicate a need to break into separate images or separate containers.

Fuller points out that you should "clean up after yourself" by removing large files (or other unnecessary cruft) once its purpose has been served. Again, this is handled by prudent use of `&& rm` in the `Dockerfile` to remove files when they are no longer needed.

When possible, it may make sense to use a language-specific base image, although there are times to use larger (more full-featured) base images. Fuller talks about using a separate image to build an artifact (like when using Go), then use a separate image for the runtime. (It seems to me this is another place where multi-stage builds would help.)

Judiciously use the `.dockerignore` file to ignore files that do not belong in a Docker container. In particular, Fuller calls out the debug log for Node.js, which can be egregiously large and should definitely be ignored.

Fuller now starts talking about multi-stage builds. In the "build-time" environment, use the `AS` keyword on the `FROM` line to specify the name of the build image; then, the `COPY` command in a second `Dockerfile` to specify the name of the build image and copy what's needed out of it.

Fuller mentions the Docker Security Scan service to identify security vulnerabilities in Docker images (including "official" images). Use it. Don't trust anyone, says Fuller.

Other optimizations include `docker image prune` to prune dangling images (images that are no longer needed by any other images). `docker system prune` goes a step further to remove unused volumes. Proper use of both of these commands helps you be a good Docker citizen and avoid getting woken up at 2am.

Of course, there are additional optimizations. Fuller doesn't explicitly call out LinuxKit, but mentions RancherOS, unikernels, and other highly minimal images.

Some key takeaways:

* Share layers where possible.
* Choose or build your own base wisely.
* Not all languages should build the same.
* Keep it simple, avoid extras.

And with that, Fuller wraps up the session.
