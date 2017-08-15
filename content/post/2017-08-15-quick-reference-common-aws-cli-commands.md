---
author: slowe
categories: Education
comments: true
date: 2017-08-15T12:00:00Z
tags:
- AWS
- CLI
- JSON
title: Quick Reference to Common AWS CLI Commands
url: /2017/08/15/quick-reference-common-aws-cli-commands/
---

This post provides an extremely basic "quick reference" to some commonly-used [AWS CLI][link-3] commands. It's not intended to be a deep dive, nor is it intended to serve as any sort of comprehensive reference (the [AWS CLI docs][link-1] nicely fill that need).<!--more-->

This post does make a couple of important assumptions:

1. This post assumes you already have a basic understanding of the key AWS concepts and terminology, and therefore doesn't provide any definitions or explanations of these concepts.

2. This post assumes the AWS CLI is configured to output in JSON. (If you're not familiar with JSON, see [this introductory article][xref-1].) If you've configured your AWS CLI installation to output in plain text, then you'll need to adjust these commands accordingly.

I'll update this post over time to add more "commonly-used" commands, since each reader's definition of "commonly used" may be different based on the AWS services consumed.

To list SSH keypairs in your default region:

    aws ec2 describe-key-pairs

To use `jq` to grab the name of the first SSH keypair returned:

    aws ec2 describe-key-pairs | jq -r '.KeyPairs[0].KeyName'

To store the name of the first SSH keypair returned in a variable for use in later commands:

    KEY_NAME=$(aws ec2 describe-key-pairs | jq -r '.KeyPairs[0].KeyName')

More information on the use of `jq` can be found in [this article][xref-2] or via [the `jq` homepage][link-2], so this post doesn't go into any detail on the use of `jq` in conjunction with the AWS CLI. Additionally, for all remaining command examples, I'll leave the assignment of output values into a variable as an exercise for the reader.

To grab a list of instance types in your region, I recommend referring to Rodney "Rodos" Haywood's post on [determining which instances are available in your region][link-4].

To list security groups in your default region:

    aws ec2 describe-security-groups

To retrieve security groups from a specific region (us-west-1, for example):

    aws --region us-west-1 describe-security-groups

To use the `jp` tool to grab the security group ID of the group named "internal-only":

    aws ec2 describe-security-groups | jp "SecurityGroups[?GroupName == 'internal-only'].GroupId"

The `jp` command was created by the same folks who work on the AWS CLI; see [this site][link-5] for more information.

To list the subnets in your default region:

    aws ec2 describe-subnets

To grab the subnet ID for a subnet in a particular Availability Zone (AZ) in a region using `jp`:

    aws ec2 describe-subnets | jp "Subnets[?AvailabilityZone == 'us-west-2b'].SubnetId"

To describe the Amazon Machine Images (AMIs) you could use to launch an instance:

    aws ec2 describe-images

This command alone isn't all that helpful; it returns too much information. Filtering the information is pretty much required in order to make it useful. 

To filter the list of images by owner:

    aws ec2 describe-instances --owners 099720109477

To use server-side filters to further restrict the information returned by the `aws ec2 describe-images` command (this example finds Ubuntu 14.04 "Trusty Tahr" AMIs in your default region):

    aws ec2 describe-images --owners 099720109477 --filters Name=root-device-type,Values=ebs Name=architecture,Values=x86_64 Name=name,Values='*ubuntu-trusty-14.04*' Name=virtualization-type,Values=hvm

To combine server-side filters and a JMESPath query to further refine the information returned by the `aws ec2 describe-images` command (this example returns the latest Ubuntu 14.04 "Trusty Tahr" AMI in your default region):

    aws ec2 describe-images --owners 099720109477 --filters Name=root-device-type,Values=ebs Name=architecture,Values=x86_64 Name=name,Values='*ubuntu-trusty-14.04*' Name=virtualization-type,Values=hvm --query 'sort_by(Images, &Name)[-1].ImageId'

Naturally, you can manipulate the filter values to find other types of AMIs. To find the latest CentOS Atomic Host AMI in your default region:

    aws ec2 describe-images --owners 410186602215 --filter Name=name,Values="*CentOS Atomic*" --query 'sort_by(Images,&CreationDate)[-1].ImageId'

To find the latest CoreOS Container Linux AMI from the Stable channel (in your default region):

    aws ec2 describe-images --filters Name=name,Values="*CoreOS-stable*" Name=virtualization-type,Values=hvm --query 'sort_by(Images,&CreationDate)[-1].ImageId'

Further variations on these commands for other AMIs is left as an exercise for the reader.

To launch an instance in your default region (assumes you've populated the necessary variables using other AWS CLI commands):

    aws ec2 run-instances --image-id $IMAGE_ID --count 1 --instance-type t2.micro --key-name $KEY_NAME --security-group-ids $SEC_GRP_ID --subnet-id $SUBNET_ID

To list the instances in your default region:

    aws ec2 describe-instances

To retrieve information about instances in your default region and use `jq` to return only the Instance ID and public IP address:

    aws ec2 describe-instances | jq '.Reservations[].Instances[] | {instance: .InstanceId, publicip: .PublicIpAddress}'

To terminate one or more instances:

    aws ec2 terminate-instances --instance-ids $INSTANCE_IDS

To remove a rule from a security group:

    aws ec2 revoke-security-group-ingress --group-id $SEC_GROUP_ID --protocol <tcp|udp|icmp> --port <value> --cidr <value>

To add a rule to a security group:

    aws ec2 authorize-security-group-ingress --group-id $SEC_GROUP_ID --protocol <tcp|udp|icmp> --port <value> --cidr <value>

To create an Elastic Container Service (ECS) cluster:

    aws ecs create-cluster [default|--cluster-name <value>]

If you use a name other than "default", you'll need to be sure to add the `--cluster <value>` parameter to all other ECS commands. This guide assumes a name other than "default".

To delete an ECS cluster:

    aws ecs delete-cluster --cluster <value>

To add container instances (instances running ECS Agent) to a cluster:

    aws ec2 run-instances --image-id $IMAGE_ID --count 3 --instance-type t2.medium --key-name $KEY_NAME --subnet-id $SUBNET_ID --security-group-ids $SEC_GROUP_ID --user-data file://user-data --iam-instance-profile ecsInstanceRole

The example above assumes you've already created the necessary IAM instance profile, and that the file `user-data` contains the necessary instructions to prep the instance to join the ECS cluster. Refer to the Amazon ECS documentation for more details.

To register a task definition (assumes the JSON filename referenced contains a valid ECS task definition):

    aws ecs register-task-definition --cli-input-json file://<filename>.json --cluster <value>

To create a service:

    aws ecs create-service --cluster <value> --service-name <value> --task-definition <family:task:revision> --desired-count 2

To scale down ECS services:

    aws ecs update-service --cluster <value> --service <value> --desired-count 0

To delete a service:

    aws ecs delete-service --cluster <value> --service <value>

To de-register container instances (instances running ECS Agent):

    aws ecs deregister-container-instance --cluster <value> --container-instance $INSTANCE_IDS --force

If there are additional AWS CLI commands you think I should add here, feel free to hit me up on Twitter. Try to keep them as broadly applicable as possible, so that I can maximize the benefit to readers. Thanks!



[link-1]: http://docs.aws.amazon.com/cli/latest/reference/
[link-2]: https://stedolan.github.io/jq/
[link-3]: https://aws.amazon.com/cli/
[link-4]: http://rodos.haywood.org/2016/03/which-instances-are-available-in-my.html
[link-5]: https://github.com/jmespath/jp
[xref-1]: {{< relref "2013-11-08-a-non-programmers-introduction-to-json.md" >}}
[xref-2]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
