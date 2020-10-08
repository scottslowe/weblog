---
author: slowe
categories: Explanation
comments: true
date: 2020-10-08T08:50:00-07:00
tags:
- Automation
- AWS
- CAPI
- Kubernetes
- Pulumi
- Terraform
title: Considerations for using IaC with Cluster API
url: /2020/10/08/considerations-for-using-iac-with-cluster-api/
---

In other posts on this site, I've talked about both infrastructure-as-code (see [my posts on Terraform][link-1] or [my posts on Pulumi][link-2]) and somewhat separately I've talked about Cluster API (see [my posts on Cluster API][link-3]). And while I've discussed the idea of [using existing AWS infrastructure with Cluster API][xref-1], in this post I wanted to try to think about how these two technologies play together, and provide some considerations for using them together.<!--more-->

I'll focus here on AWS as the cloud provider/platform, but many of these considerations would also apply---in concept, at least---to other providers/platforms.

In no particular order, here are some considerations for using infrastructure-as-code and Cluster API (CAPI)---specifically, the Cluster API Provider for AWS (CAPA)---together:

* If you're going to need the CAPA workload clusters to have access to other AWS resources, like applications running on EC2 instances or managed services like RDS, you'll need to use the `additionalSecurityGroups` functionality, as I described in [this blog post][xref-2].
* The AWS cloud provider requires certain tags to be assigned to resources (see [this post][xref-3] for more details), and CAPI automatically provisions new workload clusters with the AWS cloud provider when running on AWS. Thus, you'll want to make sure that the IaC tool you're using is assigning the correct tags on the AWS resources.
* Continuing on the tag theme, you'll also need to make sure that the tags match the cluster name assigned in the workload cluster YAML manifest. So, for example, if your workload cluster YAML manifest defines a cluster name of "blue", the AWS tag must be `kubernetes.io/cluster/blue`. Otherwise, the AWS cloud provider won't function correctly.
* When it comes to bastion hosts, both CAPA and your IaC tool of choice can create them. You'll probably want to have them handled by the IaC tool (presumably you have other AWS resources you're managing to which you may also need access), in which case the first bullet point above---about using the `additionalSecurityGroups` functionality to enable access to other AWS resources---applies.
* CAPA will need access to information about the infrastructure it is consuming. Per [the upstream docs][link-4], CAPA needs the VPC ID and the IDs of all the subnets. Ideally, you'll want some sort of automated (or relatively automated) means of getting this information out of your IaC solution and into CAPA. For a few ideas of how this might be done with Pulumi, check out [this repository][link-5] that I created to accompany my Cloud Engineering Summit session.
* Keep in mind that using IaC to manage infrastructure but using CAPI/CAPA to manage your Kubernetes clusters creates a "split management" scenario. One potential benefit to CAPI/CAPA is that it can handle the lifecycle of both Kubernetes clusters and the underlying infrastructure. Leveraging IaC with CAPI/CAPA means giving up that potential benefit. On the flip side, using IaC for infrastructure may provide greater flexibility and more options for customization. As with so many things in technology, making this decision is all about weighing the trade-offs.

No doubt there are more considerations worth discussing, but this short list should get you started. Feel free to contact [me on Twitter][link-6] or find [me on the Kubernetes Slack][link-7] if you're interested in talking more about this topic.

[link-1]: /tags/terraform
[link-2]: /tags/pulumi
[link-3]: /tags/capi
[link-4]: https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/docs/book/src/topics/consuming-existing-aws-infrastructure.md
[link-5]: https://github.com/scottslowe/2020-ces-iac-capi
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://kubernetes.slack.com
[xref-1]: {{< relref "2019-09-09-consuming-preexisting-aws-infrastructure-with-cluster-api.md" >}}
[xref-2]: {{< relref "2020-04-22-using-existing-aws-security-groups-with-cluster-api.md" >}}
[xref-3]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
[xref-4]: {{< relref "" >}}
