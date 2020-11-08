---
author: slowe
categories: Tutorial
comments: true
date: 2020-03-12T12:20:00-07:00
tags:
- Kubernetes
- macOS
- CLI
- YAML
title: "Updating Visual Studio Code's Kubernetes API Awareness"
url: /2020/03/12/updating-vscode-kubernetes-api-awareness/
---

After attempting (and failing) to get [Sublime Text][link-1] to have some of the same "intelligence" that [Visual Studio Code][link-2] has with certain languages, I finally stopped trying to make Sublime Text work for me and just went back to using Code full-time. As I mentioned in [this earlier post][xref-1], now that I've finally solved how Code handles wrapping text in brackets and braces and the like I'm much happier. (It's the small things in life.) Now I've moved on to tackling how to update Code's Kubernetes API awareness.<!--more-->

Code's awareness of the Kubernetes API comes via [the YAML extension by Red Hat][link-4], and uses the `yaml-language-server` project found in [this GitHub repository][link-5]. (This is the same language server I was trying to get working with Sublime Text via [LSP][link-6].) Note that I tested this procedure with version 0.7.2 of the extension; files may be in different locations or have different contents in other versions.

The information in this post is based heavily on [this article][link-3] by Josh Rosso, who some of you may know from his guest appearances on [TGIK][link-7]. I've adapted the information in Josh's post to apply to the macOS version of Visual Studio Code.

Here's how to update the extension to use a schema for Kubernetes 1.17 (the extension uses a schema for Kubernetes 1.14 by default):

1. With Code running, the extension installed and enabled, and a YAML file open, verify the location of the extension by running `ps auxwww | grep yaml-language-server`. The path should be something like `$HOME/.vscode/extensions/redhat.vscode-yaml-0.7.2/node_modules/yaml-language-server/out/server/src/` (where `$HOME` represents your home directory).
2. Close and exit Code. Although it shouldn't matter if it's running, I prefer not to take any chances.
3. Use an editor (other than Code!) to edit the `schemaUrls.js` file found in the `languageservice/utils` directory under the directory identified in step 1. The full path to `schemaUrls.js` should be something like `$HOME/.vscode/extensions/redhat.vscode-yaml-0.7.2/node_modules/yaml-language-server/out/server/src/languageservice/utils`.
4. Find the line that starts with `exports.KUBERNETES_SCHEMA_URL`, and change the Kubernetes version later in that line from `1.14.0` to `1.17.0`.
5. Save and exit the file, and then restart Code.

That's it---Code should now use an API schema for Kubernetes 1.17.0. Note that this still won't help Code be aware of CustomResourceDefinitions like those used by Cluster API, and so the YAML language server will still flag those lines as "errors." I haven't found a workaround for that yet, but I'll keep looking.

Hit [me up on Twitter][link-8] if you have any questions. I hope this helps!

[link-1]: https://www.sublimetext.com/
[link-2]: https://code.visualstudio.com/
[link-3]: https://octetz.com/docs/2020/2020-01-06-vim-k8s-yaml-support/#set-kubernetes-api-version
[link-4]: https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml
[link-5]: https://github.com/redhat-developer/yaml-language-server
[link-6]: https://lsp.readthedocs.io/en/latest/
[link-7]: https://github.com/vmware-tanzu/tgik
[link-8]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-03-08-modifying-visual-studio-code-bracketing-behavior.md" >}}