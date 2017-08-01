---
author: slowe
categories: Information
comments: true
date: 2016-09-09T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VXLAN
- Intel
- Windows
- Microsoft
- HyperV
- OpenStack
- Ansible
- Docker
- Linux
- CoreOS
- RedHat
- VMware
title: 'Technology Short Take #71'
url: /2016/09/09/technology-short-take-71/
---

Welcome to Technology Short Take #71! As always, I have a list of links related to various data center technologies found below; hopefully something here proves useful.

## Networking

* Dmitri Kalintsev is back with another article in a series of articles on using hardware VXLAN tunnel endpoints (VTEPs) with VMware NSX. In this case, it's on [validating HW VTEP deployment for integration with NSX][link-22]. In particular, Dmitri points out that you need connectivity _both_ on the management plane as well as on the data plane.
* Here's a handy post by Dale Coghlan on [how to find object IDs for just about anything][link-30] in a VMware NSX environment.

## Servers/Hardware

* Intel has finally started shipping silicon photonics gear, according to [this report from El Reg][link-8] (news coming out of IDF16). A more detailed analysis of silicon photonics can be had [over at The Next Platform][link-9].

## Security

* The use of VMware NSX for microsegmentation is a really popular use case, and so the topic of scripting distributed firewall (DFW) rules often comes up. Dale Coghlan has an article on [bulk DFW rule creation][link-1] that might be helpful in such instances.
* This blog post on [shielded VMs in Windows Server 2016][link-2] is a bit light on details, but the embedded video may be more informative (I didn't watch it).
* I predicted a couple of years ago that Intel SGX (Software Guard Extensions) was going to be HUGE (see [here][xref-1]). I recently saw that at Intel Developer Forum (IDF) Bromium showed off an SGX-enabled prototype that uses SGX to protect sensitive data. This is just the beginning---it's going to end up going a lot farther, I believe. See [this blog post][link-7] by Simon Crosby for more details.

## Cloud Computing/Cloud Management

* Dmitri Kalintsev tackles the subject of [infrastructure as code and why you should care][link-5]. It's a good intro to the topic of infrastructure as code (if this is new to you), and I appreciate the fact that Dmitri also took the time to provide reasons why it's valuable/important to IT professionals. So many times you'll see various technologies or trends discussed, but the author won't take the time to explain _why_ it is being discussed.
* Pradipta Kumar Banerjee has an article on [using local storage for instances in OpenStack][link-6]. Specifically, the article discusses the use of Cinder volumes provided by disks in the compute nodes, so a compute node is both a compute node as well as a Cinder volume node.
* Chris Smart has a thorough article on [setting up OpenStack Ansible all-in-one behind a proxy][link-16].
* While a _user_ of a private cloud shouldn't have to worry about the details on how the cloud operates or is built, that's not true for the _architect_ of a private cloud. This seems obvious, I know, but I do believe that this fact is often overlooked in the all-too-common refrains of "OpenStack is too complicated!" (This is not to say that sentiment isn't true in some areas, by the way.) In any case, have a look at this article by Julio Villareal Pelegrino on [architecting an OpenStack cloud][link-18].
* Speaking of OpenStack complexity...I came across [this article on YAQL expressions][link-19] (thanks to the kind folks at Mirantis for the link in their "OpenStack Unlocked" newsletter). Now, I'm likely to take some heat (Ha! No pun intended!) for this, but I have to ask---is this _really_ the sort of thing that should be exposed to cloud consumers? Maybe it's just me, but it seems that we (the OpenStack community) should be a bit more focused on usability than on adding more nerd knobs. Then again, what do I know?

## Operating Systems/Applications

* Red Hat Enterprise Linux Atomic Host (how's that for a mouthful?) recently announced version 7.2.6, and it includes the ability to add packages directly to Atomic Host. This is a fairly significant departure from a lot of other container-optimized Linux hosts, which require you to leverage containers for anything not included in the base distribution. See this [Red Hat blog][link-4] has more details on the new "package layering" functionality.
* VMware vRealize Log Insight (known as vRLI by its friends) [now has a MongoDB content pack][link-10].
* CoreOS has [extended their online validator][link-12] to validate both Ignition configurations (Ignition is their tool aimed at machine provisioning) as well as cloud-config configurations.
* Commenting on the ruckus that arose out of a Twitter conversation between [Kelsey Hightower][link-13] and [Solomon Hykes][link-14], Matt Asay strikes a bit of a different approach on the topic of container standardization in [this InfoWorld article][link-15]. On the face of it, I agree with Matt's statement that "design by committee" tends not to work well; however, I also believe that there needs to be some reasonable counterbalance to a single (commercial) entity's control over an open source project.
* Is a simple `DOCKERFILE` a myth, a figment of the imagination? The more I read about creating highly-optimized Docker images, the more I believe this is the case. The latest example is from [this article][link-17] by Denny Zhang.
* Ajeet Singh Raina has a simple example of how [service discovery works in Docker 1.12][link-20]. (Nice use of Docker Compose in helping illustrate how everything works.)
* Ryan Brown disputes the Docker claim (from DockerCon) that "Docker is serverless" in [this article][link-21]. While I agree with some of Ryan's points, I have to wonder if the disagreement isn't just a matter of different perspectives. I mean, _any_ Function-as-a-Service (FaaS) offering has to have some sort of runtime underneath; who's to say it isn't some form of Linux containers? (See the introductory text [here][link-23].) At the same time, exposing the "plumbing" behind a FaaS offering does "violate" the fundamental concepts of FaaS. Hence, it's really a matter of just different perspectives.
* [DVM looks handy][link-24].

## Storage

* A "seething cauldron of technology development" is how [this article][link-27] by Chris Mellor at The Register described the space around NVDIMM, NVMe, XPoint, ReRAM, and DRAM-SSD. (So many new terms! It's obvious I haven't paid a great deal of attention to the storage space recently.)
* If you're seeking resources related to NVMe, [this NVMe bibliography][link-28] (by J Metz) is a great resource. Not sure where to start? No problem---J has you covered with [this NVMe program of study][link-29], too.

## Virtualization

* Steve Flanders has made available [an ESXi importer manifest for Log Insight][link-3].
* Dale Carter [outlines new clustering requirements][link-11] for VMware Identity Manager in version 2.7.
* VMworld recently happened (in case you hadn't noticed), and this year VMware is opening up access to all the VMworld content, even if you didn't attend the show. (It's about time.) William Lam has a list of direct playback URLs to make the process of accessing the content easier. Read William's post [here][link-31] (and get the URLs [here][link-32]).

## Career/Soft Skills

* While reading his article on [remote site replicated Docker registries][link-25], I was intrigued by Ryan Kelly's [guide on how to find a quiet place where you will not be interrupted][link-26]. In today's society, multi-tasking is praised; however, I find that it's far more effective to manage my focus and my time instead of jumping from task to task. (Your mileage may vary, of course.)

I'd love to keep going, but I suppose I'd better wrap it up before this post gets too long! Don't worry---if this post wasn't enough, I'll have another Technology Short Take up for you in just a couple weeks.



[link-1]: http://www.sneaku.com/2015/08/27/scripting-nsx-v-bulk-dfw-rule-creation/
[link-2]: https://blogs.technet.microsoft.com/windowsserver/2016/05/10/a-closer-look-at-shielded-vms-in-windows-server-2016/
[link-3]: http://sflanders.net/2016/07/17/log-insight-converting-agent-groups-manifests
[link-4]: http://rhelblog.redhat.com/2016/08/10/announcing-red-hat-enterprise-linux-atomic-host-7-2-6/
[link-5]: https://telecomoccasionally.wordpress.com/2016/08/02/what-is-infrastructure-as-code-and-why-you-should-care/
[link-6]: http://cloudgeekz.com/71/how-to-setup-openstack-to-use-local-disks-for-instances.html
[link-7]: https://blogs.bromium.com/2016/08/09/using-intel-sgx-to-protect-on-line-credentials/
[link-8]: http://www.theregister.co.uk/2016/08/17/intel_silicon_photonics/
[link-9]: http://www.nextplatform.com/2016/01/21/light-at-the-end-of-the-silicon-photonics-tunnel/
[link-10]: https://blogs.vmware.com/management/2016/08/mongodb-content-pack-for-vrealize-log-insight.html
[link-11]: https://vdelboysview.com/2016/08/17/new-requirement-for-vmware-identity-manager-when-clustering/
[link-12]: https://coreos.com/blog/validator-supports-ignition.html
[link-13]: https://twitter.com/kelseyhightower
[link-14]: https://twitter.com/solomonstre/
[link-15]: http://www.infoworld.com/article/3105857/open-source-tools/save-the-whale-docker-rightfully-shuns-standardization.html
[link-16]: https://blog.christophersmart.com/2016/08/09/setting-up-openstack-ansible-all-in-one-behind-a-proxy/
[link-17]: https://dzone.com/articles/5-tips-for-building-docker-image
[link-18]: http://www.juliosblog.com/architecting-your-first-openstack-cloud/
[link-19]: http://blog.oddbit.com/2016/08/11/exploring-yaql-expressions/
[link-20]: http://collabnix.com/archives/1545
[link-21]: https://serverlesscode.com/post/docker-isnt-serverless/
[link-22]: https://telecomoccasionally.wordpress.com/2016/08/18/validating-hw-vtep-deployment-for-integration-with-nsx-v/
[link-23]: http://martinfowler.com/articles/serverless.html
[link-24]: https://getcarina.com/blog/docker-version-manager/
[link-25]: http://www.vmtocloud.com/remote-site-replicated-docker-registries-with-vmware-harbor/
[link-26]: http://www.vmtocloud.com/how-to-find-a-quite-place-where-you-will-not-be-interrupted/
[link-27]: http://www.theregister.co.uk/2016/08/19/persistent_memory_over_fabrics/
[link-28]: https://jmetz.com/2016/08/a-nvme-bibliography/
[link-29]: https://jmetz.com/2016/08/learning-nvme-a-program-of-study/
[link-30]: http://www.sneaku.com/2016/07/13/how-to-find-object-ids-for-almost-everything/
[link-31]: http://www.virtuallyghetto.com/2016/09/direct-playback-urls-for-all-vmworld-us-2016-sessions.html
[link-32]: https://github.com/lamw/vmworld2016-session-urls
[xref-1]: {{< relref "2014-10-08-technology-short-take-45.md" >}}
