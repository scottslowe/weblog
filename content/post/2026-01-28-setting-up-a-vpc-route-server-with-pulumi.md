---
author: slowe
categories: Tutorial
comments: true
date: 2026-01-28T09:00:00-04:00
tags:
- AWS
- BGP
- Go
- Networking
- Pulumi
title: Setting up a VPC Route Server with Pulumi
url: /2026/01/28/setting-up-a-vpc-route-server-with-pulumi/
---

If you need to work with BGP in your AWS VPCs---so that BGP-learned routes can be injected into a VPC route table---then you will likely need a VPC Route Server. While you _could_ set up a VPC Route Server manually, what's the fun in that? In this post, I will walk you through a Pulumi program that will set up a VPC Route Server. Afterward, I will discuss some ways you could check the functionality of the VPC Route Server to show that it is indeed working as expected.<!--more-->

To make things as easy as possible, I have added a simple [Pulumi][link-1] program to [my GitHub "learning-tools" repository][link-2] in the `aws/vpc-route-server` directory. This program sets up a VPC Route Server and its associated components for you, and I will walk through this program in this blog post.

The first step is creating the VPC Route Server itself. The VPC Route Server has no prerequisities, and the primary configuration needed is setting the ASN (Autonomous System Number) the Route Server should use:

```go
rs, err := vpc.NewRouteServer(ctx, "rs", &vpc.RouteServerArgs{
    AmazonSideAsn: pulumi.Int(65534),
    Tags: pulumi.StringMap{
        "Name":    pulumi.String("rs"),
        "Project": pulumi.String("vpc-route-server"),
    },
})
```

Next, you will need to associate the Route Server with a VPC. This requires a VPC ID, as you might expect; this ID could come from a configuration value passed in by the user, or from a VPC created earlier in this or another Pulumi program.

```go
rsa, err := vpc.NewRouteServerVpcAssociation(ctx, "rsa", &vpc.RouteServerVpcAssociationArgs{
    RouteServerId: rs.ID(),
    VpcId:         pulumi.String(userSuppliedVpcId),
})
```

With that association in place, the next component is a Route Server Endpoint. This endpoint is created in a subnet, and will have an IP address assigned from that subnet. This IP address is what your BGP peer will use to establish a peering relationship to exchange routes.

```go
rse, err := vpc.NewRouteServerEndpoint(ctx, "rse", &vpc.RouteServerEndpointArgs{
    RouteServerId: rs.RouteServerId,
    SubnetId:      pulumi.String(userSuppliedPrivateSubnetId),
    Tags: pulumi.StringMap{
        "Name":    pulumi.String("rse"),
        "Project": pulumi.String("vpc-route-server"),
    },
}, pulumi.DependsOn([]pulumi.Resource{rsa}))
```

In working with the VPC Route Server, I've found that sometimes the dependencies between components don't quite work as expected. Because the endpoint references the `rs` resource (the Route Server itself), the endpoint has a implicit dependency on that resource. However, if the VPC association isn't complete, then you can run into errors. To address that problem, the code above uses `pulumi.DependsOn` to create an explicit dependency on the VPC association. This way, both the Route Server and the association of the Route Server to a VPC are complete before the program attempts to create the endpoint.

The Route Server needs to know where to inject the routes it learns, and that's handled by creating a propagation. The propagation links the Route Server to a route table in the associated VPC. Here's the Pulumi code from the example in the "learning-tools" repository:

```go
_, err = vpc.NewRouteServerPropagation(ctx, "rs-prop", &vpc.RouteServerPropagationArgs{
    RouteServerId: rs.RouteServerId,
    RouteTableId:  pulumi.String(userSuppliedPrivateRouteTableId),
}, pulumi.DependsOn([]pulumi.Resource{rsa}))
```

The use of `pulumi.DependsOn` makes the propagation, like the endpoint, dependent on the VPC association. This helps eliminate errors related to the order of resource creation or deletion.

The last step is creating a peer for the Route Server. This tells the Route Server what the "other side" of a BGP peering relationship looks like. The "other side" could be an EC2 instance running a BGP daemon, or it could be a Kubernetes node on a cluster running a CNI that supports BGP (like [Cilium][link-3]). Whatever the "other side" is, you'll need to be sure the configuration you pass to the Route Server peer matches the configuration in use by the other side. Otherwise, establishing the BGP peering relationship will fail.

```go
_, err = vpc.NewRouteServerPeer(ctx, "rs-peer-01", &vpc.RouteServerPeerArgs{
    BgpOptions: &vpc.RouteServerPeerBgpOptionsArgs{
        PeerAsn:               pulumi.Int(65001),
        PeerLivenessDetection: pulumi.String("bgp-keepalive"),
    },
    PeerAddress:           pulumi.String(userSuppliedPeerIpAddress),
    RouteServerEndpointId: rse.RouteServerEndpointId,
    Tags: pulumi.StringMap{
        "Name":    pulumi.String("rs-peer-01"),
        "Project": pulumi.String("vpc-route-server"),
    },
}, pulumi.DependsOn([]pulumi.Resource{rs, rse}))
```

In the definition of the Route Server peer, you point it to the IP address of some other device or instance running BGP. In the other device or instance, you will point it to the IP address of the Route Server endpoint. This way, each member of the BGP peering relationship knows how to connect to the other member.

All of the code above is found in the `aws/vpc-route-server` directory in [my GitHub "learning-tools" repository][link-2].

Once you run `pulumi up` to create all the resources, verifying the creation is a matter of using the AWS CLI:

```shell
aws ec2 describe-route-servers # to get the Route Server ID
aws ec2 describe-route-server-endpoints
aws ec2 get-route-server-propagations --route-server-id <id>
aws ec2 describe-route-server-peers 
```

Some of these commands don't (yet) support some of the same filtering mechanisms as other AWS CLI commands, so you may find it necessary to use `jq` to filter the output on the client side.

Verifying the operation of the Route Server is a bit more complex---you will need _something_ to run BGP and peer with the Route Server to exchange routes. As I mentioned earlier, Cilium can do this for a Kubernetes cluster, but you could also use an EC2 instance running [FRR][link-4] or some other BGP daemon. This is a topic I might explore in a future blog post; if it's something you're interested in seeing, please let me know.

If you have any questions or comments, please reach out to me. I love hearing from readers and would be happy to do my best to answer any queries or respond to feedback. You can reach me via email (my address is on this site), via various social media sites ([X/Twitter][link-5], [Bluesky][link-6], [Mastodon][link-7]), or via Slack (I'm in several Slack communities, including the Kubernetes Slack community). I hope you found this information helpful, and thanks for reading!

[link-1]: https://www.pulumi.com
[link-2]: https://github.com/scottslowe/learning-tools
[link-3]: https://cilium.io
[link-4]: https://frrouting.org/
[link-5]: https://x.com/scott_lowe
[link-6]: https://bsky.app/profile/scottslowe.bsky.social
[link-7]: https://fosstodon.org/@scottslowe
