---
author: slowe
categories: Explanation
comments: true
date: 2018-06-08T13:00:00Z
tags:
- Vagrant
- Linux
- CLI
- Virtualization
- Fusion
- VirtualBox
- AWS
- Libvirt
title: A Quadruple-Provider Vagrant Environment
url: /2018/06/08/quadruple-provider-vagrant-environment/
---

In October 2016 [I wrote about][xref-1] a triple-provider [Vagrant][link-1] environment I'd created that worked with [VirtualBox][link-3], [AWS][link-4], and the VMware provider (tested with [VMware Fusion][link-5]). Since that time, I've incorporated Linux ([Fedora][link-2], specifically) into my computing landscape, and I started using the Libvirt provider for Vagrant (see my write-up [here][xref-2]). With that in mind, I updated the triple-provider environment to add support for Libvirt and make it a quadruple-provider environment.<!--more-->

To set expectations, I'll start out by saying there isn't a whole lot here that is dramatically different than the triple-provider setup that I shared back in October 2016. Obviously, it supports more providers, and I've improved the setup so that _no_ changes to the Vagrantfile are needed (everything is parameterized).

With that in mind, let's take a closer look. First, let's look at the `Vagrantfile` itself:

``` ruby
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'

# Require 'yaml' module
require 'yaml'

# Read YAML file with VM details (box, CPU, and RAM)
machines = YAML.load_file(File.join(File.dirname(__FILE__), 'machines.yml'))

# Create and configure the VMs
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Always use Vagrant's default insecure key
  config.ssh.insert_key = false

  # Iterate through entries in YAML file to create VMs
  machines.each do |machine|

    # Configure the AWS provider
    config.vm.provider 'aws' do |aws|

      # Specify default AWS key pair
      aws.keypair_name = machine['aws']['keypair']

      # Specify default region
      aws.region = machine['aws']['region']
    end # config.vm.provider 'aws'

    config.vm.define machine['name'] do |srv|

      # Don't check for box updates
      srv.vm.box_check_update = false

      # Set machine's hostname
      srv.vm.hostname = machine['name']

      # Use dummy AWS box by default (override per-provider)
      srv.vm.box = 'aws-dummy'

      # Configure default synced folder (disable by default)
      if machine['sync_disabled'] != nil
        srv.vm.synced_folder '.', '/vagrant', disabled: machine['sync_disabled']
      else
        srv.vm.synced_folder '.', '/vagrant', disabled: true
      end #if machine['sync_disabled']

      # Iterate through networks as per settings in machines.yml
      machine['nics'].each do |net|
        if net['ip_addr'] == 'dhcp'
          srv.vm.network net['type'], type: net['ip_addr']
        else
          srv.vm.network net['type'], ip: net['ip_addr']
        end # if net['ip_addr']
      end # machine['nics'].each

      # Configure CPU & RAM per settings in machines.yml (Fusion)
      srv.vm.provider 'vmware_fusion' do |vmw, override|
        vmw.vmx['memsize'] = machine['ram']
        vmw.vmx['numvcpus'] = machine['vcpu']
        override.vm.box = machine['box']['vmw']
        if machine['nested'] == true
          vmw.vmx['vhv.enable'] = 'TRUE'
        end #if machine['nested']
      end # srv.vm.provider 'vmware_fusion'

      # Configure CPU & RAM per settings in machines.yml (VirtualBox)
      srv.vm.provider 'virtualbox' do |vb, override|
        vb.memory = machine['ram']
        vb.cpus = machine['vcpu']
        override.vm.box = machine['box']['vb']
        vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
        vb.customize ['modifyvm', :id, '--nictype2', 'virtio']
      end # srv.vm.provider 'virtualbox'

      # Configure CPU & RAM per settings in machines.yml (Libvirt)
      srv.vm.provider 'libvirt' do |lv,override|
        lv.memory = machine['ram']
        lv.cpus = machine['vcpu']
        override.vm.box = machine['box']['lv']
        if machine['nested'] == true
          lv.nested = true
        end # if machine['nested']
      end # srv.vm.provider 'libvirt'

      # Configure per-machine AWS provider/instance overrides
      srv.vm.provider 'aws' do |aws, override|
        override.ssh.private_key_path = machine['aws']['key_path']
        override.ssh.username = machine['aws']['user']
        aws.instance_type = machine['aws']['type']
        aws.ami = machine['box']['aws']
        aws.security_groups = machine['aws']['security_groups']
      end # srv.vm.provider 'aws'
    end # config.vm.define
  end # machines.each
end # Vagrant.configure
```

A couple of notes about the above `Vagrantfile`:

* All the data is pulled from an external YAML file named `machines.yml`; more information on that shortly.
* The "magic," if you will, is in the provider overrides. HashiCorp recommends _against_ provider overrides, but in my experience they're a necessity when working with multi-provider setups. Within each provider override block, we set provider-specific details and adjust the box needed (because finding boxes that support multiple platforms is downright impossible in many cases).
* The `machine[nics].each do |net|` section works for the local virtualization providers (VirtualBox, VMware, and Libvirt), but is silently ignored for AWS. That made making the `Vagrantfile` much easier, in my opinion. Note that last time I _really_ tested the Libvirt provider there was some weirdness with the network configuration; the configuration shown above works as expected. Other configurations may not.

Now, let's look at the external YAML data file that feeds Vagrant the information it needs:

``` yaml
- aws:
    type: "t2.medium"
    user: "ubuntu"
    key_path: "~/.ssh/id_rsa"
    security_groups:
      - "default"
      - "test"
    keypair: "ssh_keypair"
    region: "us-west-2"
  box:
    aws: "ami-db710fa3"
    lv: "generic/ubuntu1604"
    vb: "ubuntu/xenial64"
    vmw: "bento/ubuntu-16.04"
  name: "xenial-01"
  nested: false
  nics:
    - type: "private_network"
      ip_addr: "dhcp"
  ram: "512"
  sync_disabled: true
  vcpu: "1"
```

This is pretty straightforward YAML. This configuration does support multiple VMs/instances, with one interesting twist. When working with multiple AWS instances, you only need to specify the AWS keypair and AWS region on the **last** instance defined in the YAML file. You can include it for all instances, if you like, but only the values on the last instance will actually apply. I may toy around with supporting multi-region configurations, but that is kind of far down my priority list. The other AWS-specific values (type, user, and path to private key) need to be specified for all instances in the YAML file.

To use this environment, you only need to edit the external YAML file with the appropriate values and make sure authentication against AWS is working as expected. I recommend installing and configuring the AWS CLI to ensure that authentication against AWS is working as expected. Alternately, you could use something like [`aws-vault`][link-6].

Then it's just a matter of running the appropriate command for your particular environment:

`vagrant up --provider=aws` (to spin up instances on AWS)  
`vagrant up --provider=virtualbox` (to spin up VirtualBox VMs locally)  
`vagrant up --provider=vmware_fusion` (to use Fusion to create local VMs)  
`vagrant up --provider=libvirt` (to create Libvirt guest domains locally)

Using this sort of technique to support multiple providers in a single Vagrant environment provides a clean, consistent workflow regardless of backend provider. Naturally, this could be extended to include other providers using the same basic techniques I've used here. I'll leave that as an exercise to the readers.

## My Use Case

You might be wondering, "Why did you put effort into this?" It's pretty simple, really. I'm working on a project where I needed to be able to quickly and easily spin up a few instances on AWS. I felt like [Terraform][link-7] was a bit too "heavy" for this, as all I really needed was the ability to launch an instance or two, interact with the instances, then tear them down. Yes, I _could_ have done this with the AWS CLI, but...really? I knew that Vagrant worked with AWS, and I already use Vagrant for other purposes. It seemed pretty natural to incorporate the AWS support in Vagrant into my existing environments, and this quadruple-provider environment was the result. Enjoy!

[link-1]: https://www.vagrantup.com/
[link-2]: https://getfedora.org/
[link-3]: https://www.virtualbox.org/
[link-4]: https://aws.amazon.com/
[link-5]: http://www.vmware.com/products/fusion.html
[link-6]: https://github.com/99designs/aws-vault
[link-7]: https://www.terraform.io/
[xref-1]: {{< relref "2016-10-05-triple-provider-vagrant-environment.md" >}}
[xref-2]: {{< relref "2017-12-06-using-vagrant-with-libvirt-on-fedora.md" >}}
