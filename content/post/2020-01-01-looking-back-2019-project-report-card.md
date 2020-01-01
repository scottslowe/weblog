---
author: slowe
categories: Personal
comments: true
date: 2020-01-01T08:43:00-07:00
tags:
- Personal
- Career
- Development
- OSS
title: 'Looking Back: 2019 Project Report Card'
url: /2020/01/01/looking-back-2019-project-report-card/
---

As has been my custom over the last five years or so, in the early part of the year I like to share with my readers a list of personal projects for the upcoming year (here's [the 2019 list][xref-1]). Then, near the end of that same year or very early in the following year, I evaluate how I performed against that list of personal projects (for example, here's [my project report card for 2018][xref-2]). In this post, I'll continue that pattern with an evaluation of my progress against my 2019 project list.<!--more-->

For reference, here's the list of projects I set out for myself for 2019 (you can read [the associated blog post][xref-1], if you like, for additional context):

1. Make at least one code contribution to an open source project. _(Stretch goal: Make three code contributions to open source projects.)_
2. Add at least three new technology areas to my "learning-tools" repository. _(Stretch goal: Add five new technology areas to the "learning-tools" repository.)_
3. Become more familiar with CI/CD solutions and patterns.
4. Create at least three non-written content pieces. _(Stretch goal: Create five pieces of non-written content.)_
5. Complete a "wildcard project" (if applicable).

Here's how I would grade myself on my progress against this project list:

1. _Make at least one code contribution to an open source project:_ When I set out this goal for myself, I was thinking that I would contribute using Golang, and this would help "force" me to learn Golang in more depth. That, unfortunately, did not happen. If I were to grade myself only on that, I'd have to give myself an F. However, I did make other contributions. I had five infrastructure-as-code contributions to one project and two contributions to another project in Bash. So, I did achieve the _letter_ of this goal, but not the _spirit_ of the goal. Grade: **C**

2. _Add at least three new technology areas to my "learning-tools" repository:_ [My GitHub "learning-tools" repository][link-1] is a collection of...well, tools to help folks learn new technologies. I did spend a fair amount of time this year paying down technical debt in the repository by updating documentation, adding missing documentation, fixing bugs, updating versions of software, etc. However, this wasn't the focus on the project; the focus of the project was on expanding the coverage found in the repository. In that regard, I did manage to add two new areas (one area covering the use of [Cluster API][link-2] and [`kustomize`][link-3], and another area on [Pulumi][link-4]). I suppose if you counted CAPI and `kustomize` as two different areas then I did make it to my stated target of three new technology areas, but that seems like padding the statistics. Grade: **B**

3. _Become more familiar with CI/CD solutions and patterns:_ This was a really vague project, which I recognized when I published my list of 2019 projects. I _am_ now more familiar with CI/CD terminology and with tools or projects commonly used in CI/CD solutions, but I don't have any concrete measurables by which to gauge my progress. If I had, for example, added a technology area to the "learning-tools" repository for a particular CI/CD tool, then I'd have something concrete. Given the lack of concrete measurables, I'd say I did not make the progress I really need(ed) to make. Grade: **D**

4. _Create at least three non-written content pieces:_ This one is a bit tricky. On one hand, I did create several videos that have been published on [KubeAcademy][link-5]. However, should I count that? In my head, I was thinking of content I would create separate from work, like the content I create for this site, but I didn't specify that. I guess I'll split it halfway. Grade: **C**

5. _Complete a "wildcard project" (if applicable):_ As with 2018, I don't think that I have anything that really qualifies as a "wildcard project." I did say, though, that I wouldn't penalize myself if I didn't complete one. Grade: **N/A**

Overall, progress against my project list was fair---not great, but also not awful. Is there room to improve? Absolutely!

Even though I didn't do as well on my progress against this project list as I would've liked, the pursuit of progress did yield some valuable benefits:

* In working on video content, I learned of new tools (OBS Studio), learned new techniques for existing tools (like Camtasia), and gained experience in creating video content---all things which will be useful down the road.
* In trying to bolster my Golang skills enough to make a code contribution, I learned that I need more resources---so I invested in some video training on Golang that I hope will help.

Over the next week or two I'll be brainstorming and evaluating potential projects for 2020. This will be a challenge given [the upcoming adventure for the new year][xref-3], as I need to find projects that will align with---or at the very least not interfere with---efforts related to living in Tokyo, learning Japanese, and building out a team in Japan.

Additionally, the process of looking back on this list and evaluating my progress against the list was very useful in exposing "unspoken" assumptions about the items on the list. For the upcoming year, I not only need to provide specific, measurable goals, but I also need to be specific in spelling out these assumptions.

In the meantime, feel free to [contact me on Twitter][link-99] if you'd like to provide any feedback, suggest projects or goals I should consider, or if you just want to say hi. Thanks for reading!

[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: https://github.com/kubernetes-sigs/cluster-api
[link-3]: https://github.com/kubernetes-sigs/kustomize
[link-4]: https://www.pulumi.com/
[link-5]: https://kube.academy/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-03-15-looking-ahead-2019-projects.md" >}}
[xref-2]: {{< relref "2018-12-28-looking-back-2018-project-report-card.md" >}}
[xref-3]: {{< relref "2019-12-30-new-year-new-adventure.md" >}}
