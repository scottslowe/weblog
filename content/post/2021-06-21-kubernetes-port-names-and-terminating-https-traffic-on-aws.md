---
author: slowe
categories: Education
comments: true
date: 2021-06-21T16:30:00-06:00
tags:
- AWS
- Kubernetes
- SSL
title: Kubernetes Port Names and Terminating HTTPS Traffic on AWS
url: /2021/06/21/kubernetes-port-names-and-terminating-https-traffic-on-aws/
---

I recently came across something that wasn't immediately intuitive with regard to terminating HTTPS traffic on an AWS Elastic Load Balancer (ELB) when using [Kubernetes][link-1] on AWS. At least, it wasn't intuitive to _me_, and I'm guessing that it may not be intuitive to some other readers as well. Kudos to my teammates [Hart Hoover][link-4] and Brent Yarger for identifying the resolution, which I'm going to call out in this post.<!--more-->

[This AWS Premium Support post][link-2] outlines the basic scenario:

* You're running Kubernetes on AWS. The post references EKS, but as far as I know the issue is not limited to EKS, and should apply to self-managed Kubernetes clusters on AWS (assuming these clusters are configured with the AWS cloud provider).
* You've published a Service of type LoadBalancer (which, in turn, creates a classic ELB). For self-managed clusters, this requires the AWS cloud provider to be installed and configured.
* You want to terminate HTTPS traffic on the ELB. The post references the use of an ACM certificate, but I suspect it's not limited to ACM certificates.

Consider the following YAML, taken directly from the previously-referenced AWS Premium Support article:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: echo-service
  annotations:
    # Note that the backend talks over HTTP.
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
    # TODO: Fill in with the ARN of your certificate.
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:{region}:{user id}:certificate/{id}
    # Only run SSL on the port named "https" below.
    service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "https"
spec:
  selector:
    app: echo-pod
  ports:
  - name: http
    port: 80
    targetPort: 8080
  - name: https
    port: 443
    targetPort: 8080
  type: LoadBalancer
```

I'd like to draw your attention to the `service.beta.kubernetes.io/aws-load-balancer-ssl-ports` annotation provided in the YAML above. The value of the annotation is `"https"`, which is the name commonly given to TCP port 443 (it's how the port number is referenced in `/etc/services`, for example). The `ports` field, farther down in the spec, specifies port 443, so all should be golden, right?

_Sort of._ This configuration works, certainly. The unintuitive thing here is captured, albeit ambiguously, in the comment directly above the annotation: _Only run SSL on the port named "https" below._ The "name" being referenced here isn't the name that the port number is commonly given; no, **it's the name assigned in the Kubernetes Service definition itself.** This example is ambiguous because "https" is the name most commonly associated with port 443.

Let's change the above YAML to make it more clear. Consider this Service definition---will it work?

```yaml
apiVersion: v1
kind: Service
metadata:
  name: echo-service
  annotations:
    # Note that the backend talks over HTTP.
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
    # TODO: Fill in with the ARN of your certificate.
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: arn:aws:acm:{region}:{user id}:certificate/{id}
    # Only run SSL on the port named "https" below.
    service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "https"
spec:
  selector:
    app: echo-pod
  ports:
  - name: http
    port: 80
    targetPort: 8080
  - name: http-tls
    port: 443
    targetPort: 8080
  type: LoadBalancer
```

If you said "No," give yourself a hearty round of applause, because you're correct! Although the Service is still referencing port 443 (commonly referred to and often considered "named" as HTTPS), the annotation is incorrect. In this case, the annotation needs to reference "http-tls", which is _the name assigned to the port in the Kubernetes Service definition_. The correct annotation would be `service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "http-tls"`. If the port had been named "green" in the Service definition, then the correct annotation would be `service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "green"`.

It seems obvious once you figure it out (as do most things), but it most certainly was not intuitive to me. Hopefully sharing this information here will help others avoid falling into a similar trap. Hit [me up on Twitter][link-3] if you have questions or comments.

[link-1]: https://kubernetes.io
[link-2]: https://aws.amazon.com/premiumsupport/knowledge-center/terminate-https-traffic-eks-acm/
[link-3]: https://twitter.com/scott_lowe
[link-4]: https://twitter.com/hhoover
