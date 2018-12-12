---
author: slowe
categories: Liveblog
comments: true
date: 2018-12-12T12:00:00Z
tags:
- KubeCon2018
- Kubernetes
title: KubeCon 2018 Day 2 Keynote
url: /2018/12/12/kubecon-2018-day-2-keynote/
---

This is a liveblog of the day 2 (Wednesday) keynotes at KubeCon/CloudNativeCon 2018 in Seattle, WA. For additional KubeCon 2018 coverage, check out other articles tagged [KubeCon2018][link-1].<!--more-->

Kicking off the day 2 keynotes, Liz Rice takes the stage at 9:02am (same time as yesterday, making me wonder if my clock is off by 2 minutes). Rice immediately brings out Janet Kuo, Software Engineer at Google and co-chair with Rice of the KubeCon/CloudNativeCon event program. Kuo will be delivering a Kubernetes project update.

Kuo starts off by reiterating the announcement of the Kubernetes 1.13 release, and looking back on her very first commit to Kubernetes in 2015 (just prior to the 1.0 release and the formation of the CNCF). Kuo talks about how Kubernetes, as a software cycle, has matured through the cycle of first focusing on innovation, then expanding to include scale, and finally expanding again to include stability (critical for enterprise adopters).

Reviewing usage details, Kuo states that she believes Kubernetes has moved---in the context of the technology adoption curve---from early adopters to early majority, the first phase in the mainstream market (and, for those who think in these terms, has crossed the chasm). However, this also means that Kubernetes has gotten _boring_.

In looking at Kubernetes, Kuo draws out two key facets of Kubernetes: open standards and extensibility. Open standards includes, according to Kuo, built-in APIs and conformance; these provide consistent behavior around expectations for developers deploying applications or developing applications to be deployed onto Kubernetes. Extensibility has two parts: infrastructure extensibility, and API extensibility.

Infrastructure extensibility is all about how Kubernetes consumes the underlying infrastructure; this would include, for example, cloud provider integrations, storage plugins (CSI), networking plugins (CNI), and the container runtime (CRI). API extensibility is mostly encompassed by Custom Resource Definitions (CRDs) and controllers (automation engines). Kuo uses Istio as an example of using API extensibility to create Istio-specific resources. This allows you to, according to Kuo, build "everything the Kubernetes way."

Recapping her presentation, Kuo reminds attendees that boring is essential for building a platform upon which other solutions can be built.

At this point, Rice returns to the stage to introduce Jason McGee, who is CTO for IBM Cloud and an IBM Fellow. McGee emphasizes that good application design is always a tradeoff of attributes; it's not just about stateless applications. McGee states that the cloud-native efforts have, thus far, focused only on 12-factor applications, functions, and serverless. However, applications are more than just these areas. This leads McGee to review developments in the Cloud Foundry space and the landscape around functions. The functions landscape, McGee stresses, is terribly fragmented and is holding the entire industry and community from moving forward. What's the answer? McGee moves into a discussion of Knative, and how Knative provides a single, unified platform that supports both containers, functions, and applications---all on top of Kubernetes.

McGee shifts into a product pitch talking about IBM Cloud and its use of Kubernetes before he wraps up his portion of this morning's keynotes.

Rice returns to the stage again, this time not to introduce someone else but to provide her own keynote focused---naturally, given her role at Aqua Security---Kubernetes security. Asking the question, "Don't we have security people for that?", Rice says she believes that _everyone_ can do something to help improve security; it's not just for security-dedicated people. Reiterating that every system has vulnerability, Rice reminds attendees that they should not be surprised or afraid that Kubernetes will have vulnerabilities discovered from time to time. (And she reminds everyone to update their clusters.) So what can attendees do to help improve security?

This leads Rice into a discussion of other security weak points that might exist in a Kubernetes deployment. She shows the example of just copying some YAML and pasting it into a `kubectl` command without any knowledge of what that YAML _actually_ does. She expands that example into showing how a single compromised Pod in turn exposes the Kubernetes API (via the default service account) and thus the entire cluster. Naturally, the answer here is to not just blindly apply YAML to our clusters. Rice next shows how she wrote a Validating Admission Webhook and associated controller/service that checks for and blocks the creation of service accounts.

This leads Rice to a discussion of Open Policy Agent (OPA), a CNCF Sandbox project that can act as the controller/service behind a Validating Admission Webhook to perform the same types of functions that Rice just showed (like blocking the creation of service accounts). This can enable users to load rulesets for OPA in as ConfigMaps to enforce policy decisions like blocking the creation of service accounts. Rice reminds attendees that OPA is still a relatively young project, but says she believes it's pretty important to watch.

So what sorts of things could users do with OPA to help improve Kubernetes security?

* Ensuring that only allowed registries are leveraged
* Checking to ensure that only scanned (and safe) images can be deployed
* Verifying software provenance to ensure that images are actually the images you want/expect

However, trusted code can still be bad, Rice reminds the audience. Open source libraries may change governance and "bad" code may be introduced (Rice uses the example of an NPM library compromised to include crypto-mining code). This underscores the importance of open source governance and the role of foundations such as the Linux Foundation and the CNCF in that governance.

As she wraps up her portion of the keynotes, Rice drops in a quick plug for the security book she recently wrote with Michael Hausenblas.

Rice now brings out Melanie Cebula from AirBnB to talk about AirBnB's use of Kubernetes (which now hosts about 40% of all of AirBnB's services). Cebula reviews how AirBnB has been moving away from a monolithic architecture to Kubernetes, and how the AirBnB team is now handling 125,000 deployments annually. More helpfully---in my opinion---is the portion of Cebula's presentation on addressing key challenges with Kubernetes. For example, AirBnB leveraged Go templating to make it easier to reduce YAML boilerplate. (Other examples in this space include Helm, kustomize, and kapitan.) Cebula also reviews how AirBnB generates boilerplate for new services, and how she used that to help enforce/encourage the use of documentation, testing, CI/CD, etc. Finally, Cebula talks about how AirBnb wrapped the use of `kubectl` to make it easier for developers and standardizes (again) things like namespaces and environments. Cebula does something lots of presenters often fail to do, and that's clearly summarize the recommendations out of the various portions of her presentation. All in all, this was an excellent presentation, full of very practical information learned from real-world use of Kubernetes.

Rice returns to the stage to wrap up the keynotes and remind folks of the evening events, and then closes the keynotes.

[link-1]: /tags/kubecon2018/
 