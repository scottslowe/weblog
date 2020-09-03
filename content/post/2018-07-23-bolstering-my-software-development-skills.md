---
author: slowe
categories: Information
comments: true
date: 2018-07-23T08:00:00Z
tags:
- Ballerina
- Development
- Git
- GitHub
- Go
title: Bolstering my Software Development Skills
url: /2018/07/23/bolstering-my-software-development-skills/
---

I recently [tweeted][link-1] that I was about to undertake a new pet project where I was, in my words, "probably going to fall flat on my face". Later, [I asked on Twitter][link-2] if I should share some of the learning that will occur (is ocurring) as a result of this new project, and a number of folks indicated that I should. So, with that in mind, I'm announcing this project I've undertaken is a software development project aimed at helping me bolster my software development skills, and that I'll be blogging about it along the way so that others can benefit from my mistakes...er, learning.<!--more-->

Readers may recall that [my 2018 project list][xref-1] included a project to learn to write code in [Golang][link-3]. At the time, I indicated I'd use [Kubernetes][link-4] and related projects, along with my goal of making more open source contributions, as a vehicle for helping to accomplish that goal. In retrospect, that was quite ambitious, and I've since come to the realization that there are a number of "baby steps" that I need to take _before_ I am ready to use a large software project like Kubernetes as a means to help improve my coding skills. In other words, I need to learn to walk (or even crawl!) before I can run.

At the same time, I've grown increasingly interested in [Ballerina][link-5], the new "cloud native programming language" that's recently sprung up. Given that it's difficult (perhaps even impossible?) to learn a programming language without some sort of project, I figured: why not build a simple, microservices-based application using multiple languages (specifically, Golang and Ballerina)?

Enter [Polyglot][link-6], a simple microservices-based application whose only purpose is to serve as a framework for bolstering my software development skills. Polyglot is laughably simple, at least right now: it has two services, both API driven, that will allow users or other services to interact with data from a back-end database. One will be written in Golang, and one will be written in Ballerina. That's it. Will it grow into something more? Probably. Will I realize later on that I did lots of things wrong along the way? Almost certainly. That, however, is kind of the point---without trying to tackle something like this, I can't grow my knowledge and skills beyond where they are right now. Part of learning is failing and making mistakes, and Polyglot gives me a place where I can fail, make mistakes, and (most importantly) learn from those failures and mistakes.

Along the way, I'll be blogging about what I have learned/am learning as I work on Polyglot. Feel free to [hit me up on Twitter][link-7] if you have questions or suggestions, and I welcome comments/PRs on [the Polyglot repository][link-6].

[link-1]: https://twitter.com/scott_lowe/status/1019743197105229824
[link-2]: https://twitter.com/scott_lowe/status/1020776898870181888
[link-3]: https://golang.org/
[link-4]: https://kubernetes.io/
[link-5]: https://ballerina.io/
[link-6]: https://github.com/scottslowe/polyglot
[link-7]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-03-05-looking-ahead-2018-projects.md" >}}
