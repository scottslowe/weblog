---
author: slowe
categories: Musing
comments: true
date: 2015-01-23T00:00:00Z
tags:
- Puppet
- Automation
- Writing
- Ansible
title: Self-Documenting Systems?
url: /2015/01/23/self-documenting-systems/
---

I've been in Singapore this past week, wrapping up the week by speaking at the inaugural Singapore VMUG User Conference. While at the User Conference, I had the opportunity to attend a session by John Arrasjid (you probably know him as [@vcdx001][link-1] on Twitter) on the art of IT infrastructure design. Although John's session was on helping IT architects understand one possible methodology of approaching infrastructure design, his session got me thinking about documenting IT systems. Specifically, it got me thinking about self-documenting systems.

It's no secret that most IT departments do an abysmal job of maintaining documentation about their environments. Over time, small changes here and there to address issues or problems end up creating significant inconsistencies across the data center. IT departments are already stretched thin; people just simply don't have time to document environments as well as they should. It's not a condemnation; it's an observation.

At the same time, it's no secret that I'm a fan of using configuration management tools like [Puppet][link-2], [Chef][link-4], [Salt][link-3], or [Ansible][link-5]; in fact, expanding my knowledge of and experience with configuration management tools is one of [my 2015 personal projects][xref-1]. These tools can help eliminate configuration drift and unknown inconsistencies. Can they also be used to create self-documenting systems?

I'm hesitant to believe that a configuration management system alone is enough to create a self-documenting system. I say that because the "documents" most of these tools use---manifests in Puppet, cookbooks in Chef, states in Salt, playbooks in Ansible---aren't readable enough on their own to serve as the sole source of documentation. These sorts of tools would certainly be a big part of a self-documenting system. The real question(s) are these:

1. Are there additional tools that we could add to a configuration management system that might help?
2. If "yes" to #1, what are those tools?

I don't know the answers to these questions (yet).



[link-1]: https://twitter.com/vcdx001
[link-2]: https://puppet.com/
[link-3]: https://saltstack.com/
[link-4]: https://www.chef.io/
[link-5]: https://www.ansible.com/
[xref-1]: {{< relref "2015-01-16-looking-ahead-2015-projects.md" >}}
