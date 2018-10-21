---
author: slowe
categories: Explanation
comments: true
date: 2012-07-24T20:58:00Z
slug: getting-hiera-working
tags:
- Automation
- BSD
- Linux
- OSS
- UNIX
- Interoperability
title: Getting Hiera Working
url: /2012/07/24/getting-hiera-working/
wordpress_id: 2690
---

As I mentioned in [a previous post][1], the next iteration of my Puppet explorations involves the use of [Hiera](http://projects.puppetlabs.com/projects/hiera). Hiera, a project also managed by Puppet Labs, is described as "a simple pluggable hierarchical database." In the Puppet world, what that means is we can use Hiera to store data values outside of the manifests, then look them up dynamically as the configurations are being compiled and applied to the nodes. In a future post, I'll provide an example of how you could use Hiera in a multi-OS Puppet environment.

For now, though, I just want to talk about how to get Hiera up and running and working in a Puppet environment, as it wasn't as straightforward as I expected it to be.

I'm using the same virtual environment for this post as I've used in previous posts. I have a Puppet master server running on an Ubuntu 12.04 LTS VM; this master server services client VMs running Ubuntu 12.04 LTS, CentOS 5.8, and OpenBSD 5.1. Keep in mind that if you are using a different distribution of Linux than what I'm using here, your specific directories and paths might be slightly different.

Here are the steps that I took to get Hiera up and running on the Puppet master server:

1. First, I used `gem install hiera hiera-puppet` to install Hiera and Hiera-Puppet (the connecting code between Hiera and Puppet).

2. Optionally, you can next run `gem list` to verify that Hiera and Hiera-Puppet are included in the list of installed Ruby gems.

3. You'll need to know exactly where the Hiera-Puppet files are found on your system, so run `gem list -d` to show the details of the installed gems. This will include the path where the files are found. On my Ubuntu 12.04 LTS Puppet master server, Hiera and Hiera-Puppet were installed to `/var/lib/gems/1.8/gems` (in their own directories, respectively).

4. Next, you'll need to know where Puppet stores its modules. Run `puppet master --configprint modulepath` to get the list of directories where Puppet modules are stored. On my Ubuntu 12.04 LTS Puppet master server, that included `/etc/puppet/modules`.

5. Copy the Hiera-Puppet directory (found in step 3) to one of the Puppet module directories (found in step 4). This ensures that Puppet can actually leverage Hiera. Note that [this Puppet Labs post](http://puppetlabs.com/blog/first-look-installing-and-using-hiera/) provides a different process for accomplishing this; I found that their process didn't work in my environment.

6. Hiera will need a directory to store its configuration files; I used `/etc/puppet/hieradata`. I don't think it matters where the directory is; just make a note of where.

7. You'll need to create a Hiera configuration file located in the main Puppet directory (on my Ubuntu 12.04 LTS Puppet master server, this was `/etc/puppet`). This file is called `hiera.yaml` and will probably need to look something like this (you'd specify the directory created in step 6 on the last line here):

```yaml
---
:hierarchy:
  - %{operatingsystem}
  - common
:backends:
  - yaml
:yaml:
  :datadir: '/etc/puppet/hieradata'
```

At this point, you should now have a working Hiera installation. In a future post, I'll show you how I used Hiera with Puppet to create OS-based customized configuration files.

In the directory you created for Hiera and specified in `hiera.yaml`, you'll need to create the YAML files for your hierarchy. Since I'm using a hierarchy based on operating system, I created YAML files for OpenBSD, CentOS, Ubuntu, etc. One note: the file names must match the operating system name returned by Facter **exactly**, including case (i.e., `OpenBSD.yaml` instead of `openbsd.yaml`).

In working to get Hiera up and running, I found the following websites and pages to be helpful:

[First Look: Installing and Using Hiera (part 1 of 2)](http://puppetlabs.com/blog/first-look-installing-and-using-hiera/)  

[Puppet configuration variables and Hiera](http://www.craigdunn.org/2011/10/puppet-configuration-variables-and-hiera/)

Corrections, suggestions for improvement, or questions are always welcome! Speak up in the comments below.

[1]: {{< relref "2012-07-09-updated-multi-os-puppet-configuration.md" >}}
