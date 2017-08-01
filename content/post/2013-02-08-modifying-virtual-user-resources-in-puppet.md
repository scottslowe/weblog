---
author: slowe
categories: Explanation
comments: true
date: 2013-02-08T13:22:12Z
slug: modifying-virtual-user-resources-in-puppet
tags:
- Automation
- Linux
- Puppet
title: Modifying Virtual User Resources in Puppet
url: /2013/02/08/modifying-virtual-user-resources-in-puppet/
wordpress_id: 3093
---

In an earlier post on [using Puppet to manage user accounts][1], I showed how I was using virtual user resources to manage user accounts on Puppet-managed nodes. Today, I ran into a situation where I wanted one of these virtual user resources to have a different group membership on one class of nodes than on another class of nodes. In this post, I'll show you how I managed to modify the group membership of a realized virtual user resource.

(Disclaimer: There are probably better, more elegant ways of solving this problem. The purpose of this post is to help stimulate discussion and encourage learning, not to say this is the only way to do something. Keep that in mind as you read.)

To refresh your memory, this is how the virtual user resources were being realized on individual nodes:

``` puppet
node default {
}

node 'server.domain.net' {
  include accounts
  realize (Accounts::Virtual['johndoe'])
}
```

Note the use of capitalized "Account::Virtual", which means we are referring to a specific, defined (virtually defined, at least) resource.

My first attempt was to try overriding, where I refer back to the realized user resource using a similar syntax, then specify a particular attribute (the `groups` attribute, in this case). I'd used this technique in the past with non-virtual resources (example [here][2]), but---unfortunately---it didn't seem to work with virtual resources.

A web search took me to [this Google Groups thread](https://groups.google.com/forum/?fromgroups=#!topic/puppet-users/e9b53Eyq2Fw), where I found two critical pieces of information:

1. Realizing apparently doesn't allow overrides.

2. You can use the collection function to modify realized resources.

Armed with that information, I added this code to the `nodes.pp` manifest to modify the group membership for a realized virtual resource on specific nodes:

``` puppet
User <| title == 'johndoe' |> {
  groups              =>  'othergroup',
}
```

That worked! On the nodes where this code was added, the account named "johndoe" was added to the group "othergroup" successfully.

Great, so this solution works, but really the bigger questions are these:

* Are there better, more elegant ways of solving this particular problem? 

* What drawbacks are there to this solution?

Yes, I could modify the `accounts::virtual` class to include supplementary group memberships, and that's probably a better long-term solution. However, that requires more testing, since you're modifying code that affects user accounts across a number of systems. This solution, on the other hand, is pretty quick and relatively safe.

Thoughts? Ideas? I'd love to hear your feedback, so speak up in the comments below.


[1]: {{< relref "2012-11-25-using-puppet-for-account-management.md" >}}
[2]: {{< relref "2012-07-05-using-puppet-with-multiple-operating-systems.md" >}}
