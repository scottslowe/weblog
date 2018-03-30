---
author: slowe
categories: Explanation
comments: true
date: 2016-10-05T00:00:00Z
tags:
- Vagrant
- Linux
- CLI
- Virtualization
- Fusion
- VirtualBox
- AWS
title: A Triple-Provider Vagrant Environment
url: /2016/10/05/triple-provider-vagrant-environment/
---

In this post, I'd like to share with you some techniques I used to build a triple-provider [Vagrant][link-1] environment---that is, a Vagrant environment that will work unmodified with multiple backend providers. In this case, it will work (mostly) unmodified with [AWS][link-4], [VirtualBox][link-3], and the VMware provider (tested with Fusion, but should work with Workstation as well). I know this may not seem like a big deal, but it marks something of a milestone for me.

Since I first started using Vagrant a couple of years ago, I've---as expected---gotten better and better at leveraging this tool in a flexible way. You can see this in the evolution of the Vagrant environments found in [my GitHub "learning-tools" repository][link-2], where I went from hard-coded data values to pulling data from external YAML files.

One thing I'd been shooting for was a `Vagrantfile` that would work with multiple backend providers without any modifications, and tonight I managed to build an environment that works with AWS, VirtualBox, and [VMware Fusion][link-5]. There are still a couple of hard-coded values, but the vast majority of information is pulled from an external YAML file.

Let's take a look at the `Vagrantfile` that I created. Here's the first part of the Vagrant configuration, with ample comments to explain what's happening:

``` ruby
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'

# Require the AWS provider plugin and YAML module
require 'vagrant-aws'
require 'yaml'

# Read YAML file with instance information (box, CPU, RAM, IP addresses)
# Edit machines.yml to change VM configuration details
machines = YAML.load_file(File.join(File.dirname(__FILE__), 'machines.yml'))

# Create and configure the specified systems
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Use dummy AWS box (override in provider configuration blocks)
  config.vm.box = 'aws-dummy'

  # Configure default AWS provider settings
  config.vm.provider 'aws' do |aws|

    # Specify access/authentication information
    #aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    #aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

    # Specify default AWS key pair
    aws.keypair_name = 'aws_rsa'

    # Specify default region and default AMI ID
    aws.region = 'us-west-2'
  end # config.vm.provider 'aws'
```

Most of the stuff you see in this section is self-explanatory and I've probably already talked about it. Some of the AWS configurations I described when discussing [using Vagrant with AWS][xref-1], and the Ruby syntax for reading the YAML file I described in [this post][xref-2]. So far, nothing terribly new, unusual, or groundbreaking here.

Now let's look at the next section of the `Vagrantfile`:

``` ruby
  # Loop through YAML file and set per-VM information
  machines.each do |machine|
    config.vm.define machine['name'] do |srv|

      # Specify the hostname of the machine
      srv.vm.hostname = machine['name']

      # Don't check for box updates
      srv.vm.box_check_update = false

      # Disable default shared folder
      srv.vm.synced_folder '.', '/vagrant', disabled: true

      # Set per-machine AWS provider configuration/overrides
      srv.vm.provider 'aws' do |aws, override|
        override.ssh.private_key_path = '~/.ssh/aws_rsa'
        override.ssh.username = machine['aws']['user']
        aws.instance_type = machine['aws']['type']
        aws.ami = machine['box']['aws']
        aws.security_groups = ['default']
      end # srv.vm.provider 'aws'
```

Here you see using a loop to iterate through the values of an external YAML file (the one loaded in the first snippet). This is a technique I first described [here][xref-3], and one I've used extensively since that time. What _is_ new is the `machine['aws']['user']` syntax, which is used to pull data from a multi-level YAML file. In this particular case, the YAML file looks something like this:

``` yaml
---
- name: "instance-01"
  box:
    aws: "ami-20be7540"
    vmw: "slowe/ubuntu-trusty-x64"
    vb: "ubuntu/trusty64"
  aws:
    type: "t2.micro"
    user: "ubuntu"
  ram: "1024"
  vcpu: "1"
```

So, using `machine['aws']['user']` allows Vagrant to access the value of the "user" key under the "aws" item. Similarly, using `machine['box']['vmw']` allows Vagrant to pull the name of the VMware-formatted box found in the "vmw" key under the "box" item. This is a relatively new discovery for me, although in retrospect it now seems quite simple and straightforward.

In any case, getting back to the `Vagrantfile` in question, you can see that it's setting AWS-specific configuration settings, pulling those values from the external YAML file. The use of "override" here is helpful, as it allows me to override a value with a provider-specific value.

Overrides are used pretty extensively in the last section of the Vagrant environment, which configures the VMware and VirtualBox providers. Here's that code:

``` ruby
      # Set per-machine VMware provider configuration/overrides
      srv.vm.provider 'vmware_fusion' do |vmw, override|
        override.vm.box = machine['box']['vmw']
        vmw.vmx['memsize'] = machine['ram']
        vmw.vmx['numvcpus'] = machine['vcpu']
      end # srv.vm.provider 'vmware_fusion'

      # Set per-machine VirtualBox provider configuration/overrides
      srv.vm.provider 'virtualbox' do |vb, override|
        override.vm.box = machine['box']['vb']
        vb.memory = machine['ram']
        vb.cpus = machine['vcpu']
      end # srv.vm.provider 'virtualbox'
    end # config.vm.define
  end # machines.each
end # Vagrant.configure
```

These two provider-specific blocks look a fair amount like the AWS-specific block earlier, using Vagrant's "override" feature to set a provider-specific box name. Again, all the configuration data is pulled from the external YAML file.

So, to use this environment, one needs only to edit the external YAML file and set three AWS-specific settings (region, keypair name, and private key path), then run the appropriate command for his or her particular environment:

`vagrant up --provider=aws` (to spin up instances on AWS)  
`vagrant up --provider=virtualbox` (to spin up VirtualBox VMs locally)  
`vagrant up --provider=vmware_fusion` (to use Fusion to create local VMs)

Using this sort of technique to support multiple providers in a single Vagrant environment provides a clean, consistent workflow regardless of backend provider. Naturally, this could be extended to include other providers using the same basic techniques I've used here. I'll leave that as an exercise to the readers.

If you'd like to play around with a sample environment, have a look in the `multi-provider-simple` directory in [my GitHub "learning-tools" repository][link-2]. Enjoy!



[link-1]: https://www.vagrantup.com/
[link-2]: https://github.com/scottslowe/learning-tools/
[link-3]: https://www.virtualbox.org/
[link-4]: https://aws.amazon.com/
[link-5]: http://www.vmware.com/products/fusion.html
[xref-1]: {{< relref "2016-09-15-using-vagrant-with-aws.md" >}}
[xref-2]: {{< relref "2016-01-14-improved-way-yaml-vagrant.md" >}}
[xref-3]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
