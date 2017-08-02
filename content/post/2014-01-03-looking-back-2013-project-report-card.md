---
author: slowe
categories: Personal
comments: true
date: 2014-01-03T09:00:00Z
slug: looking-back-2013-project-report-card
tags:
- Automation
- Linux
- Networking
- OpenStack
- OVS
- Personal
- Puppet
- Ubuntu
title: 'Looking Back: 2013 Project Report Card'
url: /2014/01/03/looking-back-2013-project-report-card/
wordpress_id: 3393
---

For the last couple of years, I've been sharing my annual "projects list" and then grading myself on the progress (or lack thereof) on the projects at the end of the year. For example, I shared [my 2012 project list][1] in early January 2012, then [gave myself grades on my progress][2] in early January 2013.

In this post, I'm going to grade myself on [my 2013 project list][3]. Here's the project list I posted just under a year ago:

1. Continue to learn German.

2. Reinforce base Linux knowledge.

3. Continue using Puppet for automation.

4. Reinforce data center networking fundamentals.

So, how did I do? Here's my assessment of my progress:

1. _Continue to learn German:_ I have made some progress here, though certainly not the progress that I wanted to learn. I've incorporated the use of [Memrise](http://www.memrise.com/), which has been helpful, but I still haven't made the progress I'd like. If anyone has any other suggestions for additional tools, I'm open to your feedback. Grade: **D** _(below average)_

2. _Reinforce base Linux knowledge:_ I've been suggesting to VMUG attendees that they needed to learn Linux, as it's popping up all over the place in all sorts of roles. In [my original 2013 project list][3], I said that I was going to focus on RHEL and RHEL variants, but over the course of the year ended up focusing more on Debian and Ubuntu instead (due to more up-to-date packages and closer alignment with OpenStack). Despite that shift in focus, I think I've made decent progress here. There's always room to grow, of course. Grade: **B** _(above average)_

3. _Continue using Puppet for automation:_ I've made reasonable progress here, expanding my use of Puppet to include managing Debian/Ubuntu software repositories (see [here][4] and [here][5] for examples), [managing SSH keys][6], [managing Open vSwitch (OVS)][7] via a third-party module, and---most recently---exploring the use of Puppet with OpenStack (no blog posts--_yet_). There's still quite a bit I need to learn (some of my manifests don't work quite as well as I'd like), but I did make progress here. Grade: **C** _(average)_

4. _Reinforce data center networking fundamentals:_ Naturally, my role at VMware has me spending a great deal of time on how network virtualization affects DC networking, and this translated into some progress on this project. While I gained solid high-level knowledge on a number of DC networking topics, I think I was originally thinking I needed more low-level "in the weeds" knowledge. In that regard, I don't feel like I did well; on the flip side, though, I'm not sure whether I really _needed_ more low-level "in the weeds" knowledge. This highlights a key struggle for me personally: how to balance the deep, "in the weeds" knowledge with the high-level knowledge. Suggestions on how others have overcome this challenge are welcome. Grade: **C** _(average)_

In summary: not bad, but could have been better!

What's **not** reflected in this project list is the progress I made with understanding OpenStack, or my deepened level of knowledge of OVS (just browse [articles tagged OVS](/tags/ovs/) for an idea of what I've been doing in that area).

Over the next week or two, I'll be reflecting on my progress with my 2013 projects and thinking about what projects I should be taking in 2014. In the meantime, I would love to hear any feedback, suggestions, or thoughts on projects I should consider, technologies that should be incorporated, or learning techniques I should leverage. Feel free to speak up in the comments below.

[1]: {{< relref "2012-01-02-some-projects-for-2012.md" >}}
[2]: {{< relref "2013-01-07-looking-back-2012-project-report-card.md" >}}
[3]: {{< relref "2013-02-07-looking-ahead-my-2013-projects.md" >}}
[4]: {{< relref "2013-10-04-using-puppet-for-ubuntu-cloud-archive-support.md" >}}
[5]: {{< relref "2013-12-17-updating-ubuntu-cloud-archive-support-with-puppet.md" >}}
[6]: {{< relref "2013-10-21-managing-ssh-authorized-keys-with-puppet.md" >}}
[7]: {{< relref "2013-12-18-managing-open-vswitch-with-puppet.md" >}}
