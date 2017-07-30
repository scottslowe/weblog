---
author: slowe
categories: Musing
comments: true
date: 2011-09-19T09:00:00Z
slug: determining-an-optimal-design
tags:
- Storage
- Virtualization
- VMworld2011
- vSphere
title: Determining an Optimal Design
url: /2011/09/19/determining-an-optimal-design/
wordpress_id: 2423
---

I had an interesting e-mail conversation last week with fellow EMC vSpecialist Clint Kitson. Now, if you've never met Clint, he's a super-sharp fellow, no questions about it. I love conversations with super-sharp folks as they invariably lead to new thoughts and new ideas, and this conversation was no different. The topic of the conversation, in generalized terms, was this: when crafting designs for customers, what defines an _"optimal design"_?

Allow me to share a few details from our conversation, and perhaps that question will make a bit more sense. Clint and I were discussing storage array designs and the various types of utilization that exist within a storage array. In this particular case, we were discussing a scenario in which an array---any type of array, not just an EMC array---could be configured in one of two ways:

1. You could configure the array with fewer drives than the storage controllers are actually capable of driving. Thus, there is a certain amount of storage controller capacity left unused, even if all the drives are fully utilized.

2. You could configure the array with more drives than the storage controllers are capable of driving. Thus, even if the storage controllers are fully utilized, there are some unused drive resources (IOPS and capacity, but mostly IOPS) left unused.

In either configuration, there is a resource (either storage controller capacity or drive capacity in IOPS) that is not fully utilized. So what's the optimal design in this sort of scenario? Either way, something is going to be under-utilized.

If you think about it, this same question could be asked of a VMware vSphere design. In almost every vSphere design, there is at least one resource that is not fully utilized. So what is an optimal design? Which resource should be left under-utilized? How does an architect---be it a storage architect creating storage array configurations or a virtualization architect creating virtualized infrastructure designs---determine which resources should be left under-utilized in order to create an optimal design?

If you attended my VMworld session (VSP1926), you probably already know what I think the answer is: functional requirements. Which approach best satisfies the customer's functional requirements? In the example that Clint and I were discussing, which approach will best meet the customer's needs and requirements?

"But Scott," you say, "either approach could satisfy the functional requirements."

That might be true (it's hard to say here since we were discussing this in a very abstract sort of way), but I'm almost certain that in every one of these situations one approach is more cost-effective than the others, and every project I've ever seen has a financial functional requirement (more commonly referred to as a "budget"). Can you still say all approaches meet the functional requirements equally?

This is a bit of an academic discussion, but an interesting one (in my mind, at least). What do you think? Share your thoughts, rants, and ideas in the comments below.
