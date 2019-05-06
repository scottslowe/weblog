---
author: slowe
categories: Information
comments: true
date: 2019-05-05T19:00:00Z
tags:
- Pulumi
- Virtualization
- Vagrant
- AWS
- CLI
title: A Sandbox for Learning Pulumi
url: /2019/05/05/a-sandbox-for-learning-pulumi/
---

I recently started using [Pulumi][link-1], a way of using a general purpose programming language for infrastructure-as-code projects. I've been using Pulumi with JavaScript (I know, some folks would say I should question my life decisions), and while installing Pulumi itself is pretty low-impact (a small group of binaries) there are a number of dependencies that need to be installed when using Pulumi with JavaScript. As I'm a stickler for keeping my primary system very "clean" with regard to installed packages and software, I thought I'd create a means whereby I can easily spin up a "sandbox environment" for learning Pulumi.<!--more-->

When creating this sandbox environment, I turned to some tools that are very familiar:

* I used virtualization (a virtual machine) as the isolation mechanism. The next step is to use a Linux container, like a Docker container, as the isolation mechanism, but I thought I'd start with something a bit simpler at first.
* [Vagrant][link-4] provides a way of automating the creation/destruction of said VM. Again, Vagrant is well-understood and widely used.
* [Ansible][link-5] provides the automation to configure the VM with the necessary software (Pulumi and associated dependencies).
* I also thought that some folks might find it interesting or useful to be able to instantiate AWS instances from a preconfigured AMI, so I also included a [Packer][link-6] build file to enable folks to build their own Pulumi-ready AMI.

Since it seemed reasonable to think that others might find this useful as well, I placed all of this into [my GitHub "learning-tools" repository][link-2]; check the `pulumi` folder. Obviously, you'll need a virtualization provider (VMware Fusion, VirtualBox, and Libvirt are all supported), Vagrant, and Ansible installed. If you want to build your own AMI, you'll need Packer and the associated AWS tooling installed as well.

There's nothing terribly new or novel here; it uses tools and techniques that are already pretty well-known and well-understood by most folks. My goal here wasn't to create something entirely new, but rather to make it easier to learn Pulumi while minimizing the impact/affect on my Linux system.

To use the Vagrant environment I created, simply copy the directory (or clone the entire repository) and then run `vagrant up`. After Vagrant is finished, run `ansible-playbook configure.yml` to configure the VM for Pulumi, and then you're good to go.

In AWS, you could create a new AMI using Packer by running `packer build packer.json`, or create a new EC2 instance and run the `configure.yml` Ansible playbook against it.

To then make it easier to set up this Pulumi sandbox for various scenarios, I'll be creating scenario-specific playbooks that you can apply using `ansible-playbook`. There's only one there right now; it's for creating a single EC2 instance using Pulumi and JavaScript. Since I've chosen to focus my efforts around JavaScript, future scenarios will also be JavaScript-centric, so keep that in mind.

(By the way, all this information is in the README file in the `pulumi` directory of my "learning-tools" repository.)

## Additional Resources

For more background and context on Pulumi, you may want to check out [episode 30][link-3] of the Full Stack Journey podcast, which features Luke Hoban, CTO of Pulumi, discussing what Pulumi is and why users should consider using it for their infrastructure-as-code projects. Also, Joe Beda did a TGIK episode on Pulumi; [check it out on YouTube][link-7].

[link-1]: https://pulumi.io
[link-2]: https://github.com/scottslowe/learning-tools
[link-3]: https://packetpushers.net/podcast/full-stack-journey-030-building-cloud-native-infrastructure-as-code-with-pulumi/
[link-4]: https://www.vagrantup.com/
[link-5]: https://www.ansible.com/
[link-6]: https://www.packer.io/
[link-7]: https://www.youtube.com/watch?v=ILMK65YVSKw
