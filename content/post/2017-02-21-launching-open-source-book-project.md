---
author: slowe
categories: Information
comments: true
date: 2017-02-21T00:00:00Z
tags:
- Linux
- CLI
- OVS
- Networking
- Writing
title: Launching an Open Source Book Project
url: /2017/02/21/launching-open-source-book-project/
---

In [my list of planned 2017 projects][xref-1], I mentioned that one thing I'd like to do this year is launch an open source book project. Well, I'm excited to announce _The Open vSwitch Cookbook_, an Apache 2.0-licensed book project aimed at providing "how to" recipes for [Open vSwitch (OVS)][link-1].

Portions of the book [are already available][link-3], with more content being added soon (more on that in a moment).

I'm using [GitBook][link-4] as the publishing platform; this allows me to write in [Markdown][link-5] and publish to a variety of formats. I'll only be publishing to HTML at first; other formats may come down the road. I chose GitBook for a few reasons:

1. It's free for open source projects. This book, as well as the software that is its focus, are both open source projects.
2. As I mentioned already, I can use Markdown for all the content.
3. It allows me to store the book in a [Git][link-8] repository and use standard Git workflows.

I decided _against_ using GitBook to host the Git repository for the book. Instead, [the book's source is found on GitHub][link-2]. This enables collaboration on the book's content---an aspect of this project that I think is much more important and far-reaching. You see, I don't want this book to be a solo effort; I encourage anyone and everyone who has worked with OVS to feel free to pitch in. How can you help?

* You can create [issues for the book][link-6] (new recipes you'd like to see, inaccuracies or errors in existing recipes, suggestions for improving the organization or content)
* You can submit your own content or corrections [via GitHub pull requests][link-7]
* You can help spread the word about the cookbook, so that it reaches more OVS users

Stay tuned for more updates as this project progresses.

**UPDATE 28 March 2017:** I'm canceling this project. See this post for more details.



[link-1]: http://openvswitch.org/
[link-2]: https://github.com/scottslowe/ovs-cookbook
[link-3]: https://www.gitbook.com/book/scottslowe/ovs-cookbook/details
[link-4]: https://www.gitbook.com/
[link-5]: https://en.wikipedia.org/wiki/Markdown
[link-6]: https://github.com/scottslowe/ovs-cookbook/issues
[link-7]: https://github.com/scottslowe/ovs-cookbook/pulls
[link-8]: https://git-scm.com/
[xref-1]: {{< relref "2017-01-31-looking-ahead-2017-projects.md" >}}
[xref-2]: {{< relref "2017-03-28-canceling-ovs-cookbook-project.md" >}}
