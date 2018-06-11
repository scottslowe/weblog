---
author: slowe
categories: Education
comments: true
date: 2018-06-11T12:00:00Z
tags:
- Terraform
- AWS
- Automation
title: Using Variables in AWS Tags with Terraform
url: /2018/06/11/using-variables-in-aws-tags-with-terraform/
---

I've been working to deepen my [Terraform][link-1] skills recently, and one avenue I've been using to help in this area is expanding my use of Terraform modules. If you're unfamiliar with the idea of Terraform modules, you can liken them to [Ansible][link-3] roles: a re-usable abstraction/function that is heavily parameterized and can be called/invoked as needed. Recently I wanted to add support for tagging AWS instances in a module I was building, and I found out that you can't use variable interpolation in the normal way for AWS tags. Here's a workaround I found in my research and testing.<!--more-->

Normally, variable interpolation in Terraform would allow one to do something like this (this is taken from the `aws_instance` resource):

``` text
tags {
    Name = "${var.name}-${count.index}"
    role = "${var.role}"
}
```

This approach works, creating tags whose keys are "Name" and "role" and whose values are the interpolated variables. (I am, in fact, using this exact snippet of code in some of my Terraform modules.) Given that this works, I decided to extend it in a way that would allow the code calling the module to supply both the key as well as the value, thus providing more flexibility in the module. I arrived at this snippet:

``` text
tags {
    Name = "${var.name}-${count.index}"
    role = "${var.role}"
    "${var.opt_tag_name}" = "${var.opt_tag_value}"
}
```

The idea here is that the `opt_tag_name` variable contains a tag key, and the `opt_tag_value` contains the associated tag value.

Unfortunately, this doesn't work---instead of interpolating the value for `opt_tag_name`, the string was applied literally, and the value wasn't interpolated at all (it came through blank). I'm not really sure why this is the case, but after some searching I came across [this GitHub issue][link-4] that provides a workaround.

The workaround has 2 parts:

1. A local definition
2. Mapping the tags into the `aws_instance` resource

The local definition looks like this:

``` text
locals {
    common_tags = "${map(
        "${var.opt_tag_name}", "${var.opt_tag_value}",
        "role", "${var.role}"
    )}"
}
```

This sets up a "local variable" within the module (it's scoped to the module). In this case, it's a map of keys and values. One of the keys is determined by interpolating the `opt_tag_name` variable, and the value for that key is determined by the variable `opt_tag_value`. The second key is "role", and the value is taken by interpolating the `role` variable.

The second step references this local definition. In the `aws_instance` resource itself, you'll reference the local definition along with any additional tags like this:

``` text
tags = "${merge(
    local.common_tags,
    map(
        "Name", "${var.name}-${count.index}"
    )
)}"
```

This snippet of code sets up a new map that defines the tag with the "Name" key and the value taken from the variable `name` and suffixed by a count (derived from the number of instances being created). Then it uses the `merge` function to create a union of the two maps, which are then used by the AWS provider to set the tags on the AWS instance.

Taken together, this allows me to provide _both_ the tag name and the tag value to the module, which will then pass those along to the AWS instances created by the module. Now I just need to figure out how to make this truly _optional_, so that the module doesn't try to create a key-name value if no values are passed to the module. I haven't figured that part out (yet).

## More Resources

[Here's][link-2] the Terraform documentation on modules, though---to be honest---I haven't found it to be as helpful as I'd like.

[link-1]: https://www.terraform.io/
[link-2]: https://www.terraform.io/docs/modules/index.html
[link-3]: https://www.ansible.com/
[link-4]: https://github.com/hashicorp/terraform/issues/14516
