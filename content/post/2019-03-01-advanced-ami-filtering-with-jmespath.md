---
author: slowe
categories: Explanation
comments: true
date: 2019-03-01T21:00:00Z
tags:
- AWS
- CLI
- JSON
title: Advanced AMI Filtering with JMESPath
url: /2019/03/01/advanced-ami-filtering-with-jmespath/
---

I recently had a need to do some "advanced" filtering of AMIs returned by the AWS CLI. I'd already mastered the use of the `--filters` parameter, which let me greatly reduce the number of AMIs returned by `aws ec2 describe-images`. In many cases, using filters alone got me what I needed. In one case, however, I needed to be even more selective in returning results, and this lead me to some (slightly more) complex [JMESPath][link-4] queries than I'd used before. I wanted to share them here for the benefit of my readers.<!--more-->

What I'd been using before was a command that looked something like this:

```bash
ec2 describe-images --owners 099720109477 \
--filters Name=name,Values="*ubuntu-xenial-16.04*" \
Name=virtualization-type,Values=hvm \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
--query 'sort_by(Images,&CreationDate)[-1].ImageId'
```

The part after `--query` is a JMESPath query that sorts the results, returning only the `ImageId` attribute of the most recent result (sorted by creation date). In this particular case, this works just fine---it returns the most recent Ubuntu Xenial 16.04 LTS AMI.

Turning to Ubuntu Bionic 18.04, though, I found that the same query _didn't_ return the result I needed. In addition to the regular builds of 18.04, Canonical apparently also builds EKS (Amazon's managed Kubernetes offering) versions and special minimal versions of Ubuntu 18.04 AMIs. It was one of those AMIs (the EKS-related one, in fact) that was getting returned by my command. So how do I go about filtering out images in a more granular fashion?

After reading through a few blogs (see [here][link-1], [here][link-2], and [here][link-3]---great work by their respective authors, thank you!), I finally stumbled upon the syntax that would work:

```bash
ec2 describe-images --owners 099720109477 \
--filters Name=name,Values="*ubuntu-bionic-18.04*" \
Name=virtualization-type,Values=hvm \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
--query 'sort_by(Images,&CreationDate)[?Name!=`null`]|[?contains(Name,`ubuntu-eks`)==`false`]|[?contains(Name,`minimal`)==`false`]|[-1].ImageId'
```

Let me walk through this command a bit:

* The first part of it uses `--filters` to show only those AMIs that are for the x86_64 architecture, using an EBS volume as the root device type, using hardware virtualization, with the string "ubuntu-bionic-18.04" somewhere in the name. This is pretty much identical to the Xenial version I showed earlier.
* The JMESPath query is more complex, however. The first part removes entries that don't have a value for the Name attribute. This is to prevent an error with JMESPath trying to evaluate results with no Name attribute and failing.
* The next two portions check for results that contain either "ubuntu-eks" _or_ "minimal" in the Name attribute. If either of those strings are present, the query will return a true result, and those AMIs would not be included in the final set of results (because the command is only selecting false results, i.e., items that do _not_ contain those strings).
* Finally, it selects the most recent AMI (using the `[-1]` syntax, since things are sorted by the creation date) and then returns only the value of the `ImageId` attribute.
* You'll note the command is using JMESPath's pipe (`|`) operator, which just takes the result of the previous query as its input.

This got me the result I saw seeking: the AMI ID for the latest Canonical build of Ubuntu Bionic 18.04, not any of the "special" builds that are also offered. Obviously, I can extend this command to exclude other variations as well as needed.

Hopefully, providing this example will be useful to readers in the event one of you needs to do the same kind of slightly more "advanced" filtering of results from an AWS CLI command. This example was for AMIs, but you could equally apply the same concepts to any number of other resources as well.

[link-1]: https://opensourceconnections.com/blog/2015/07/27/advanced-aws-cli-jmespath-query/
[link-2]: https://blog.ashiny.cloud/2017/09/25/useful-jmespath-in-awscli/
[link-3]: http://iambelmin.com/2016/03/20/using-aws-cli-to-find-rhel-and-ubuntu-amis/
[link-4]: http://jmespath.org/
