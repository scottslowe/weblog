---
author: slowe
categories: Personal
comments: true
date: 2015-12-31T00:00:00Z
tags:
- Personal
- OpenStack
- OSS
- Ansible
- OVS
title: 'Looking Back: 2015 Project Report Card'
url: /2015/12/31/looking-back-2015-project-report-card/
---

In early 2015, I posted [a look ahead at my planned 2015 projects][xref-4], where I took a quick look at some of the self-development projects I set out for myself over the course of 2015. In this post, I'm going to review my progress on those 2015 projects.

The 2015 projects were as follows:

1. Complete a new book
2. Make more open source contributions
3. Expand to a new configuration management solution
4. Complete a "wildcard project"

So, how well did I do? Let's take a look.

1. _Complete a new book:_ Technically, I haven't (fully) completed a new book, but given that [my new book project with Jason Edelman and Matt Oswalt on network automation][xref-3] is [available now as an Early Access edition][link-6], I suppose this should count for _something_. Strangely enough, this wasn't the book project I had in mind at the start of 2015, but sometimes things like this take unexpected turns. Grade: **C**

2. _Make more open source contributions:_ I expected this one to be easy, but it turns out that this is the area where my performance is the worst. I submitted a pull request to [Terraform][link-5] (for a docs update), but I did not make the contributions I had expected (or planned) to make to projects like [OpenStack][link-4], [Open vSwitch (OVS)][link-3], or Open Virtual Network (OVN). Grade: **F**

3. _Expand to a new configuration management solution:_ I've done a pretty fair amount of work in [Ansible][link-2] this year, although not all of it has shown up here on the blog (only a couple of articles on [bootstrapping servers][xref-1] or [bootstrapping cloud instances][xref-2] into Ansible). I still have a lot to learn, but I feel reasonably comfortable with the foundational knowledge I've established. Grade: **C**

4. _Complete a "wildcard project":_ Looking back and assessing my performance here is probably the hardest, due to the vague nature of this goal. What constitutes a "wildcard project", anyway? Is a few blog posts on a topic enough? However, in looking back over 2015, one thing does stand out that doesn't fall into any other categories: the creation of [my GitHub "learning-tools" repository][link-7] (first commit in early February 2015). This repository has Vagrant and Terraform environments to help people learn new technologies like Consul, Docker Swarm, LXD, VMware Photon, and others. Based on the feedback I've received, this is proving to be useful to the community. Grade: **B**

In summary: pretty good, but there's always room to improve and strive for more! (And that's exactly what I aim to do.)

I'll spend the next few weeks reflecting on the progress toward my 2015 projects and how that needs to affect the projects I select for 2016. Once I've sorted all that, I'll post a list of my 2016 projects. Until that time, feel free to drop me an e-mail (my address is on this site) or [hit me on Twitter][link-8] if you have any feedback on my 2015 projects or things I should be considering for 2016.

Thanks for another awesome year!



[link-2]: http://www.ansible.com/
[link-3]: http://openvswitch.org/
[link-4]: http://www.openstack.org/
[link-5]: https://terraform.io/
[link-6]: http://shop.oreilly.com/product/0636920042082.do
[link-7]: https://github.com/scottslowe/learning-tools
[link-8]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-05-26-bootstrap-servers-ansible.md" >}}
[xref-2]: {{< relref "2015-11-23-bootstrapping-cloud-instances-ansible.md" >}}
[xref-3]: {{< relref "2015-12-28-next-gen-network-engineering-skills.md" >}}
[xref-4]: {{< relref "2015-01-16-looking-ahead-2015-projects.md" >}}
