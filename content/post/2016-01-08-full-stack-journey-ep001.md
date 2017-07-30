---
author: slowe
categories: Podcast
comments: true
date: 2016-01-08T00:00:00Z
tags:
- Career
- Cloud
- Virtualization
- Linux
- AWS
- Podcast
title: 'Full Stack Journey Episode #1: Bart Smith'
url: /2016/01/08/full-stack-journey-ep001/
---

In this first-ever episode of the Full Stack Journey podcast, I talk with Bart Smith ([old GitHub account][link-1] migrating to [new GitHub account][link-2], [YouTube channel][link-3]). Bart shares some details about his journey from being a Microsoft-centric infrastructure engineer to what he calls a cloud-native full-stack engineer. Here are some notes from our conversation, along with some additional resources Bart wanted to share with readers/listeners. Enjoy!

The podcast audio recording is [available on Soundcloud][link-32].

## Show Notes

* His journey started in June 2014 as a result of the Microsoft announcement regarding support for Linux and Kubernetes on Azure---this really indicated a shift in the industry.
* Bart's view is that a full-stack engineer knows about operations, the hardware stack (compute, storage, network), the software (network, operating system [OS], management, logging), and most importantly knows how to "code" an immutable infrastructure. An operations full-stack engineer can read code, work with developers, and be part of a DevOps team of support DevOps teams in deploying code into production both to on-premises solutions and off-premises solutions.
* IT folks don't need to be strictly involved in software engineering to benefit from a journey toward a more full-stack role.
* His journey from Microsoft-centric engineer to cloud-native engineer encompasses learning the following areas:
    - Getting a good understanding of microservices (recommends book _Building Microservices_ by Sam Newman; [here's a link][link-5])
    - Container technologies ([Docker][link-15], most notably, but also [CoreOS][link-16] and [etcd][link-17]); recommends [this Docker course][link-4]
    - PaaS/container orchestration ([Mesos][link-18]/[Mesosphere][link-19], [OpenShift][link-20], Docker Swarm, [Kubernetes][link-21])
    - AWS (10x the size of all competitors), Joyent
    - [Microsoft][link-29] and/or [VMware][link-30], but _do not start here_ (start elsewhere)
    - [Otto][link-22] and [Terraform][link-23] by Hashicorp
    - [Ansible][link-26] (not Chef, Puppet, or Salt)
    - [Cassandra][link-24] and [Neo4j][link-25]
    - [Git][link-27]
    - VXLAN
* He highly recommends using OS X on Apple hardware (he bought a used MacBook Pro, including SSD, for less than 300 euros).
* Safari Books Online ([link][link-9]) a great resource, highly recommended.
* Bart believes attending meetups, if available in your area, is a valuable source of information.
* Attending relevant conferences, digging into the material, asking questions, and meeting people has also been very helpful.
* After attending a meetup or conference, be sure to give yourself a "call to action"---actually _do_ the thing about which you're learning.
* Bart recorded meetups so that he could review them later, multiple times if necessary---"repeat to remember, remember to repeat" has been a key learning strategy for him.
* Certifications may not be as valuable as they used to be (although Red Hat might be worthwhile); a lot of this learning is going to be something you're going to create/shape yourself.
* Sharing the information you've learned, or information you're creating as you learn, helps you and helps the community. [GitHub][link-28] is a great way to share.
* There should probably be more graphics and drawings!
* [DevOps Enterprise Summit][link-31] would be a good conference to attend (he couldn't make it this past year).
* Looking ahead, Bart is really intrigued by unikernels ([MirageOS][link-10] in particular).

## List of Resources

Here are some resources that were either mentioned during the podcast, or provided by Bart separately.

Docker course: [http://nkhare.github.io/data_and_network_containers/Docker_Introduction/][link-4]

_Building Microservices_ by Sam Newman: [https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/][link-5]

_Pro Git, Second Edition_ by Ben Straub and Scott Chacon: [https://www.safaribooksonline.com/library/view/pro-git-second/9781484200766/][link-6] (also available [here][link-7] or [here][link-8])

[Safari Books Online][link-9]

Trustworthy secure modular operating system engineering: [https://media.ccc.de/v/31c3_-_6443_-_en_-_saal_2_-_201412271245_-_trustworthy_secure_modular_operating_system_engineering_-_hannes_-_david_kaloper#video][link-11]

MirageOS videos from around the world: [https://mirage.io/blog/videos-around-world-2015][link-12]

Unikernel Screencasts: [https://www.youtube.com/playlist?list=PLmqXqqjZSStnG33E_865NgHONETzM8kVb][link-13]

Why are computers so @#!*, and what can we do about it?: [https://media.ccc.de/v/31c3_-_6574_-_en_-_saal_1_-_201412301245_-_why_are_computers_so_and_what_can_we_do_about_it_-_peter_sewell#video][link-14]


[link-1]: https://github.com/smartbitnl
[link-2]: https://github.com/smartbit
[link-3]: https://www.youtube.com/user/TheSmartbit
[link-4]: http://nkhare.github.io/data_and_network_containers/Docker_Introduction/
[link-5]: https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/
[link-6]: https://www.safaribooksonline.com/library/view/pro-git-second/9781484200766/
[link-7]: https://git-scm.com/book/en/v2
[link-8]: https://progit.org
[link-9]: https://www.safaribooksonline.com
[link-10]: https://mirage.io
[link-11]: https://media.ccc.de/v/31c3_-_6443_-_en_-_saal_2_-_201412271245_-_trustworthy_secure_modular_operating_system_engineering_-_hannes_-_david_kaloper#video
[link-12]: https://mirage.io/blog/videos-around-world-2015
[link-13]: https://www.youtube.com/playlist?list=PLmqXqqjZSStnG33E_865NgHONETzM8kVb
[link-14]: https://media.ccc.de/v/31c3_-_6574_-_en_-_saal_1_-_201412301245_-_why_are_computers_so_and_what_can_we_do_about_it_-_peter_sewell#video
[link-15]: http://www.docker.com/
[link-16]: https://coreos.com/
[link-17]: https://github.com/coreos/etcd
[link-18]: https://mesos.apache.org/
[link-19]: https://mesosphere.com/
[link-20]: https://openshift.com/
[link-21]: http://kubernetes.io/
[link-22]: https://www.ottoproject.io/
[link-23]: https://terraform.io/
[link-24]: https://cassandra.apache.org/
[link-25]: http://neo4j.com/
[link-26]: http://www.ansible.com/
[link-27]: https://git-scm.com/
[link-28]: https://github.com
[link-29]: http://www.microsoft.com/en-us/
[link-30]: http://www.vmware.com/
[link-31]: http://devopsenterprise.io
[link-32]: https://soundcloud.com/fullstackjourney/full-stack-journey-episode-001
