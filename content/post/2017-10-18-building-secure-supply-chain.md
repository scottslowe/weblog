---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-18T14:30:00Z
tags:
- Docker
- DockerCon2017
- Security
title: Building a Secure Supply Chain
url: /2017/10/18/building-secure-supply-chain/
---

This is a liveblog of the session titled "Building a Secure Supply Chain," part of the Using Docker track at DockerCon EU 2017 in Copenhagen. The speakers are Ashwini Oruganti ([@_ashfall_][link-1] on Twitter) and Andy Clemenko ([@aclemenko][link-2] on Twitter), both from Docker. This session was recommended in the Docker EE deep dive (see [the liveblog for that session][xref-1]) as a way to get more information on Docker Content Trust (image signing). The Docker EE deep dive presenter only briefly discussed Content Trust, so I thought I'd drop into this session to get more information.<!--more-->

Oruganti starts the session by reviewing some of the steps in the software lifecycle: planning, development, testing, packaging/distribution, support/maintenance. From a security perspective, there are some additional concepts as well: code origins, automated builds, application signing, security scanning, and promotion/deployment. Within Docker EE, there are three features that help with the security aspects of the lifecycle: signing, scanning, and promotion. (Note that scanning and promotion were also discussed in the Docker EE deep dive, which I liveblogged; link is in the first paragraph).

Before getting into the Docker EE features, Clemenko reminds attendees how _not_ to do it: manually. This approach doesn't scale, and leaves organizations open to holes. With respect to the software supply chain, there are two starting points: the Docker Store and your source code repositories. Clemenko briefly plugs the Docker Store, Docker Certified items, and Docker Official images. Moving past the Certified and Official images is only recommended, according to Clemenko, if you the images are automated builds, you have access to the `Dockerfile`, and it makes sense. If these aren't possible, then you should look at building your own, and Clemenko briefly reviews some recommendations around the types of files that should be stored in a source code repository.

Clemenko next reminds the audience that automated builds are strongly recommended. One reason would be to provide an audit trail that shows the history of image builds.

Oruganti now takes over again to dig into the related Docker EE features. First up is image signing (Docker Content Trust). Oruganti reviews a few aspects of Docker Content Trust, and then goes into a new feature of the `docker trust` functionality that is intended to make image signing much easier. The demo of the new feature (which is apparently the `docker trust sign` command) shows how you could use UCP to set a signing policy, refuse to run applications that don't meet the signing policy, and then easily sign an image from the CLI.

Next, Oruganti moves on to image scanning. She doesn't spend much time here at all, and quickly moves on to talking about image promotion. As covered in the Docker EE deep dive session, image promotion is about moving "blessed" images between repositories within a single registry. Oruganti hands it off to Clemenko to do a demo of the image promotion functionality.

Clemenko's demo makes some changes to the source code, perform a `git commit` and `git push` to a GitLib repository. GitLab CI runs a build and pushes it to a Docker Trusted Registry (DTR) instance. The DTR repository gets updated automatically by GitLab, and then a promotion policy (using the criterion that there are no vulnerabilities) controls whether images pushed to this DTR repository will be automatically promoted to another repository (still within the same DTR instance). This is all the same stuff I saw in the Docker EE deep dive, but Clemenko also adds a webhook that calls GitLab CI again after promotion to sign the promoted image (using the new `docker trust` functionality Oruganti mentioned).

Next Clemenko sets up a demo that he knows will fail (he uses an older version of an image that contains known vulnerabilities). As expected/planned, the demo appropriately flags the image coming from the GitLab CI has vulnerabilities, and therefore the promotion policy is not triggered (the image is _not_ promoted to the public repository).

Oruganti and Clemenko wrap up the session by pointing attendees to the same 4 hour trial available of Docker EE, and then open the session up to questions from the audience.



[link-1]: https://twitter.com/_ashfall_
[link-2]: https://twitter.com/aclemenko
[xref-1]: {{< relref "2017-10-18-docker-ee-deep-dive.md" >}}
