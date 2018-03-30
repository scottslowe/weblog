---
author: slowe
categories: Information
comments: true
date: 2016-12-09T00:00:00Z
tags:
- Networking
- Storage
- Security
- Virtualization
- AWS
- VMware
- vSphere
- OpenStack
- Ansible
- OVN
- Vagrant
- Ubuntu
- SSH
- Intel
- Linux
- Consul
- Docker
- NAS
- SAN
title: 'Technology Short Take #74'
url: /2016/12/09/technology-short-take-74/
---

Welcome to Technology Short Take #74! The end of 2016 is nearly upon us, and it looks as if there will be only one more Technology Short Take before the end of the year. So, let's get on with the content---time is short!

## Networking

* If you haven't heard of Apstra, David Varnum has a great [introduction to Apstra][link-7] available on his site.
* Will Robinson talks about [how to structure your Ansible playbooks][link-12] in the context of using Ansible to control your network gear.
* [This][link-17] is an interesting project to watch, I think---it's porting OVN (Open Virtual Network) from a "traditional" OvS back-end to an IOVisor-based back-end (IOVisor implements the data plane in eBPF).
* If you're interested in playing around with OVN, I've built a Vagrant-based environment running OVS/OVN 2.6.0 on Ubuntu 16.04. Have a look [here][link-32].

## Servers/Hardware

Nothing this time, but I'll stay alert for content to include in the future.

## Security

* Jake Bennett discusses [strengthening your AWS security posture][link-13] by protecting application credentials and rotating EC2 and IAM keys.
* Gert van Dijk has a great article on [upgrading your SSH keys][link-14] to take advantage of new key types and enhanced security.
* In case you're interested, [here's more details on CVE-2016-5195][link-15], aka "Dirty Cow," and how it might be used to allow a user to escape from a Linux container (like a Docker container).
* Intel has [a tutorial series on Intel Software Guard Extensions (SGX)][link-29]. It's a bit deep in the programming side for me, but developers interested in adding SGX capabilities should give this a look.

## Cloud Computing/Cloud Management

* Sirish Raghuram has [some thoughts][link-1] on the VMware-AWS announcement (about VMware Cloud on AWS) compared to the "Omni" announcement at the OpenStack Summit in Barcelona. Both announcements are related to AWS, but as Sirish points out they are quite different in nature. I understand Sirish's point regarding the difference between the two announcements, but I think there's one thing he's missing: Cross-Cloud Services.
* Yet another OpenStack project related to containers [has emerged][link-2]. I'm pretty clear about Kolla, Kuryr, and Magnum, but I'm not so clear about OpenStack Zun.
* Subbu Allamaraju makes his position pretty clear: [don't build private clouds][link-5].
* I noted that [a Vagrant provider for vRealize Automation][link-10] was recently released as an open source project. While it's early yet, it will be exciting to see how this continues to develop.
* The inimitable "Cloud Opinion" says [don't build private clouds][link-18].
* Patricia Johnson tries to tackle the task of [comparing AWS vs. Azure][link-25].
* JJ Asghar [announces][link-27] two Chef vRA 7.0+ Blueprints that demonstrate VMware-Chef integration.
* Ryan Kelly talks about to enable vRA 7.1's new scale out/in feature to [scale a Kubernetes deployment][link-28].

## Operating Systems/Applications

* In trying to better understand how Git credential helpers work, I stumbled across this article on [using both GitHub and AWS CodeCommit credential helpers together][link-4]. It turns out that you can "scope," or limit, the reach of a credential helper. That's handy.
* If you're an Ansible user, [here's one reason][link-8] you might want to hold off on Ubuntu 16.04.
* Consul is a HashiCorp tool I've written about before (see [my quick introduction to Consul][xref-1]), and I recently saw that the 0.7.1 release [added a CLI for interacting with Consul's key-value store][link-9]. The addition of a CLI may make integrating with Consul easier in some situations.
* And while we are talking about Consul...check out two articles I recently found by Flynn Bundy. First, we have an article on [getting to know Consul][link-19]. Next, Flynn shows [how to use Consul and Python to build dynamic inventories for Ansible][link-20]. Good stuff.
* Larry Smith Jr. has an article on [using Ansible to provision a Docker cluster in swarm mode][link-23].
* Here's a piece by Emmet O'Grady (of NimbleCI) talking about the [top 10 new features coming in Docker 1.13][link-21].
* Thomas Maurer has a write-up [looking at Windows Nano Server][link-26].

## Storage

* J Metz recently published an article that helps understand [when to use NAS (Network Attached Storage) versus SAN (Storage Area Network)][link-11]. Although the letters in both acronyms are the same, they are different and (typically) have different use cases.

## Virtualization

* William Lam is back with more nested ESXi goodness, this time discussing [virtual NVMe support for nested ESXi 6.5][link-3].
* This post by Brett Sinclair on [using PowerCLI to manipulate DRS rules][link-6] was a bit of a "blast from the past" for me---I remember discussing stretched clusters, site bias, detach rules, etc., right after the introduction of EMC VPLEX in 2010. Good times.
* Rawlinson Rivera has [a post][link-31] on using [Chaperone][link-30], an Ansible-based deployment tool, to deploy the VMware SDDC stack (vSphere, vSAN, NSX, and VIO).

## Career/Soft Skills

* Ben Fathi, former CTO of VMware, offers [some advice][link-16] to folks unsure about whether they should stay or leave.
* Phocean took my "Reducing the Friction" series and decided to add to it, talking about [streamlining keeping up with technology][link-22] using Netvibes.
* Drew Firment has [a great article][link-24] expanding on Simon Wardley's "pioneer-settler-town planner" trimodal approach (which is, in turn, based on Cringely's "commandos-infantry-police" model). Like Drew, I tend to think I fit firmly in the "settler" category. What about you?

That's all for this time. The next Technology Short Take will be published on Friday, December 30, and will be the last Technology Short Take of 2016. Until then, Happy Holidays!



[link-1]: https://www.linkedin.com/pulse/hybrid-clouds-comparing-openstack-vmwares-divergent-aws-raghuram-1
[link-2]: http://www.internetnews.com/blog/skerner/openstack-zun-debuts-new-approach-to-cloud-containers.html
[link-3]: http://www.virtuallyghetto.com/2016/10/virtual-nvme-and-nested-esxi-6-5.html
[link-4]: http://jameswing.net/aws/using-codecommit-and-git-credentials.html
[link-5]: https://m.subbu.org/dont-build-private-clouds-9a54b3d30c8b#.sw03p6tee
[link-6]: http://www.pragmaticio.com/2016/11/vmware-drs-rule-manipulation-using-powercli-emc-vplex-use-case/
[link-7]: https://overlaid.net/2016/11/23/apstra-intends-greatness-beyond-sparta/
[link-8]: https://fuzzygroup.github.io/blog/aws/2016/11/29/aws-tech-note-problems-with-ubuntu-16-04-and-ansible.html
[link-9]: https://www.hashicorp.com/blog/consul-kv-cli.html
[link-10]: https://github.com/sky-uk/vagrant-vrealize
[link-11]: https://jmetz.com/2016/12/storage-basics-when-to-use-san-v-nas/
[link-12]: http://www.oznetnerd.com/2016/11/27/ansible-playbook-structure/
[link-13]: http://www.randomant.net/strengthen-your-aws-security-by-protecting-app-credentials-and-automating-ec2-and-iam-key-rotation/
[link-14]: https://blog.g3rt.nl/upgrade-your-ssh-keys.html
[link-15]: https://blog.paranoidsoftware.com/dirty-cow-cve-2016-5195-docker-container-escape/
[link-16]: http://benbobsworld.blogspot.com/2016/11/should-i-stay-or-should-i-go-now-and.html
[link-17]: https://github.com/netgroup-polito/iovisor-ovn
[link-18]: https://medium.com/@cloud_opinion/dont-build-private-clouds-b24b9d51f75b#.4ckgm3ijn
[link-19]: https://flynnbundy.com/service-discovery/2016/11/26/getting-to-know-consul.html
[link-20]: https://flynnbundy.com/ansible/2016/12/04/dynamic-inventory-with-consul-and-ansible.html
[link-21]: https://blog.nimbleci.com/2016/11/17/whats-coming-in-docker-1-13/
[link-22]: https://phocean.net/2016/11/27/reducing-the-friction-with-social-media-thanks-to-netvibes.html
[link-23]: http://everythingshouldbevirtual.com/ansible-provision-docker-swarm-mode-1-12-cluster
[link-24]: https://cloudrumblings.io/a-pioneer-a-settler-and-a-town-planner-walk-into-a-bar-9889d7c8a19e#.muinrdjiu
[link-25]: http://www.whitesourcesoftware.com/whitesource-blog/aws-vs-azure/
[link-26]: http://www.thomasmaurer.ch/2016/11/nano-server-the-future-of-windows-server-just-enough-os/
[link-27]: https://blog.chef.io/2016/11/29/vmware-vrealize-automation-7-0-and-chef/
[link-28]: http://www.vmtocloud.com/how-to-configure-the-kubernetes-blueprint-to-scale-out-with-vra-7-1/
[link-29]: https://software.intel.com/en-us/articles/introducing-the-intel-software-guard-extensions-tutorial-series
[link-30]: https://github.com/vmware/chaperone
[link-31]: http://www.punchingclouds.com/2016/11/29/hci-automated-deployment-configuration-vsphere-vsan-nsx-vio-the-devops-way/
[link-32]: https://github.com/scottslowe/learning-tools/tree/master/ovn
[xref-1]: {{< relref "2015-02-06-quick-intro-to-consul.md" >}}
