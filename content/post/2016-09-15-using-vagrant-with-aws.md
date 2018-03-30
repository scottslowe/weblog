---
author: slowe
categories: Tutorial
comments: true
date: 2016-09-15T00:00:00Z
tags:
- Vagrant
- AWS
- CLI
title: Using Vagrant with AWS
url: /2016/09/15/using-vagrant-with-aws/
---

In this post, I'd like to describe how to use [Vagrant][link-1] with [AWS][link-5], as well as provide a brief description of why this combination of technologies may make sense for some use cases. In some respects, this post is similar to my posts on [using Docker Machine with OpenStack][xref-2] and [using Vagrant with OpenStack][xref-1] in that combining Vagrant with AWS creates another clean "provider/consumer" model that makes it easy for users to consume infrastructure.

If you aren't already familiar with Vagrant, I'd highly recommend first taking a look at my [introduction to Vagrant][xref-3], which provides an overview of the tool and how it's used.

## Prerequisites

Naturally, you'll need to first ensure that you have Vagrant installed. This is really well-documented already, so I won't go over it here. Next, you'll need to install the AWS provider for Vagrant, which you can handle using this command:

    vagrant plugin install vagrant-aws

Once you've installed the `vagrant-aws` plugin, you'll next need to install a box that Vagrant can use. Here, the use of Vagrant with AWS is a bit different than the use of Vagrant with a provider like [VirtualBox][link-2] or [VMware Fusion][link-3]/[VMware Workstation][link-4]. In those cases, the box is a VM template that is then cloned/copied to instantiate running VMs. When using Vagrant with AWS, you'll leverage AWS' Amazon Machine Images (AMIs), and so the role of the Vagrant box is really nothing more than a formality. In fact, Mitchell Hashimoto (the author of Vagrant and the `vagrant-aws` plugin) has a "dummy" box you can add:

    vagrant box add aws-dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

You can name this box whatever you like; in the command above, I've called it "aws-dummy".

Once you've gotten the AWS plugin and the dummy box installed, you're ready to start spawning AWS instances via Vagrant.

## Launching AWS Instances via Vagrant

In my use of Vagrant, I prefer to keep the Vagrant configuration (stored in the file named `Vagrantfile`) as clean as possible, and separate details into a separate data file (typically using YAML). I outlined this approach [here][xref-5] and [here][xref-6]. For the purposes of this post, however, I'll just embed the details directly into the Vagrant configuration to make it easier to understand. I have examples of using a YAML data file with Vagrant and AWS in the "Additional Resources" section below.

Here's a snippet of a `Vagrantfile` you could use to instantiate AWS instances using Vagrant:

``` ruby
# Require the AWS provider plugin
require 'vagrant-aws'

# Create and configure the AWS instance(s)
Vagrant.configure('2') do |config|

  # Use dummy AWS box
  config.vm.box = 'aws-dummy'

  # Specify AWS provider configuration
  config.vm.provider 'aws' do |aws, override|
    # Read AWS authentication information from environment variables
    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

    # Specify SSH keypair to use
    aws.keypair_name = 'ssh-keypair-name'

    # Specify region, AMI ID, and security group(s)
    aws.region = 'us-west-2'
    aws.ami = 'ami-20be7540'
    aws.security_groups = ['default']

    # Specify username and private key path
    override.ssh.username = 'ubuntu'
    override.ssh.private_key_path = '~/.ssh/ssh-keypair-file'
  end
end
```

Naturally, this is a very generic configuration, so you'd need to supply the specific details you want use. In particular, you'd need to supply the following details:

* The SSH keypair in AWS you want to use
* The AWS region you want to use
* The AMI ID you want to use
* The name(s) of the security group(s) you want applied to the instance(s)
* The username that should be used to access the instances (this will vary based on the AMI)
* The path to the private key file for the specified keypair

Also, you'll note that the `Vagrantfile` assumes you've set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables with the appropriate values, so be sure to do that before you try to run `vagrant up` (Vagrant will report an error otherwise).

With the right configuration details in place, simply run `vagrant up` from the same directory where the Vagrant configuration (in `Vagrantfile`) is stored. Vagrant will communicate with AWS (using the values in the corresponding environment variables) and instantiate the instance(s) per the details provided.

Once the instances are up, the "standard" Vagrant workflow applies:

* Use `vagrant ssh <name>` to log into one of the instances.
* Use `vagrant provision` to apply any provisioning instructions (i.e., to copy files across or run a configuration management tool).
* Use `vagrant destroy` to kill/terminate the instances.

All in all, it makes using AWS instances feel a lot like working with local VMs.

## Why Use Vagrant with AWS?

The idea behind Vagrant---as I understand it---is to help simplify the creation of temporary environments to be used for testing, software development, etc. The ability to quickly and easily spin up instances on AWS makes using Vagrant with AWS a natural fit for these sorts of use cases, in my mind. It also keeps a consistent workflow for users: `vagrant up` creates local VMs or instantiates AWS instances, as appropriate.

In situations where you are creating more "permanent" infrastructure---such as deploying production applications onto AWS infrastructure---then I would say that Vagrant is _not_ the right fit. In those cases, using a tool like [Terraform][link-9] (see [my introductory post][xref-4]) or [AWS CloudFormation][link-8] would be more appropriate.

## Additional Resources

To help make using Vagrant with AWS easier, I've created a couple learning environments that are part of [my GitHub "learning-tools" repository][link-6]. Specifically, see the `vagrant-aws` and `vagrant-aws-multi` directories for sample Vagrant configurations.

Additionally, see the documentation for [the `vagrant-aws` plugin on GitHub][link-7] for more details.



[link-1]: https://www.vagrantup.com
[link-2]: https://www.virtualbox.org
[link-3]: http://www.vmware.com/products/fusion.html
[link-4]: http://www.vmware.com/products/workstation.html
[link-5]: https://aws.amazon.com
[link-6]: https://github.com/scottslowe/learning-tools
[link-7]: https://github.com/mitchellh/vagrant-aws
[link-8]: https://aws.amazon.com/cloudformation/
[link-9]: https://www.terraform.io/
[xref-1]: {{< relref "2015-09-28-using-vagrant-with-openstack.md" >}}
[xref-2]: {{< relref "2015-09-24-using-docker-machine-with-openstack.md" >}}
[xref-3]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-4]: {{< relref "2015-11-25-intro-to-terraform.md" >}}
[xref-5]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
[xref-6]: {{< relref "2016-01-14-improved-way-yaml-vagrant.md" >}}
