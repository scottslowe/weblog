---
author: slowe
categories: Explanation
comments: true
date: 2019-02-01T22:00:00
tags:
- Kubernetes
- Prometheus
- Envoy
title: Scraping Envoy Metrics Using the Prometheus Operator
url: /2019/02/01/scraping-envoy-with-prometheus-operator/
---

On a recent customer project, I recommended the use of [Heptio Contour][link-1] for ingress on their Kubernetes cluster. For this particular customer, Contour's support of the IngressRoute CRD and the ability to delegate paths via IngressRoutes made a lot of sense. Of course, the customer wanted to be able to scrape metrics using [Prometheus][link-3], which meant I not only needed to scrape metrics from Contour but _also_ from [Envoy][link-2] (which provides the data plane for Contour). In this post, I'll show you how to scrape metrics from Envoy using the Prometheus Operator.<!--more-->

First, I'll assume that you've already installed and configured Prometheus using the Prometheus Operator, a task which is already fairly well-documented and well-understood. If this is something you think would be helpful for me to write a blog post on, please [contact me on Twitter][link-99] and let me know.

The overall process looks something like this:

1. Modify the Envoy DaemonSet (or Deployment, depending on your preference) to add a sidecar container and expose additional ports.
2. Modify the Service for the Envoy DaemonSet/Deployment to expose the ports you added in step 1.
3. Add a ServiceMonitor object (a CRD added by the Prometheus Operator) to tell Prometheus to scrape the metric from Envoy.

Let's take a closer look at each of these steps.

## Modifying the Envoy DaemonSet/Deployment

The first step is to modify the Envoy DaemonSet (or Deployment) to include a sidecar container and expose some additional ports. I found it helpful to use [this example DaemonSet configuration from the Heptio Gimbal repository][link-4] as a guide. There are three sets of changes you need to make:

1. Add port 8002 to the Envoy container.
2. Add a sidecar container for the statsd-exporter.
3. Modify the Contour initContainer to add the `--statsd-enabled` flag.

The first set of changes---adding port 8002---isn't reflected in the linked Gimbal example because that configuration assumes Prometheus has a scrape configuration that finds the annotations. From everything I've been able to find so far, the Prometheus Operator doesn't use that sort of configuration, so you'll have to manually add the necessary ports. So add port 8002 to the list of ports for the Envoy container using something like this:

```yaml
ports:
- containerPort: 8002
  hostPort: 8002
  name: metrics
  protocol: TCP
```

Next, add a sidecar container for the statsd-exporter. You can see this in [lines 52 through 64][link-5] in the example configuration from Gimbal. You'll also need to add a reference to a ConfigMap (see [lines 88 through 90][link-6]); the ConfigMap itself can be found [here][link-7]. Note that the Gimbal example uses the name "metrics" for port 9102 on the sidecar container; I chose to differentiate this using the name "statsd-exporter".

The final change is reflected [on line 75][link-8] of the Gimbal example.

Once all these changes have been made, you're ready to proceed to the next step.

## Modifying the Envoy Service

The changes here are pretty straightforward; you're just going to add a couple additional ports on the service:

```yaml
ports:
- port: 8002
  name: metrics
  protocol: TCP
- port: 9102
  name: statsd-exporter
  protocol: TCP
```

These should be the only changes to the Service that are needed.

## Creating a ServiceMonitor

Finally, we can create the Prometheus ServiceMonitor to tell Prometheus to scrape the newly-exposed ports to gather metrics. Here's an example ServiceMonitor definition you could use:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  labels:
    prometheus: kube-prometheus
  name: envoy
  namespace: monitoring
spec:
  selector:
    matchLabels:
      app: envoy
  namespaceSelector:
    matchNames:
      - ingress
  endpoints:
  - targetPort: 8002
    interval: 30s
    path: /stats/prometheus
  - targetPort: 9102
    interval: 30s
```

There are a few changes you might need to make to _your_ ServiceMonitor definition:

* Under `metadata.labels`, you'll want to change the value of the `prometheus` label to point to the Prometheus Operator instance in your cluster.
* You may need to specify a different namespace.
* Under `spec.selector.MatchLabels` and `spec.namespaceSelector`, you'll most likely need to specify different values here. _Make sure_ the labels listed under `spec.selector.MatchLabels` matches the label(s) on the Envoy Service, not the Envoy Pods.
* Note that the path of "/stats/prometheus" under `spec.endpoints` for port 8002 is _required_.

If you are using NetworkPolicies in your Kubernetes cluster, you'll want to be sure that your policies allow the appropriate traffic from Prometheus to the Envoy Pods.

## Applying the Changes

With all the changes in place, you're ready to use `kubectl` to apply the changes to the DaemonSet/Deployment and the Service, and then to create the ServiceMonitor. Assuming you don't have any typos (I almost always do!), you should be able to browse the Prometheus web interface and see your Envoy instances show up as targets for Prometheus. Now you're good to go!

If you have any questions or run into issues, feel free to [contact me on Twitter][link-99] and I'll do my best to help you out.

[link-1]: https://github.com/heptio/contour
[link-2]: https://www.envoyproxy.io/
[link-3]: https://prometheus.io/
[link-4]: https://github.com/heptio/gimbal/blob/master/deployment/contour/03-envoy.yaml
[link-5]: https://github.com/heptio/gimbal/blob/master/deployment/contour/03-envoy.yaml#L52-#L64
[link-6]: https://github.com/heptio/gimbal/blob/master/deployment/contour/03-envoy.yaml#L88-#L90
[link-7]: https://github.com/heptio/gimbal/blob/master/deployment/contour/02-statsd.yaml
[link-8]: https://github.com/heptio/gimbal/blob/master/deployment/contour/03-envoy.yaml#L75
[link-99]: https://twitter.com/scott_lowe
