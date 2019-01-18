---
author: slowe
categories: Information
comments: true
date: 2019-01-11T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Microsoft
- Azure
- AWS
- Terraform
- Wireless
- Linux
- Fedora
- Cisco
- Vagrant
- VMware
- vSphere
- Git
title: 'Technology Short Take 109'
url: /2019/01/11/technology-short-take-109/
---

Welcome to Technology Short Take #109! This is the first Technology Short Take of 2019. It may be confirmation bias, but I've noticed of number of sites adding "Short Take"-type posts to their content lineup. I'll take that as flattery, even if it wasn't necessarily intended that way. Enjoy!<!--more-->

## Networking

* Niran Even-Chen [says service mesh is a form of virtualization][link-9]. While I get what Niran is trying to say here, I'm not so sure I agree with the analogy. Sometimes analogies such as this are helpful, but sometimes the analogy brings unnecessary connotations that make understanding new concepts more difficult. One area where I do strongly agree with Niran is in switching your perspective: looking at service mesh from a developer's perspective gives one quite a different viewpoint than viewing service mesh in an infrastructure light.
* Jim Palmer has [a detailed write-up][link-15] on DHCP Option 51 and different behaviors from different DHCP clients.
* Niels Hagoort talks about [some network troubleshooting tools][link-23] in a vSphere/ESXi environment.

## Servers/Hardware

Nothing this time around, but I'll stay alert for items to include next time.

## Security

* Nandor Kracser outlines a method that Banzai Cloud uses to [inject secrets from Vault directly into Kubernetes Pods][link-1].
* Via Container Journal, tech journalist Bill Doerrfeld [outlines how to use the open source Anchore Engine][link-3] to perform Docker container image auditing.
* Eric Hammond explains [how to use AWS SSM parameter store with Git SSH keys][link-8] for secure distribution of Git credentials.

## Cloud Computing/Cloud Management

* Chris Nwamba walks readers through [getting started with Azure Functions][link-6] (using Visual Studio Code).
* Here's another "getting started" post, this time from Kyle Galbraith and this time [focusing on AWS Elastic Beanstalk][link-7]. Elastic Beanstalk is an AWS service that I've heard mentioned a lot, but Kyle's post was helpful in breaking it down and comparing it to other services.
* Need a curated list of KubeCon 2018 session content? [Look no further][link-12].
* Spot Instances are a cost optimization for AWs that I simply haven't had time to really explore. Fortunately, there are others out there who _have_ had time to explore, and one of them provides a write-up on [using Spot Instances with Kubernetes using Amazon EKS][link-14].
* Here's an interesting look at [using the HashiCorp stack on your home network][link-20]. It'll be interesting to get more details in future portions of this series.
* Matthew Sullivan shares some information on how his employer [leverages the "cattle not pets" appraoch for managing infrastructure][link-21]. Although nothing Matthew shares is necessarily revolutionary, it's still useful and helpful to hear stories from other real-world users applying these techniques in the trenches.

## Operating Systems/Applications

* Jorge Salamero Sanz describes [how to use the Sysdig Terraform provider][link-2] to do "container security as code." I'm a fan of Terraform (despite some of its limitations), so it's kind of cool to see new providers coming online.
* [OS/2 lives on][link-4]. 'Nuff said.
* [This project][link-13] purports to help you generate an AWS IAM policy with exactly the permissions needed. It's a bit of a brute force tool, so be sure to read the caveats, warnings, and disclaimers in the documentation!
* I do manage most of my "dotfiles" in a Git repository, but I'd never heard of `rcm` before reading [this Fedora Magazine article][link-18]. It might be something worth exploring to supplant/replace my existing system.
* I found this article by Forrest Brazeal on [a step-by-step exploration of moving from a relational database to a single DynamoDB table][link-22] to be very helpful and very informative. DynamoDB---along with other key-value store solutions---have been something I've been really interested in better understanding, but never could quite understand how they fit with traditional RDBMSes. I still have tons to learn, but at least now I have a bit of a framework by which to learn more. Thanks Forrest!
* Steve Flanders provides [an introduction to Ambassador][link-24], an open source API gateway. This looks interesting, but embedding YAML configuration in annotations seems...odd.
* Mark Hinkle, a co-founder at TriggerMesh, [announces TriggerMesh KLR][link-25]---the Knative Lambda Runtime that allows users to run AWS Lambda functions in a Knative-enabled Kubernetes cluster. This seems very powerful to me, but I'm no serverless expert so maybe I'm missing something. Would the serverless experts care to weigh in?
* Via [Jeremy Daly's Off-by-None newsletter][link-27], I found Jerry Hargrove's [Cloud Diagrams & Notes][link-26] site. I haven't dug in terribly deep yet, but at first glance Jerry's site looks to be enormously helpful. (I have a suspicion that I've probably seen references to Jerry's site via Corey Quinn's [Last Week in AWS newsletter][link-28], too.)
* AWS users who prefer Visual Studio Code may want to track the development of the [AWS Toolkit for Visual Studio Code][link-29]. It's early days yet, so keep that in mind.
* And while we're talking about Visual Studio Code, Julien Oudot highlights [why users should choose Code for their Kubernetes/Docker work][link-30].

## Storage

* Here's [a quick primer on using OpenEBS for Kubernetes cluster storage][link-19].

## Virtualization

* Marc Weisel shares [how to use Cisco IOSv in a Vagrant box with VMware Fusion][link-5].
* Paul Czarkowski talks about how [the future of Kubernetes is virtual marchines][link-10]. The title is a bit of linkbait; what Paul is really addressing here is how to solve the multi-tenancy challenges that currently exist with Kubernetes (which wasn't really designed for multi-tenant deployments). VMs provide good isolation, so VMs could be the method whereby operators can provide the sort of strong isolation that multi-tenant environments need. One small clarification to Paul's otherwise excellent post: by admission on [their own web page][link-11], gVisor is _not_ a VM container technology, but rather uses a different means to providing additional security.

## Career/Soft Skills

* Matt McCue [shares some advice on effective communications in a virtual world][link-16]. Increasingly, more and more teams have remote members, so adjusting communications techniques to accommodate remote members is effective.
* If you're looking for some skills to focus on in the new year, Justin Paul has prepared [a list of 10 in-demand skills to learn in 2019][link-17].

In the infamous words of Porky Pig, that's all folks! Feel free to [engage with me on Twitter][link-99] if you have any comments, questions, suggestions, corrections, or clarifications (or if you just want to chat!). I also welcome suggestions for content to include in future instances of Technology Short Take. Thank you for reading!

[link-1]: https://banzaicloud.com/blog/inject-secrets-into-pods-vault/
[link-2]: https://sysdig.com/blog/using-terraform-for-container-security-as-code/
[link-3]: https://containerjournal.com/2018/11/14/methods-to-audit-docker-container-security/
[link-4]: https://www.arcanoae.com/blue-lion/
[link-5]: https://binarynature.blogspot.com/2016/04/cisco-iosv-vagrant-box-for-vmware-fusion.html
[link-6]: https://scotch.io/tutorials/getting-started-with-azure-functions-using-vs-code-zero-to-deploy
[link-7]: https://blog.kylegalbraith.com/2018/06/28/getting-started-with-aws-up-and-running-with-elastic-beanstalk-in-minutes/
[link-8]: https://alestic.com/2018/12/aws-ssm-parameter-store-git-key/
[link-9]: http://cloud-abstract.com/service-mesh-is-just-another-form-of-virtualization/
[link-10]: https://tech.paulcz.net/blog/future-of-kubernetes-is-virtual-machines/
[link-11]: https://github.com/google/gvisor
[link-12]: https://github.com/cloudyuga/kubecon18-NA
[link-13]: https://github.com/Stretch96/terraform-aws-permissions-generator
[link-14]: https://aws.amazon.com/blogs/compute/run-your-kubernetes-workloads-on-amazon-ec2-spot-instances-with-amazon-eks/
[link-15]: https://jimswirelessworld.wordpress.com/2019/01/03/you-should-care-about-dhcp-option-51/
[link-16]: https://99u.adobe.com/articles/59994/the-new-rules-of-communicating-in-a-virtual-world
[link-17]: https://www.jpaul.me/2018/12/10-in-demand-skills-to-learn-in-2019/
[link-18]: https://fedoramagazine.org/managing-dotfiles-rcm/
[link-19]: https://vadosware.io/post/kicking-the-tires-on-openebs-for-cluster-storage/
[link-20]: https://www.mockingbirdconsulting.co.uk/blog/2019-01-05-hashicorp-at-home/
[link-21]: https://mattslifebytes.com/2019/01/06/cattle-not-pets-in-our-new-cloud-native-world/
[link-22]: https://www.trek10.com/blog/dynamodb-single-table-relational-modeling/
[link-23]: https://blogs.vmware.com/vsphere/2018/12/esxi-network-troubleshooting-tools.html
[link-24]: https://sflanders.net/2019/01/09/an-intro-to-ambassador/
[link-25]: https://triggermesh.com/2019/01/09/announcing-triggermesh-knative-lambda-runtime-klr/
[link-26]: https://www.awsgeek.com/
[link-27]: https://www.jeremydaly.com/newsletter/
[link-28]: https://lastweekinaws.com/
[link-29]: https://github.com/aws/aws-toolkit-vscode
[link-30]: https://blogs.msdn.microsoft.com/premier_developer/2018/12/26/why-you-should-consider-vs-code-for-your-kubernetes-docker-work/
[link-99]: https://twitter.com/scott_lowe
