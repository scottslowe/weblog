---
author: slowe
categories: Tutorial
comments: true
date: 2021-08-23T12:20:00-06:00
tags:
- AWS
- CLI
- JSON
- Kubernetes
title: Using the AWS CLI to Tag Groups of AWS Resources
url: /2021/08/23/using-aws-cli-tag-groups-of-aws-resources/
---

To conduct some testing, I recently needed to spin up a group of [Kubernetes][link-1] clusters on AWS. Generally speaking, my "weapon of choice" for something like this is [Cluster API (CAPI)][link-2] with the AWS provider. Normally this would be enormously simple. In this particular case---for reasons that I won't bother going into here---I needed to spin up all these clusters in a single VPC. This presents a problem for the [Cluster API Provider for AWS (CAPA)][link-3], as it currently doesn't add some required tags to existing AWS infrastructure (see [this issue][link-4]). The fix is to add the tags manually, so in this post I'll share how I used the AWS CLI to add the necessary tags.<!--more-->

Without the necessary tags, the AWS cloud provider---which is responsible for the integration that creates Elastic Load Balancers (ELBs) in response to the creation of a Service of type `LoadBalancer`, for example--- won't work properly. Specifically, the following tags are needed:

    kubernetes.io/cluster/<cluster-name>
    kubernetes.io/role/elb
    kubernetes.io/role/internal-elb

The latter two tags are mutually exclusive: the former should be assigned to public subnets to tell the AWS cloud provider where to place public-facing ELBs, while the latter is assigned to private subnets to manage the placement of internal ELBs.

When CAPA is managing the infrastructure, this isn't a problem because CAPA will add the necessary tags when it creates the infrastructure. Therefore, had I been able to use a separate VPC for each cluster, I could have let CAPA manage the infrastructure and avoided any issues entirely. In this case I was using a separate infrastructure-as-code tool ([Pulumi][link-7]) to manage the underlying AWS infrastructure and had the requirement to use a single VPC for multiple clusters.

Now, I _could_ have logged into the AWS console and used "point-and-click" to work my way through tagging the VPC and the subnets, but I preferred to use the AWS CLI. I quickly found [the `aws ec2 create-tags` command][link-6], which would do exactly what I needed; all I had to do was provide a list of the resources to tag and the tags to add.

To find the resources, I just had to make use of the tags that I'd made sure to assign to all the resources I created. So, to find all my public subnets, I used this AWS CLI command:

    aws ec2 describe-subnets --filters Name=tag:Owner,Values="Scott Lowe" Name=tag:Name,Values="*pub*" --query 'Subnets[*].SubnetId' --output text

Similarly, I could pull up my private subnets like this:

    aws ec2 describe-subnets --filters Name=tag:Owner,Values="Scott Lowe" Name=tag:Name,Values="*priv*" --query 'Subnets[*].SubnetId' --output text

Next, I had to prepare the tags I wanted added to each resource. For this, the parameter `--generate-cli-skeleton input` was very helpful; it generated the following framework:

```json
{
    "DryRun": true,
    "Resources": [
        ""
    ],
    "Tags": [
        {
            "Key": "",
            "Value": ""
        }
    ]
}
```

Using this skeleton as the foundation, I created two JSON input files---one for the tags to be assigned to public subnets, and one for the tags to be assigned to private subnets.

The documentation for the `aws ec2 create-tags` command indicated that it would take a space-delimited list of resource IDs, and the output from the `aws ec2 describe-subnets` command appeared to be space-delimited. At this point, I thought I'd be able to do something like this:

    aws ec2 describe-subnets --filters Name=tag:Owner,Values="Scott Lowe" \
    Name=tag:Name,Values="*priv*" --query 'Subnets[*].SubnetId' \
    --output text | xargs -I {} aws ec2 create-tags --resources {} \
    --cli-input-json file://priv-subnet-tags.json

Alas, this did not work. Further, no amount of messing around with the output of the `aws ec2 describe-subnets` command could get it into a format that `aws ec2 create-tags` liked. I'm sure it was an error of some sort on my part, but I couldn't figure it out.

I'd been spending a fair amount of time with `jq` recently (parsing a lot of Envoy configurations), so I thought, "Why not drop back to JSON output and use `jq`?"

Dropping the `--output text` and piping output through `jq` finally got me to a working command. Here's the command for the public subnets:

    aws ec2 describe-subnets --filters Name=tag:Owner,Values="Scott Lowe" \
    Name=tag:Name,Values="*pub*" --query 'Subnets[*].SubnetId' \
    jq -r '.[]' | xargs -p -I {} -n 1 aws ec2 create-tags --resources {} \
    --cli-input-json file://pub-subnet-tags.json

There was also a corresponding version for the private subnets as well.

Along the way, I did end up accidentally applying some private tags to public subnets; fortunately, [the `aws ec2 delete-tags` command][link-5] was there to save me.

Once the (correct) tags were applied, I was able to create a series of workload clusters via CAPI, and everything worked just as expected.

What did I learn from this whole process?

* I had not been previously aware of the `aws ec2 create-tags` and `aws ec2 delete-tags` commands; these are pretty handy.
* It seems that working with structured data, like JSON, can sometimes be easier than freeform text. `jq` is your ally here.
* Using the `--generate-cli-skeleton input` parameter is very useful for generating JSON input documents. I'll definitely be using that one again.

I hope this information is useful to you in some way. Thanks for reading! Feedback is always welcome, so feel free to reach out to [me on Twitter][link-8] if you have any questions or comments.

[link-1]: https://kubernetes.io/
[link-2]: https://cluster-api.sigs.k8s.io/
[link-3]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/issues/2584
[link-5]: https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/delete-tags.html
[link-6]: https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/create-tags.html
[link-7]: https://www.pulumi.com/
[link-8]: https://twitter.com/scott_lowe
