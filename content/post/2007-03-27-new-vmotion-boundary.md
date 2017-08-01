---
author: slowe
categories: Explanation
comments: true
date: 2007-03-27T11:14:27Z
slug: new-vmotion-boundary
tags:
- Hardware
- Virtualization
- VMotion
- VMware
title: New VMotion Boundary
url: /2007/03/27/new-vmotion-boundary/
wordpress_id: 434
---

In an earlier article about [VMotion compatibility][1], I mentioned that SSE support was a [VMotion](http://www.vmware.com/products/vi/vc/vmotion.html) boundary, i.e., a host with SSE3 CPUs and a host with SSE2 CPUs would not be able to VMotion guests between them without first masking the SSE support bit. I used this technique myself in our lab (more info in [this article][2]), and this issue drew some attention as the result of a presentation at VMworld last year.

Now, according to [this posting on the VMTN forums](http://www.vmware.com/community/thread.jspa?threadID=50828&messageID=608820#608820), the introduction of the Intel quad-core CPUs has introduced another VMotion boundary, and that is the introduction of SSE4. What this means is that you won't be able to seamlessly integrate both dual-core and quad-core CPUs into a DRS cluster, because VMotion won't work between these CPUs. The only way to move systems from a dual-core SSE3 CPU to a quad-core SSE4 CPU will be a cold migration. Having now been spoiled by live migrations with VMotion, I suspect a lot of administrators will be more than a bit perturbed by this.

It's odd that this comes up---I was just speaking to some customers a couple of days ago about VMotion boundaries and CPU compatibility, and I commented that I was not aware of any major developments in CPU capabilities that might introduce new VMotion boundaries. I guess I was wrong!

I have a feeling that [VMware](http://www.vmware.com/) is going to need to resolve this kind of issue sooner rather than later. As the hardware vendors race with each other to add new features and new virtualization support, we are going to see more and more artificial VMotion boundaries being introduced, and this will eliminate much of the flexibility that VMware currently offers organizations since they will have to resort to buying exactly identical systems. The sooner that VMware can move into a position of being able to support custom CPU masks, the better off they'll be in my opinion.

[1]: {{< relref "2006-11-23-vmotion-compatibility.md" >}}
[2]: {{< relref "2006-09-25-sneaking-around-vmotion-limitations.md" >}}
