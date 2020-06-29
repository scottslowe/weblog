---
author: slowe
categories: Tutorial
comments: true
date: 2017-11-08T12:00:00Z
tags:
- Docker
- Python
- CLI
title: How to Tag Docker Images with Git Commit Information
url: /2017/11/08/how-tag-docker-images-git-commit-information/
---

I've recently been working on a very simple Flask application that can be used as a demo application in containerized environments (here's [the GitHub repo][link-2]). It's nothing special, but it's been useful for me as a learning exercise---both from a Docker image creation perspective as well as getting some additional Python knowledge. Along the way, I wanted to be able to track versions of the Docker image (and the `Dockerfile` used to create those images), and link those versions back to specific Git commits in the source repository. In this article, I'll share a way I've found to tag Docker images with Git commit information.<!--more-->

Before I proceed any further, I'll provide the disclaimer that this information isn't unique; I'm building on the work of others. Other articles sharing similar information include [this one][link-1]; no doubt there are countless more I haven't yet seen. I'm presenting this information here simply to show one way (not the only way) of including Git commit information with a Docker image.

Getting the necessary information from Git is actually far easier than one might think. This variation of the `git log` command will print only the full hash of the last commit to the repository:

    git log -1 --format=%H

If you prefer the shortened commit hash (which is what I use currently), then just change the `%H` to `%h`, like this:

    git log -1 --format=%h

Getting the information _out_ of Git is only half the puzzle, though; the other half is getting it _into_ the Docker image. The answer lies in some changes to the `Dockerfile` and the use of an additional command-line flag when building the image.

First, you'll need to add lines like this to your `Dockerfile`:

    ARG GIT_COMMIT=unspecified
    LABEL git_commit=$GIT_COMMIT

The first line defines a build-time argument, and the use of `=unspecified` means that if the built-time argument is omitted or not supplied, it will default to the value of "unspecified". The second line takes the information from the argument and adds it as a label on the image.

With the `Dockerfile` prepared to leverage Git commit information, all that's necessary is to build the image with the `--build-arg` flag, like this (here I'm showing the command I'd use to build the "flask-web-svc" image for the Flask application I've been building):

    docker build -t flask-local-build --build-arg GIT_COMMIT=$(git log -1 --format=%h) .

Here I'm using Bash command substitution to take the output of `git log -1 --format=%h` and supply it to `docker build` as the GIT_COMMIT argument (i.e., what the `Dockerfile` is expecting). This command assumes that you're building the Docker image from the latest Git commit; if this isn't the case, then you'll need to modify your command. As I mentioned earlier, if you omit the `--build-arg` parameter, then the label will be assigned with the default value of "unspecified".

When you build the image this way, you can then see the Git commit attached to the image as a label using this command:

    docker inspect flask-local-build | jq '.[].ContainerConfig.Labels'

Note I'm using the incredibly-useful `jq` tool here. (If you're not familiar with `jq`, check out [my introductory post][xref-1].)

Assuming that the build was successful and the container operates as expected/desired, then you can tag the image and push it to a registry:

    docker tag flask-local-build slowe/flask-web-svc:0.3
    docker push slowe/flask-web-svc:0.3

I also create a GitHub release corresponding to the Git commit used to build an image, so I can easily correlate a particular version of the Docker image with the appropriate commit in the repository. This makes it easier to quickly jump to the `Dockerfile` for each version of the Docker image. So, for example, when I release version 0.3 of the Docker image (which I recently did), I also have [a matching v0.3 release in GitHub][link-4] that points to the specific Git commit from which version 0.3 of the Docker image is built. This allows me---and anyone else consuming my Docker image---to have full traceability from a particular version of a Docker image all the way back to the specific Git commit from which that Docker image was built.

I imagine there are probably better/more efficient ways of doing what I've done here; feel free to [hit me up on Twitter][link-3] to help me improve. Thanks for reading!

**UPDATE:** Michael Gasch also pointed out that `git rev-parse HEAD` will return the full (long) commit hash from the last commit, so this is another way to get the information from Git. Given the nature of Git, no doubt there are countless more!

**UPDATE 2020-06-29:** A reader contacted me via Twitter to point out at the Open Containers Initiative has a list of suggested image tags/annotations, including one for the Git commit hash (it's "org.opencontainers.image.revision"). See [this page][link-5] for more information and details. Thanks, Hans!

[link-1]: http://container-solutions.com/tagging-docker-images-the-right-way/
[link-2]: https://github.com/scottslowe/flask-web-svc
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://github.com/scottslowe/flask-web-svc/releases/tag/v0.3
[link-5]: https://github.com/opencontainers/image-spec/blob/master/annotations.md
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
