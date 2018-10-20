---
author: slowe
categories: Explanation
comments: true
date: 2018-10-20T14:00:00Z
tags:
- Development
- CLI
- Docker
title: Validating RAML Files Using Docker
url: /2018/10/20/validating-raml-files-using-docker/
---

Back in July of this year I [introduced Polyglot][xref-1], a project whose only purpose is to provide a means for me to learn more about software development and programming (areas where am I sorely lacking real knowledge). In the limited spare time I've had to work on Polyglot in the ensuing months, I've been building out an API specification using [RAML][link-4], and in this post I'll share how I use [Docker][link-1] and a Docker image to validate my RAML files.<!--more-->

Since I was (am) using [Visual Studio Code][link-2] as my primary text editor/development environment these days, I started out by looking for a RAML extension that would provide some sort of linting/validation functionality. I found an extension to do RAML syntax highlighting, which seemed like a reasonable first step.

After a bit more research, I found that there was a `raml-cli` NPM package that one could use to validate RAML files from the command line. I was a bit leery of installing an NPM package on my system, so I thought, "Why not use a Docker container for this?" It will keep my system clean of excess/unnecessary packages and dependencies, and it will provide some practice with building Docker containers.

A bit of work later I had this `Dockerfile` (which you can find in [my "dockerfiles" GitHub repository][link-3]):

```
FROM node:6.14.3-alpine

LABEL maintainer "Scott Lowe <scott.lowe@scottlowe.org>"

RUN apk add --no-cache git && \
    rm -rf /var/cache/apk/*

RUN npm install -qg raml-cli && \
    npm cache clean

RUN mkdir -p /data
VOLUME /data
WORKDIR /data

ENTRYPOINT ["raml"]

CMD ["--help"]
```

Feeding this to `docker build` results in an image of about 196MB in size, which isn't too shabby (I'd like to continue to try to shave this down even further). With this image, I can now easily validate RAML files:

```sh
docker run --rm -v $(pwd):/data raml-cli:0.1 validate orders-api.raml
```

And, of course, I could make this even easier with a Bash alias:

```sh
alias raml-cli="docker run --rm -v $(pwd):/data raml-cli:0.1"
```

Using the alias means I can now simply run `raml-cli validate <filename.raml>`. This makes it very easy for me to validate the RAML specifications I'm writing.

In the event others may find this helpful, the `Dockerfile` is on GitHub in [my "dockerfiles" repository][link-3], and I've [pushed the Docker image to Docker Hub][link-5] for anyone to pull down.

[link-1]: https://www.docker.com/
[link-2]: https://code.visualstudio.com/
[link-3]: https://github.com/scottslowe/dockerfiles/
[link-4]: https://raml.org/
[link-5]: https://hub.docker.com/r/slowe/raml-cli/
[xref-1]: {{< relref "2018-07-23-bolstering-my-software-development-skills.md" >}}
