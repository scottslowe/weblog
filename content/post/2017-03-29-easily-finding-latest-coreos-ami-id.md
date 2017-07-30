---
author: slowe
categories: Tutorial
comments: true
date: 2017-03-29T00:00:00Z
tags:
- Linux
- CLI
- CoreOS
- AWS
title: Easily Finding the Latest CoreOS AMI ID
url: /2017/03/29/easily-finding-latest-coreos-ami-id/
---

It seems as if finding the right Amazon Machine Image (AMI) ID for the workload you'd like to deploy can sometimes be a bit of a challenge. Each combination of region and AMI produces a unique ID, so you have to look up the AMI for the particular region where you're going to deploy the workload. This in and of itself wouldn't be so bad, but then you have to wade through multiple versions of the same AMI in each region. Fortunately, if you're using [CoreOS Container Linux][link-3] on AWS, there's an easy way to find the right AMI ID. Here's how it works.

CoreOS publishes a JSON feed of the latest AMI for each of their channels (stable, beta, and alpha). You can find links to these JSON feeds [on this page][link-1]. This is powerful for 2 reasons:

1. Because it's available via HTTP, you can use `curl` to retrieve it anytime you need it.

2. Because it's in JSON, you can use `jq` (see [my post on `jq`][xref-1] for more information) to easily parse it to find the information you need. (Not super comfortable with JSON? Check out [my introductory post][xref-2].)

Putting these two reasons together, you end up with a command that looks something like this:

    curl -s https://coreos.com/dist/aws/aws-stable.json | jq -r '."us-west-2".hvm'

This command pulls the stable JSON feed, then parses it using `jq` to look for the "us-west-2" AMI. Note the somewhat odd quoting necessary for `jq` to work properly. The `-s` parameter to `curl` suppresses any output, and the `-r` parameter to `jq` returns plain, unformatted (raw) text. Use some [Bash command substitution][link-2] to assign the output of this command to a variable and you've got an easy, always up-to-date way of determining the AMI ID for the latest release of CoreOS Container Linux in your AWS region of choice.

Now, if only other organizations would do the same...

Enjoy!


[link-1]: https://coreos.com/os/docs/latest/booting-on-ec2.html
[link-2]: http://wiki.bash-hackers.org/syntax/expansion/cmdsubst
[link-3]: https://coreos.com/os/docs/latest
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
[xref-2]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
