---
author: slowe
categories: Information
comments: true
date: 2020-06-10T13:55:00-07:00
tags:
- Ansible
- AWS
- CAPI
- Docker
- Kubernetes
- Linux
title: Making it Easier to Get Started with Cluster API on AWS
url: /2020/06/10/making-it-easier-to-get-started-with-cluster-api-on-aws/
---

I've written a few articles about [Cluster API][link-1] (you can see a list of the articles [here][link-98]), but even though I strive to make my articles easy to understand and easy to follow along many of those articles make an implicit assumption: that readers are perhaps already somewhat familiar with Linux, Docker, tools like `kind`, and perhaps even Kubernetes. Today I was thinking, "What about folks who are new to this? What can I do to make it easier?" In this post, I'll talk about the first idea I had: creating a "bootstrapper" AMI that enables new users to quickly and easily jump into [the Cluster API Quick Start][link-2].<!--more-->

Normally, in order to use the Quick Start, there are some prerequisites that are needed first (these are all clearly listed on the Quick Start page):

* You need `kubectl` installed
* You need `kind` (which in turn requires Docker) or an existing Kubernetes cluster up and running

For Linux users (like myself), these prerequisites are pretty easy/simple to handle. But what if you're a Windows or Mac user? Yes, you could use Docker Desktop and then install `kind` (or use `docker-machine`, if [you're feeling adventurous][xref-1]). Then you'd have to also install `clusterctl`, `clusterawsadm`, etc. With that in mind, I decided to build an AWS AMI that has all these prerequisites installed already---thus enabling new users to simply spin up an instance and hit the ground running.

The AMI has the following tools already installed:

* The AWS CLI (it just needs to be configured with your credentials)
* `kubectl` (version 1.18.2)
* Docker CE (the latest version for Ubuntu 18.04; you'll need to add the "ubuntu" user to the "docker" group to avoid having to use `sudo`)
* `kind` (version 0.8.1)
* `clusterctl` (version 0.3.5)
* `clusterawsadm` (version 0.5.3)

With these tools all pre-installed, new users can immediately jump right into the Quick Start, and not have to worry about any of the steps that call for downloading or installing a binary---everything needed is already there.

There is one drawback right now, though: until such time as I can figure out how to securely share the AMI I've built without impacting my own AWS account (and monthly bill!), you'll have to build it yourself using [Packer][link-3] and [Ansible][link-4] and the files from [this GitHub repository][link-5].

To build your own "CAPI bootstrapper image":

1. Make sure you have Packer and Ansible installed. (Sorry, I tried to eliminate as much upfront work as possible, but there's only so much that can be done.)
2. Ensure your AWS access is working (make sure the AWS CLI works, for example).
3. Clone [my "packer-templates" GitHub repository][link-5] to your local system.
4. From the cloned repository, run this command:

        packer build templates/aws-capi-bootstrapper-1.18.2.json
    You may need to specify the full path to the `packer` binary, depending on how/where it is installed.

Once the Packer process completes, you'll have an AMI with all the prerequisites for getting started with Cluster API on AWS. Spin up an instance based on that AMI and you're almost completely ready to roll; the only additional step needed is to configure the AWS CLI in the instance with your credentials, as mentioned earlier. (I can't do that for you.)

Hopefully this helps someone. The Ansible roles and Packer template are both pretty simplistic right now; if you're experienced with these tools and are interested in improving them, I welcome PRs to the repository. If you have questions or suggestions for feedback, you're welcome to find [me on Twitter][link-99] or in [the Kubernetes Slack community][link-6]. I'd love to hear from you.

[link-1]: https://cluster-api.sigs.k8s.io/introduction.html
[link-2]: https://cluster-api.sigs.k8s.io/user/quick-start.html
[link-3]: https://www.packer.io/
[link-4]: https://www.ansible.com/
[link-5]: https://github.com/scottslowe/packer-templates
[link-6]: https://kubernetes.slack.com
[link-98]: /tags/capi/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-03-19-using-kind-with-docker-machine.md" >}}
