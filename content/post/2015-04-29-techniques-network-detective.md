---
author: slowe
categories: Liveblog
comments: true
date: 2015-04-29T16:00:00Z
tags:
- Networking
- Interop2015
- CLI
title: 'Interop Liveblog: Techniques of a Network Detective'
url: /2015/04/29/techniques-network-detective/
---

This is "Techniques of a Network Detective," led by Denise "Fish" Fishburne ([@DeniseFishburne][link-1] on Twitter). Denise starts the session with a quick introduction, in which she discloses that she is a "troubleshooting junkie." She follows up with a short description of what life looks like in her role in the customer proof-of-concept lab at Cisco.

Denise kicks off the main content of the session by drawing an analogy between solving crimes and solving network performance/behavior problems. The key is technique and methodology, which may sound boring but really have a huge payoff in the end.

When a network error occurs, the network _is_ the crime scene. This crime scene is filled with facts, clues, evidence, and potential witnesses---or even potential suspects. How does one get from receiving notification of the problem, to asking the right questions, to solving the problem? Basically it boils down to these major areas:

* First, identify the suspects (even if the problem seems immediately obvious). This involves gathering facts, collecting clues, following the evidence, and interviewing witnesses.
* Next, question the suspects. Although you may not be an SME (subject matter expert), you can still work logically through gathering facts from the suspects.
* After you identify the problem, it's time to improve (document, prevent/prepare/repair)
* Even before being notified there was a problem, you need to be prepared for a failure (be aware of failures).

Denise starts with "Be Prepared." This means that you need to know your crime scene, know what is normal, know your witnesses, and know your suspects. Spending more time here will directly impact (in a positive way) time spent troubleshooting. Knowing "normal" behavior is important---how will you be able to distinguish abnormal behavior without this baseline. Distinguishing facts from clues requires an understanding of "normal" behavior. Traffic that routes through a particular location is just a fact. However, knowing that the traffic doesn't _normally_ route through that location turns that information from a fact into a clue. This also means you need to how how you expect it to fail---what is the expected behavior if there is a failure? If a particular router or location fails, what is the expected behavior? If the network behaves differently than expected after a failure, this is a clue.

"Being Prepared" also means knowing your witnesses and suspects---this includes switches, routers, firewalls, cables, patch panels, hosts, virtualized workloads, and virtualized network functions. Network monitoring tools are important; they are like surveillance cameras. Know how to interview the witnesses and suspects: what commands to run, what protocols to investigate, what tables to investigate, what values to expect, etc.

You also need to know that a crime has occurred. This helps you reduce downtime and enables you to "get to the scene of the crime" when it is fresh---or possibly even active. This gets you fresh evidence and information before anything else has happened. Finally, getting there quickly can help avoid cascading failures.

Denise also underscored the need for accurate and up-to-date "crime scene maps" (i.e., network topology/map). _Documentation like this is critically important._
She uses the game of "Clue" (and the secret passageways that exist there) as an example of the critically-important need for accurate, comprehensive, and up-to-date network maps. She shares some tips for diagrams: using different line styles to denote layer 3 links versus trunks, using different thicknesses to denote preferred paths, and avoiding the color red.

Next, Denise moves into "Finding the Suspects." This involves:

* Gather the facts: Ask questions like "What is the problem?" or "What is happening?" Sometimes understanding the problem is the biggest step. On the first pass, you're looking for the quick confession or obvious clues. The second pass---assuming you didn't find a "quick confession" in the first pass---becomes a scavenger hunt for clues. **Be sure to take notes/document things as you progress.** The third pass involves preparing for deeper troubleshooting.
* Collect the clues: Where is the problem?
* Follow evidence
* Interview witnesses

A methodology is really helpful here. Here are some things to avoid during this process:

* Assumptions and/or holding on too tightly to a theory
* Inaccurate crime scene map
* Inability to differentiate fact from clue/evidence
* Silos on the team or in the network
* Fear and/or pride

Next up is "Question the Suspects." In this portion of the process, there is no substitute for knowledge and experience. You're very likely going to need to bring in appropriate SMEs during this portion of troubleshooting. You need to know the _modus operandi_: the normal mode of operation for a device or function.

Here's an important takeaway: **Every failure should improve the procedures.** Learn from failures, learn from mistakes---of course, this requires documentation. This is an important part of the "Improve" section to be even better after a failure. Document, document, document! When documenting, don't focus on fault---instead, focus on fixes. Document how this problem can be prevented in the future. Document how you can be better prepared for this type of failure in the future. Document how to get to repair faster. What are the weakest areas? How can the network be improved?

And with that, Denise opens the floor to questions and the session wraps up.


[link-1]: https://twitter.com/denisefishburne/
