---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-17T13:00:00Z
tags:
- Docker
- DockerCon2017
title: "Rock Stars, Builders, and Janitors: You're Doing it Wrong"
url: /2017/10/17/rock-stars-builders-janitors/
---

This is a liveblog of the session titled "Rock Stars, Builders, and Janitors: You're Doing it Wrong". The speaker is Alice Goldfuss ([@alicegoldfuss][link-1]) from GitHub. This session is part of the "Transform" track at DockerCon; I'm attending it because I think that cultural and operational transformation is key for companies to successfully embrace new technologies like containers and fully maximize the benefits of these technologies. (There's probably a blog post in that sentence.)<!--more-->

Goldfuss starts out by asking the audience some questions about what they've been doing for the last 3 months, and then informs the attendees that they are, in fact, part of the problem.

Goldfuss now digs into the meat of the presentation by covering some terminology. First, what is a _rock star_? They're the idea person, the innovator. They're curious, open-minded, iterating faster, and always looking for the new things and the new ideas. They're important to our companies, but they do have some weaknesses. They get bored easily, they have no patience for maintenance, and they're not used to thinking about end user experience. Thus, according to Goldfuss, you can't have a team of only rock stars.

Next, Goldfuss talks about _builders_. Builders understand scale, reliability, planning, and taking what the rock stars made up and catching the "what ifs". That also makes them the resident doom-sayers. Their weaknesses including moving more slowly, having a hard time letting things go, and being a bit too negative/critical. Thus, according to Goldfuss, you can't have a team of only builders.

Last, but not least, Goldfuss talks about the _janitors_. Names aside (Goldfuss eschews the use of custodians or stewards), janitors are really, really important. Janitors understand the weaknesses of the systems, they're really good at bandaids, and they are able to do a lot with a little. However, janitors also have weaknesses. They can often resort to cleaning manually instead of automating it, they may get stuck in the weeds instead of looking at the big picture, and are resistant to change. Thus, Goldfuss says you can't have a team of only janitors.

The arrangement that we've all seemed to agree to puts rock stars with the most visibility (and the least amount of undesirable work) and janitors with the least visibility (and the most amount of undesirable work). Goldfuss says that while organic is usually good, in this particular case she's on the side of GMO (Greater Mobility, Okay?).

So what's the problem with this arrangement? People feel stuck, and they get tunnel vision (this causes them to build their own little kingdoms which causes silos within the organization).

Goldfuss shares some statistics from a survey she ran in June of this year, and ultimately shows that there is some unrest within organizations as a result of unfair work distribution. Companies try various things to help address this unrest: focusing on reliability (to reduce the burden on the janitors) or rotating the teams (letting people try different types of work). Neither of these solutions works long-term.

So what's the long-term answer? It comes from Ursula Le Guin. Specifically, it comes from Le Guin's book _The Left Hand of Darkness_. Goldfuss explains the story a bit (I'll leave it to the readers to go research the book to determine the story), and uses the book to say that the answer is to _have everyone do everything._

Once the team gets over the initial shock, Goldfuss talks about the benefits this approach brings. First, teams will build better. Second, teams will foster and develop new skills. How do we implement this? First, rotate the roles in a team. Goldfuss provides an example of how this rotation may work (1 week janitor, 1 week builder, and 2 weeks rock star). This proposed stage (50% rock star, 25% janitor, and 25% builder) looks somewhat like the preferred state that Goldfuss observed out of the survey she ran earlier in the year. Goldfuss also argues that this arrangement will help with both technical debt and velocity.

So, where do people start? Goldfuss says to roll this out to the Ops team first, which is currently mostly janitor and wants to be builders. Ask them three questions: What are you worried about? What could we do better? What do you want to add? (These questions identify janitor, builder, and rock star tasks, respectively.) This arrangement may put pressure on upstream teams, but according to Goldfuss this is absolutely necessary in order to help them understand the roles of each team and the impacts of each team's activities on the other teams.

What are some rock star tasks? Researching new projects or creating demo mockups are good ideas. What about builder tasks? Improving testing, improving service resiliency, or understanding the impact scope are good ideas. And janitor tasks? Bug fixes, refactoring, and configuration cleanup are all great candidates.

Finally, Goldfuss summarizes the key points from her presentation, and concludes the presentation.



[link-1]: https://twitter.com/alicegoldfuss
