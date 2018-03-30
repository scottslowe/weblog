---
author: slowe
categories: Tutorial
comments: true
date: 2016-10-23T00:00:00Z
tags:
- AWS
- CLI
- Ansible
title: Managing AWS Infrastructure with Ansible
url: /2016/10/23/managing-aws-infrastructure-ansible/
---

In this post, I'm going to discuss some concepts behind managing your [Amazon Web Services (AWS)][link-1] infrastructure using [Ansible][link-2]. Ansible is a very popular tool for configuring operating system instances and software; using the concepts and examples provided in this post would allow you to expand your use of Ansible to include---when using AWS---the creation and deletion of the operating system instances themselves, as well as related infrastructure components (like security groups or other services).

## Preface

Before I continue, I'd like to first discuss the "fit" of using Ansible for this particular purpose. Ansible doesn't store the state of managed systems. Perhaps this is due to the agentless architecture; I don't know. What that means in this particular use case is that you must take other steps to store information you'll absolutely need like instance IDs, security group IDs, and the like because Ansible itself doesn't. In my mind, this makes Ansible a less-than-ideal tool for this particular use case. That doesn't mean Ansible isn't a good tool; it just means that Ansible may not be the _best_ tool for this particular purpose. (Think of it like this: Yes, you can sometimes unscrew something using a knife, but a screwdriver is typically much more efficient.)

With that caveat out of the way, let's dig in.

## Required Components

To make this work, you'll need the following items installed on whatever system will execute the Ansible playbooks:

* Ansible itself (obviously; I tested using Ansible 2.1.1.0)
* The `ec2.py` dynamic inventory script, along with the accompanying `ec2.ini` configuration file; both are available [here on GitHub][link-4] as of the writing of this blog post)
* The "boto" Python library, which you can easily install via `pip` (if you are using a virtualenv for Ansible [like me][xref-1], be sure to install into the same virtualenv as Ansible)

## Configuring the Components

The system on which I did my testing also had [the AWS CLI][link-5] installed, and was able to use the information there for AWS authentication. Some of the blog posts I read indicated a need to configure the "boto" library; I can't confirm personally whether that is necessary or not. I _can_ confirm that if the AWS CLI is working, then the "boto" library should work without any additional work.

As for Ansible itself, I'm a fan of using per-project Ansible configuration files, so I placed an `ansible.cfg` in the directory where my playbooks resided. This configuration file did only one thing: it set the location of the Ansible inventory. I did this for two reasons:

1. It makes running the various `ansible-*` commands easier because you don't have to specify the `-i` parameter (usually; you'll see an exception later in this article).

2. It allowed me to address the issue of running Ansible in a Python virtualenv. If you do run Ansible in a Python virtualenv, then the default Python interpreter (which defaults to `/usr/bin/python` on my OS X system) won't have Ansible or the "boto" library installed (because these are in a virtualenv). However, a very quick and simply change to the inventory file fixes this:

        [local]
        localhost ansible_python_interpreter=python

    Making this simple change means that Ansible picks up whatever Python interpreter is in the path, thus picking up the Python from the virtualenv and working as expected. Nifty trick, right? (Not mine---I got it from [here][link-7].)

The other piece of configuration required is configuring the `ec2.py` dynamic inventory script. This script has a configuration file, `ec2.ini`, that it expects in the same directory. For me, I only needed to change the `regions=` line in this file to point to "us-west-2" (where I run my infrastructure). Obviously, configure this as appropriate for your use of AWS.

Once the configuration is done, you're ready to proceed with using the playbooks.

## Provisioning AWS Infrastructure

Provisioning (creating) AWS infrastructure is pretty straightforward. Here's a sample Ansible playbook (some values have been changed to protect the innocent):

``` yaml
---
- hosts: "localhost"
  connection: "local"
  gather_facts: false
  vars:
    ami: "<insert AMI value here>"
    region: "<insert AWS region name>"
    type: "t2.micro"
    sshkey: "<insert keypair name>"
    vpcid: "<insert VPC ID>"

  tasks:
  - name: "Create a new security group"
    ec2_group:
      name: "ansible-sec-group"
      description: "New SG for Ansible-created instances"
      region: "{{ region }}"
      vpc_id: "{{ vpcid }}"
      rules:
        - proto: "tcp"
          from_port: 22
          to_port: 22
          cidr_ip: "0.0.0.0/0"
      rules_egress:
        - proto: "all"
          cidr_ip: "0.0.0.0/0"
    register: secgrp

  - name: "Provision an EC2 instance"
    ec2:
      key_name: "{{ sshkey }}"
      group_id: "{{ secgrp.group_id }}"
      instance_type: "{{ type }}"
      ec2_region: "{{ region }}"
      image: "{{ ami }}"
      wait: true
      count: 1
      instance_tags:
        tool: "ansible"
        env: "test"
    register: ec2instance
```

This playbook is, hopefully, pretty straightforward, but let's walk through it nevertheless:

* The Ansible EC2 module works against the local system running Ansible, so first we set that at the top of the YAML file.
* Next, we define some variables that are used later. You'll want to customize these for your particular usage of AWS.
* Next, we create a new security group. This will hold our instances. We register the result of that operation into a variable named `secgrp`, which is used later in the playbook. (Note that you could use this same approach to create a new VPC and then create the security group in that VPC.)
* Finally, we provision an EC2 instance using the specified AMI, instance type, region, SSH keypair, and security group (which references the security group created earlier). We also assign some AWS tags; these are _essential_ for working with resources on AWS. Finally, although we don't use it in this playbook, we register the result of this task into a variable named `ec2instance`.

To run this playbook, you'd switch into the directory where the playbook is stored and simply run `ansible-playbook create.yml`. Boom! Within a couple of minutes you'll have a new instance running on AWS.

Nice---but what about tearing down infrastructure? That's the more challenging part, in my opinion.

## Destroying AWS Infrastructure

Lots and lots of articles showed examples of how to create (provision) AWS infrastructure elements using Ansible; not very many showed how to _destroy_ that same infrastructure with Ansible. I consider the ability to (relatively) easy tear down infrastructure to be a core part of managing infrastructure with AWS, and therefore wanted to be sure to include that in this article.

To tear down infrastructure, you'll need to leverage the `ec2.py` dynamic inventory script. This will query the AWS APIs to retrieve information about the instances, and Ansible can then leverage this to delete instances. Here's a playbook that tears down the infrastructure created using the playbook from the previous section:

``` yaml
---
- hosts: "tag_tool_ansible"
  connection: "local"
  gather_facts: false
  vars:
    region: "us-west-2"

  tasks:
  - name: "Remove tagged EC2 instances from security group"
    ec2:
      state: "running"
      region: "{{ region }}"
      instance_ids: "{{ ec2_id }}"
      group_id: ""
    delegate_to: "localhost"
      
  - name: "Terminate tagged EC2 instances"
    ec2:
      state: "absent"
      region: "{{ region }}"
      instance_ids: "{{ ec2_id }}"
      wait: true
    delegate_to: "localhost"

- hosts: "localhost"
  connection: "local"
  gather_facts: false
  vars:
    region: "us-west-2"

  tasks:
  - name: "Remove security group"
    ec2_group:
      name: "ansible-sec-group"
      description: "New SG for Ansible-created instances"
      region: "{{ region }}"
      state: "absent"
```

This playbook is a bit more complex than the previous one, so let's break it down a bit:

* This playbook has two plays: one that is configured to run against a set of hosts called "tag_tool_ansible", and another that is set to run against the local system. Ultimately, all plays will run on the local system via the use of the `delegate_to` command.
* The first play has two tasks. The first task takes instances out of the security group we created; this is necessary in order to remove the group (otherwise the task removing the group fails because there are still instances that are part of the group). This play uses the membership of the "tag_tool_ansible" group from the dynamic inventory script to enumerate the list of instances, and then executes the task locally via the `delegate_to` command. The result is a local task that iterates against all the instances that are tagged with the tag "tool" that has a value of "ansible". (Astute readers will note that this tag was the tag assigned by the earlier playbook. See the connection now?)
* The second play removes the security group. Now that all the instances have been removed from the security group (and then terminated), we can remove the group itself.

To run this playbook, we have to specify the inventory, so the command looks like this:

    ansible-playbook -i ./ec2.py delete.yml

When the playbook executes, it will run the dynamic inventory script to generate an inventory. The inventory includes a group that corresponds to each tag/value assigned to the instances, generated as "tag_name_value" (i.e., like "tag_tool_ansible" or "tag_env_test"). You can use this group as the target for a play (as we did in the playbook), and since the tasks need to run locally on the system where the playbook is executing, use `delegate_to` to assign the task to the local system.

So there you have it---example Ansible playbooks to not only create infrastructure on AWS, but also to tear it down.

## Additional Resources

To make this easier to test out yourself, I've made all the playbooks and such discussed in this article available via [my "learning-tools" GitHub repository][link-6]. Just have a look in the `ansible-aws` directory.



[link-1]: https://aws.amazon.com/
[link-2]: https://www.ansible.com/
[link-3]: https://djaodjin.com/blog/deploying-on-ec2-with-ansible.blog.html
[link-4]: https://github.com/ansible/ansible/tree/devel/contrib/inventory
[link-5]: https://aws.amazon.com/cli/
[link-6]: https://github.com/scottslowe/learning-tools
[link-7]: https://www.zigg.com/2014/using-virtualenv-python-local-ansible.html
[xref-1]: {{< relref "2016-04-30-installing-ansible-in-virtualenv.md" >}}
