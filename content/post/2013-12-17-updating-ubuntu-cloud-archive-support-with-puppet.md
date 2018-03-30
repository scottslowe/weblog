---
author: slowe
categories: Explanation
comments: true
date: 2013-12-17T09:00:00Z
slug: updating-ubuntu-cloud-archive-support-with-puppet
tags:
- Automation
- Linux
- OpenStack
- Puppet
- Ubuntu
title: Updating Ubuntu Cloud Archive Support with Puppet
url: /2013/12/17/updating-ubuntu-cloud-archive-support-with-puppet/
wordpress_id: 3386
---

Some time ago, I showed you [how to use Puppet to add Ubuntu Cloud Archive support][1] to your Ubuntu installation. Since that time, [OpenStack](http://www.openstack.org/) has had a new release (the Havana release) and the Ubuntu Cloud Archive repository has been updated with new packages to support the Havana release. In this post, I'll show you an updated snippet of code to take advantage of these newer packages in the Ubuntu Cloud Archive repository.

For reference, [here's the original Puppet code][gist-1] I posted in the first article. This code points your Ubuntu installation to the Grizzly packages.

Here's updated code that will point your installation to the appropriate packages to support OpenStack's Havana release:

``` puppet
apt::source { 'ubuntu-cloud':
  location          =>  'http://ubuntu-cloud.archive.canonical.com/ubuntu',
  repos             =>  'main',
  release           =>  'precise-updates/havana',
  include_src       =>  false,
  required_packages =>  'ubuntu-cloud-keyring',
}
```

(Click [here](https://gist.github.com/scottslowe/7949509) for this code snippet as a GitHub Gist.)

As you can see, there is only one small change between the two code snippets: changing "precise-updates/grizzly" in the first to "precise-updates/havana" in the second. (Naturally, this assumes you're using Ubuntu 12.04, the latest LTS release as of this writing.) I know this seems like a pretty simple thing to post, but I wanted to include it here for the sake of completeness and the benefit of future readers.

Feel free to speak up in the comments with any questions, suggestions, or corrections.

[1]: {{< relref "2013-10-04-using-puppet-for-ubuntu-cloud-archive-support.md" >}}
[gist-1]: https://gist.github.com/scottslowe/6827241
[gist-2]: https://gist.github.com/scottslowe/7949509
