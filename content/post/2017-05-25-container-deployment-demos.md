---
author: slowe
categories: Information
comments: true
date: 2017-05-25T00:00:00Z
tags:
- AWS
- Docker
- Google
- Kubernetes
- Terraform
- Ansible
- CLI
title: Container Deployment Demos from Interop ITX
url: /2017/05/25/container-deployment-demos/
---

At Interop ITX 2017 in Las Vegas, I had the privilege to lead a half-day workshop on options for deploying containers to cloud providers. As part of that workshop, I gave four live demos of using different deployment options. Those demos---along with the slides I used for my presentation along the way---are now available to anyone who might like to try them on their own.<!--more-->

The slides and all the resources for the demos are available in [this GitHub repository][link-6]. The four demos are:

1. _Docker Swarm on EC2:_ This demo leverages [Terraform][link-7] and [Ansible][link-8] to stand up and configure a [Docker][link-9] Swarm cluster on AWS.

2. _Amazon EC2 Container Service (ECS):_ This demo uses [AWS CloudFormation][link-10] to create an [EC2 Container Service][link-11] cluster with 3 instances and an Amazon RDS instance for backend database storage.

3. _Kubernetes on AWS using `kops`_: Using [the `kops` CLI tool][link-12], this demo turns up a [Kubernetes][link-13] cluster on AWS to show how to deploy containerized applications on Kubernetes.

4. _Google Container Engine:_ The final demo shows using [Google Container Engine][link-14]---which is Kubernetes---to deploy an application.

In the coming weeks, I plan to recreate the demos, record them, and publish them via YouTube, so that readers/viewers can see the demos in action. In the meantime, each of the demos has its own folder in the GitHub repository with instructions on how to go about recreating the demo on your own. I hope that making these resources available will be useful to readers.

To build these demos, I took inspiration and knowledge from a _huge_ variety of resources. Here are a few of the resources that I used:

* [Multi-Cloud Docker Swarm with Terraform and Ansible][link-1]
* [Terraform, Docker Swarm, and AWS][link-2]
* [Deploying Docker Swarm with Ansible][link-3]
* [Amazon EC2 Container Service Template Snippets][link-4]
* [Installing Kubernetes on AWS with kops][link-5]

I'm sure there are many more that I did not capture as I was researching, but these resources were certainly instrumental in building the demos. So---thanks to these authors for sharing their knowledge.



[link-1]: https://rsmitty.github.io/Multi-Cloud-Swarm/
[link-2]: http://ngerakines.me/2016/11/20/terraform_docker_swarm_and_aws/
[link-3]: http://thisendout.com/2016/09/13/deploying-docker-swarm-with-ansible/
[link-4]: http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-ecs.html
[link-5]: http://kubernetes.io/docs/getting-started-guides/kops/
[link-6]: https://github.com/scottslowe/2017-itx-container-workshop
[link-7]: https://www.terraform.io/
[link-8]: https://www.ansible.com/
[link-9]: https://www.docker.com/
[link-10]: https://aws.amazon.com/cloudformation/
[link-11]: https://aws.amazon.com/ecs/
[link-12]: https://github.com/kubernetes/kops
[link-13]: https://kubernetes.io/
[link-14]: https://cloud.google.com/container-engine/
