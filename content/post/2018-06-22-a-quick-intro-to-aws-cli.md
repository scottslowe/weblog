---
author: slowe
categories: Education
comments: true
date: 2018-06-22T08:00:00Z
tags:
- AWS
- CLI
- JSON
title: A Quick Intro to the AWS CLI
url: /2018/06/22/a-quick-intro-to-aws-cli/
---

This post provides a (very) basic introduction to the [AWS CLI (command-line interface)][link-3] tool. It's not intended to be a deep dive, nor is it intended to serve as a comprehensive reference guide (the [AWS CLI docs][link-1] nicely fill that need). I also assume that you already have a basic understanding of the key AWS concepts and terminology, so I won't bore you with defining an instance, VPC, subnet, or security group.<!--more-->

For the purposes of this introduction, I'll structure it around launching an EC2 instance. As it turns out, there's a fair amount of information you need before you can launch an AWS instance using the AWS CLI. So, let's look at how you would use the AWS CLI to help get the information you need in order to launch an instance using the AWS CLI. (Tool inception!)

To launch an instance, you need five pieces of information:

1. The ID of an Amazon Machine Image (AMI)
2. The type of instance you're going to launch
3. The name of the SSH keypair you'd like to inject into the instance
4. The ID of the security group to which this instance should be added
5. The ID of the subnet on which this instance should be placed

Some (most?) of this information is easily located via the AWS console, but I'll use the CLI nevertheless.

Let's start by determining the name of the SSH keypair you'd like to inject into the instance (this is assuming you're launching a Linux-based instance). The basic format for AWS CLI commands looks something like `aws <service> <command>`. In this case, we're dealing with the EC2 service, and we want to get a list of---or _describe_---the SSH keypairs. So the command looks like this:

    aws ec2 describe-key-pairs

What you'll get back is JSON (see [this article][xref-1] if you need a quick introduction to JSON) that looks something like this (some of the data has been randomized to protect the innocent):

``` json
{
  "KeyPairs": [
    {
      "KeyName": "key_pair_name",
      "KeyFingerprint": "57:ca:27:99:fe:2a:24:60:8e:7f:b4:de:ad:be:ef:f1"
    }
  ]
}
```

Because it's JSON, you can use handy tools like `jq` to manipulate and format this data. (If you aren't familiar with `jq`, see [this article][xref-2].) So, let's say you wanted to extract the keypair name into a variable so you can re-use it later. That command looks something like this:

    KEYPAIR=$(aws ec2 describe-key-pairs | jq -r '.KeyPairs[0].KeyName')

Subsequently running `echo $KEYPAIR` would produce the output `key_pair_name`, showing you that you've successfully extracted the name of the keypair into the variable named KEYPAIR. This trick---assigning the output of a command to a variable---is a trick of the Bash shell called _command substitution_, and I'll use it extensively in this article to store pieces of information we'll need later.

Next, you'll need to know what type of instance you want to launch. Rodney "Rodos" Haywood has a great article on [determining which instances are available in your region][link-4]. For now, we'll just assume you want to create a "t2.micro" instance.

Next, let's track down security group and subnet information. To retrieve security group details, you'd use this command:

    aws ec2 describe-security-groups

This will return a list of security groups and the rules in the security groups, so the output will be lengthy and/or complex. Once again we'll turn to `jq` to help parse the information down to show us only the group ID of the default security group:

    aws ec2 describe-security-groups | jq '.SecurityGroups[] | select (.GroupName == "default") | .GroupId'

This finds the group whose `GroupName` key has the value "default" (i.e., the security group named "default") and returns the group ID. You could modify the value for which you're searching as needed, of course.

You can use Bash command substitution to place the output of this command into a variable we'll use later:

    SG_ID=$(aws ec2 describe-security-groups | jq -r '.SecurityGroups[] | select (.GroupName == "default") | .GroupId')

In reviewing the output of the `aws ec2 describe-security-groups` command, perhaps you notice the security group doesn't allow SSH access. That will be problematic, as SSH is the nearly-universal means by which you gain access to Linux- and UNIX-based instances. We can fix that pretty easily with the oh-so-intuitively-named `aws ec2 authorize-security-group-ingress` command:

    aws ec2 authorize-security-group-ingress --group-id <value> --protocol <tcp|udp|icmp> --port <value> --cidr <value>

To allow SSH access, then, we'll use the `$SG_ID` value we obtained earlier and plug in the correct values for SSH:

    aws ec2 authorize-security-group-ingress --group-id $SG_ID \
    --protocol tcp --port 22 --cidr 0.0.0.0/0

And now our soon-to-be-launched instance will be available via SSH! (If you decide you don't want SSH access, just use the `aws ec2 revoke-security-group-ingress` command, which has the same syntax as the `aws ec2 authorize-security-group-ingress` command.)

So far, we've gathered the SSH key pair, the instance type, and the security group ID. Determining the next piece of information we need---the subnet ID---is pretty straightforward as well. The basic command is `aws ec2 describe-subnets`; when it's combined with a `jq` filter we can get the subnet ID for the subnet in a particular availability zone. Let's say we want the subnet from the "us-west-2b" availability zone:

    aws ec2 describe-subnets | jq '.Subnets[] | select(.AvailabilityZone == "us-west-2b") | .SubnetId'

This uses the same `jq` syntax we used with the security groups: finds the subnet object whose AvailabilityZone key is equal to "us-west-2b", then returns the ID for that subnet.

Naturally, we'll store this value in a variable for use later (adding the `-r` flag to `jq` to return the value in plain text, not as JSON):

    SUBNET_ID=$(aws ec2 describe-subnets | jq -r '.Subnets[] | select(.AvailabilityZone == "us-west-2b") | .SubnetId')

We're now left with only the AMI (Amazon Machine Image). I've saved this for last as some might consider it the most complex of the tasks. As you've likely guessed, the basic command is `aws ec2 describe-images`. However, you can't just run that command; it will return _way_ too much information. (Go ahead and try it, I'll wait here.)

To limit the amount of information returned by the `aws ec2 describe-images` command, we need to use some server-side filtering/querying functionality. There's a few different tricks we can use here:

* First, we can add the `--owners` flag to limit the command to show AMIs belonging to a specific account. Let's suppose that we're looking for an Ubuntu AMI; in the case, we'd use the owner ID of "099720109477", so that the command would look like this:

        aws ec2 describe-images --owners 099720109477

* However, that's not enough, because we still get far too many values returned. To whittle down the list even further, the AWS CLI supports the  "--filters" parameter, which allows us to add more constraints to the list. The most useful filter, in my opinion, is filtering on the Name value. This allows you to do "wildcard" searches for AMIs whose name match a pattern. Here's an example:

        aws ec2 describe-images --owners 099720109477 \
        --filters Name=name,Values='*ubuntu-xenial-16.04*'

    This will still return too much information, but we can tack on additional filters as needed:

        aws ec2 describe-images --owners 099720109477 \
        --filters Name=root-device-type,Values=ebs \
        Name=architecture, Values=x86_64 \
        Name=name,Values='*ubuntu-xenial-16.04*' \
        Name=virtualization-type,Values=hvm

    Here you can see I've added filters to show only AMIs that use EBS as the root volume type, are hardware-virtualized images, and are 64-bit images. The full list of available filters for the `describe-images` command is available [here][link-5].

* Our final trick is to add a server-side query to the command. This query uses JMESPath syntax (which I won't cover here because that's enough for a separate post on its own). Here's an example of using a server-side query to show only the most recent AMI that matches the rest of the criteria:

        aws ec2 describe-images --owners 099720109477 --filters Name=root-device-type,Values=ebs Name=architecture,Values=x86_64 Name=name,Values='*ubuntu-xenial-16.04*' Name=virtualization-type,Values=hvm --query 'sort_by(Images, &Name)[-1].ImageId'

    The last and final step would be to store the value this command returns into a variable for use later:

        IMAGE_ID=$(aws ec2 describe-images --owners 099720109477 --filters Name=root-device-type,Values=ebs Name=architecture,Values=x86_64 Name=name,Values='*ubuntu-xenial-16.04*' Name=virtualization-type,Values=hvm --query 'sort_by(Images, &Name)[-1].ImageId')

At this point, we've gathered all the information we need to launch an AWS instance. The command to use is (drum roll, please) `aws ec2 run-instances`, and we'll plug in the variables we've created along the way to supply all the necessary information. Here's what the final command would look like (I've wrapped it with backslashes for improved readability):

    aws ec2 run-instances --image-id $IMAGE_ID \
    --count 1 --instance-type t2.micro \
    --key-name $KEYPAIR \
    --security-group-ids $SG_ID \
    --subnet-id $SUBNET_ID

This command will also return some JSON, which will include the instance IDs of the instances we just launched. We'll ignore that output for now for the sake of being able to show a few more ways to use the AWS CLI.

With an instance now running, let's retrieve the details of the instance with another AWS CLI command:

    aws ec2 describe-instances

This command also returns a pretty fair amount of information, and again we can use `jq` to filter down the information to show _only_ the details you need. Suppose you need to see the private and public IP addresses assigned to the instance you just launched. With this set of `jq` filters, that's exactly what you'll see:

    aws ec2 describe-instances | jq '.Reservations[].Instances[] | { instance: .InstanceId, publicip: .PublicIpAddress, privateip: .PrivateIpAddress }'

Handy! I could run through a dozen more examples, but I think you get the point now.

Let's move on to terminating an instance. In order to terminate an instance---and you guessed it, the appropriate command is `aws ec2 terminate-instances` as expected---we'll need the instance IDs. Turning back to `jq` again:

    aws ec2 describe-instances | jq '.Reservations[].Instances[] | .InstanceId'

And once again using command substitution store that into a variable:

    INSTANCE_ID=$(aws ec2 describe-instances | jq -r '.Reservations[].Instances[] | .InstanceId')

Then we can plug that value into this command to terminate instances:

    aws ec2 terminate-instances --instance-ids $INSTANCE_ID

Hopefully this quick introduction to basic tasks you can perform with the AWS CLI has been useful. Feel free to [hit me on Twitter][link-7] if you have questions. If you have suggestions for improving this article, I invite you to open an issue or file a PR [on the GitHub repository for this site][link-6].

[link-1]: http://docs.aws.amazon.com/cli/latest/reference/
[link-2]: https://stedolan.github.io/jq/
[link-3]: https://aws.amazon.com/cli/
[link-4]: http://rodos.haywood.org/2016/03/which-instances-are-available-in-my.html
[link-5]: https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-images.html
[link-6]: https://github.com/scottslowe/weblog
[link-7]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
