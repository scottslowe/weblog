---
author: slowe
categories: Information
comments: true
date: 2018-06-08T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Firefox
- Kubernetes
- AWS
- JSON
- vSphere
- Encryption
- Docker
- Juniper
- Ansible
- Automation
- Google
title: 'Technology Short Take 101'
url: /2018/06/08/technology-short-take-101/
---

Welcome to Technology Short Take #101! I have (hopefully) crafted an interesting and varied collection of links for you today, spanning all the major areas of modern data center technology. Now you have some reading material for this weekend!<!--more-->

## Networking

* Kirk Byers [discusses the direct TextFSM integration][link-3] he added to the 2.0 release of Netmiko.
* David Gee [provides a walkthrough][link-19] on creating a self-signed root certificate for use with mutual SSL for gRPC on JunOS. (How's that for a mouthful!) If you're a networking pro and didn't understand most of that last sentence, I'd content that you've got some learning to do. The networking industry is changing, don't get left behind!
* Here's another one from David: [running Juniper vQFX10K on ESXi 6.5][link-23]. (Personally, I'd love to see the KVM option explained.)
* Kamal Kyrala discusses [a method for accessing Kubernetes Services without Ingress, NodePort, or load balancers][link-20]. Why is this in the networking section? Because it involves IP routing and ECMP!
* On the Ansible web site, Sean Cavanaugh provides [a deep dive][link-21] into the `command` modules for network devices.

## Servers/Hardware

* AWS adds local NVMe storage to the M5 instance family; more details [here][link-15]. What I found interesting is that the local NVMe storage is also hardware encrypted. AWS also mentions that these M5d instances are powered by (in their words) "Custom Intel Xeon Platinum" processors, which just goes to confirm the long-known fact that AWS is leveraging custom Intel CPUs in their stuff (as are all the major cloud providers, I'm sure).

## Security

* Daniel Steinberg [takes a look inside Firefox's DOH (DNS over HTTPS) engine][link-7], slated to be included in the Firefox 62 release.
* Guy Maliar has a detailed post describing [dynamic secrets on Kubernetes pods using HashiCorp Vault][link-9]. Along the way, just for fun, he also throws init containers into the mix as well. Good stuff here.

## Cloud Computing/Cloud Management

* Amazon EKS (Elastic Container Service for Kubernetes) [was made generally available this past week][link-4]. I plan to take a closer look at EKS very soon, and will share some feedback when I am able to do so.
* Jason Plum of GitLab shares [five things he wishes he'd known about Kubernetes before he started][link-11].
* Co-worker Ruben Orduz takes a (potentially controversial) stance and says that [you don't need an HA control plane for Kubernetes][link-12]. His points are spot-on: making the control plane highly available isn't going to improve the availability or scalability of your applications.

## Operating Systems/Applications

* Speaking of EKS, here's [a new command-line interface for EKS][link-1], courtesy of Weaveworks.
* Along with the GA of EKS, HashiCorp has a release of the Terraform AWS provider that has EKS support. More details are available [here][link-5].
* Google recently [announced `kustomize`][link-6], a tool that provides a new approach to customizing Kubernetes object configuration.
* Following after [a recent post][xref-1] involving parsing AWS instance data with `jq`, a Twitter follower pointed out [`jid` (the JSON incremental digger)][link-8]. Handy tool!
* I'm seeing a fair amount of attention on `podman`, a tool primarily backed by Red Hat that aims to replace Docker as the client-side tool of choice. The latest was [this post][link-10]. Anyone else spent any quality time with this tool and have some feedback?
* Nick Janetakis has a collection of quick "Docker tips" that you may find useful; the latest one shows [how to see all your container's environment variables][link-22].

## Storage

* This is an interesting announcement from a few weeks ago that I missed---Dell EMC will offer Isilon on Google Cloud Platform. See Chris Evans' article [here][link-18]. (I'll leave the analysis and pontificating to Chris, who's much better at it than I am.)

## Virtualization

* Mike Foley [shares his experience][link-2] in updating his vSphere hosts to support the new Secure Boot functionality in vSphere 6.7. I'd stay tuned to Mike's blog; he's indicated that some TPM 2.0-related content is due soon.
* Ed Haletky discusses [some considerations around upgrading to vSphere 6.7][link-13].
* VMware rock star Frank Denneman provides [an introduction to Elastic DRS][link-14], a feature of VMware Cloud on AWS.

## Career/Soft Skills

* I found this two-part series ([part 1][link-16], [part 2][link-17]) on understanding how to process information to help you get organized to be an interesting (and quick) read. It's been my experience that improving your skills at being organized often reaps benefits in other areas.

OK, that's all this time around. I hope you found something useful in this post. As always, your feedback is welcome; feel free to [hit me up on Twitter][link-24].

[link-1]: https://eksctl.io/
[link-2]: https://www.yelof.com/2018/06/06/prepping-an-esxi-6-7-host-for-secure-boot/
[link-3]: https://pynet.twb-tech.com/blog/automation/netmiko-textfsm.html
[link-4]: https://aws.amazon.com/blogs/aws/amazon-eks-now-generally-available/
[link-5]: https://www.hashicorp.com/blog/hashicorp-announces-terraform-support-aws-kubernetes
[link-6]: https://kubernetes.io/blog/2018/05/29/introducing-kustomize-template-free-configuration-customization-for-kubernetes/
[link-7]: https://daniel.haxx.se/blog/2018/06/03/inside-firefoxs-doh-engine/
[link-8]: https://github.com/simeji/jid
[link-9]: https://medium.com/@gmaliar/dynamic-secrets-on-kubernetes-pods-using-vault-35d9094d169
[link-10]: https://medium.com/cri-o/how-good-is-podman-eecedbbf71f
[link-11]: https://about.gitlab.com/2018/04/16/five-things-i-wish-i-knew-about-kubernetes/
[link-12]: https://medium.com/@walloffire/you-dont-need-a-ha-control-plane-3af9da8e556d
[link-13]: https://www.astroarch.com/2018/06/vsphere-upgrade-saga-prepping-for-vsphere-6-7/
[link-14]: http://frankdenneman.nl/2018/06/07/introduction-elastic-drs/
[link-15]: https://aws.amazon.com/blogs/aws/ec2-instance-update-m5-instances-with-local-nvme-storage-m5d/
[link-16]: https://unclutterer.com/2018/05/24/understanding-how-you-process-information-to-help-you-get-organized-part-i/
[link-17]: https://unclutterer.com/2018/05/25/understanding-how-you-process-information-to-help-you-get-organized-part-2/
[link-18]: https://blog.architecting.it/dell-emc-isilon-gcp/
[link-19]: http://ipengineer.net/2018/05/configuring-ssl-grpc-junos/
[link-20]: https://medium.com/@kyralak/accessing-kubernetes-services-without-ingress-nodeport-or-loadbalancer-de6061b42d72
[link-21]: https://www.ansible.com/blog/command-module-deep-dive-for-networks
[link-22]: https://nickjanetakis.com/blog/docker-tip-58-output-all-of-your-containers-env-variables
[link-23]: http://ipengineer.net/2018/06/juniper-vqfx10k-esxi-6-5/
[link-24]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-05-23-quick-post-parsing-aws-instance-data-with-jq.md" >}}
