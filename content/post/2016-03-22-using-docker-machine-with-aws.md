---
author: slowe
categories: Tutorial
comments: true
date: 2016-03-22T00:00:00Z
tags:
- AWS
- CLI
- Docker
- Ubuntu
title: Using Docker Machine with AWS
url: /2016/03/22/using-docker-machine-with-aws/
---

As part of a broader effort (see the post on [my 2016 projects][xref-1]) to leverage public cloud resources more than I have in the past, some [Docker Engine][link-2]-related testing I've been conducting recently has been done using AWS EC2 instances instead of VMs in my home lab. Along the way, I've found [Docker Machine][link-3] to be quite a handy tool, and in this post I'll share how to use Docker Machine with AWS.

By and large, using Docker Machine with AWS is pretty straightforward. You can get an idea of what information Docker Machine needs by running `docker-machine create -d amazonec2 --help`. (You can also view [the documentation for the AWS driver][link-7] specifically.) The key pieces of information you need are:

* `--amazonec2-access-key`: This is your AWS access key. Docker Machine can read it from the $AWS_ACCESS_KEY_ID environment variable, or---if you have the AWS CLI installed---Docker Machine can read it from there.
* `--amazonec2-secret-key`: This is your AWS secret key. As with the AWS access key, Docker Machine can read this from an environment variable ($AWS_SECRET_ACCESS_KEY) or from the AWS CLI credentials file (by default, found in `~/.aws/credentials`).
* `--amazonec2-region`: The AWS driver defaults to us-east-1 (N. Virginia). Note that this particular value does _not_ get read from your AWS CLI configuration, so even if you have your AWS CLI configuration set to use a different region you'll still need to include this flag when using Docker Machine.
* `--amazonec2-instance-type`: This specifies the instance type for the instance Docker Machine will create. In my testing (using the us-west-2 region), Docker Machine defaulted to using "t2.micro" instances when this value wasn't specified.
* `--amazonec2-ami`: The AWS machine image that Docker Machine should use to provision the instance. If you omit this value, Docker Machine will use a daily build of [Ubuntu][link-8] 15.10.
* `--amazonec2-ssh-user`: The name of the user account that Docker Machine will use to log into the system and provision the Docker Engine. If you're using the default AMI (see the previous bullet), then this value will default to "ubuntu". If you specify a custom AMI, you'll need to specify the correct value for this parameter as well.
* `--amazonec2-ssh-keypath`: This specifies the SSH key pair that should be injected into the instance. If you don't specify this value, Docker Machine will generate a new key for use with this instance.

There are some additional parameters that you can use to fine-tune the behavior of Docker Machine when provisioning AWS instances (refer to [the docs][link-7]), but these basic options should cover the majority of the use cases out there.

Putting all this together, an example command might look something like this:

    docker-machine create -d amazonec2 \
    --amazonec2-region us-west-2 \
    --amazonec2-instance-type "t2.micro" \
    --amazonec2-ssh-keypath ~/.ssh/ssh_key \
    aws-test

This would create a machine named "aws-test", with an instance type of t2.micro in the us-west-2 region, based on a daily Ubuntu 15.10 AMI, and using the SSH key found in the file `~/.ssh/ssh_key`. Once this instance is up and running, Docker Machine would provision and configure Docker Engine on the instance. Along the way, Docker Machine also creates a security group named "docker-machine" that allows the necessary Docker Engine-related traffic.

If you needed to install a test build or an experimental build of Docker on the provisioned AWS instance, you can use the `--engine-install-url` parameter. (See my post on [using Vagrant and Docker Machine together][xref-2] for an example.)

From there, you can use `eval $(docker-machine env aws-test)` to set this instance as the default Docker host for Docker commands you are running on your local system (a quick and easy way to use the Docker client on your Windows or OS X system with a Docker daemon on a remote Linux box).

Without this turning into a full-blown tutorial on Docker Machine, there are a few other things you can do at this point:

* You can run `docker-machine ls` to see the "machines" (each machine corresponds to an instance) defined on your local system.
* You can start or stop an instance using `docker-machine start <name>` and `docker-machine stop <name>`. (Obviously, the instance must have been created first, but this allows you to start/stop instances after they've been created.)
* You can use `docker-machine ssh <name>` to open an SSH session to the specified instance.

There's much more, of course, but this should get you started.

Once you're done with the instance(s), use `docker-machine rm <name>` to decommission the instance. This includes removing the local reference to it as well as terminating the remote EC2 instance.

## Other Resources

Please note that plenty of other folks have written about using Docker Machine on AWS; here are just a few resources that I used and recommend:

[Docker Machine Provisioning on AWS][link-1]  
[Using Docker Machine with AWS][link-5]  
[A lap around AWS and docker-machine][link-6]  



[link-1]: http://networkstatic.net/docker-machine-provisioning-on-aws/
[link-2]: https://www.docker.com/products/docker-engine
[link-3]: https://www.docker.com/products/docker-machine
[link-4]: https://aws.amazon.com/
[link-5]: http://www.tothenew.com/blog/using-docker-machine-with-aws/
[link-6]: https://alexanderzeitler.com/articles/a-lap-around-aws-and-docker-machine/
[link-7]: https://docs.docker.com/machine/drivers/aws/
[link-8]: http://www.ubuntu.com/
[xref-1]: {{< relref "2016-01-21-looking-ahead-2016-projects.md" >}}
[xref-2]: {{< relref "2015-08-04-using-vagrant-docker-machine-together.md" >}}
