---
author: slowe
categories: Explanation
comments: true
date: 2024-08-14T09:00:00-06:00
tags:
- Encryption
- CLI
- Pulumi
- JSON
- YAML
title: Using SOPS with Pulumi
url: /2024/08/14/using-sops-with-pulumi/
---

I was first introduced to SOPS at a platform engineering event hosted in Denver last year. SOPS, which is an acronym for **S**ecrets **OP**eration**S**, describes itself as "an editor of encrypted files that supports YAML, JSON, ENV, INI and BINARY formats and encrypts with AWS KMS, GCP KMS, Azure Key Vault, age, and PGP" (taken directly from the project's GitHub repository). In this post, I'll explore using Pulumi with SOPS---and I'll also touch upon whether this combination of tools offers value or users or not.<!--more-->

SOPS is a command-line tool written primarily in [Go][link-2]; users can download binaries from [the SOPS GitHub repository][link-1] or, for Linux users, install it via their distribution's package manager. Depending upon the encryption mechanism you're going to use, you may also need to install other tools (for example, if you're going to use AWS KMS then you'll need the appropriate AWS tools installed and configured). I chose to use [Rage][link-3], a Rust-based implementation of [Age][link-4], as my encryption mechanism.

One of the things I find really interesting about SOPS is the fact that it supports data formats like YAML and JSON. What does this mean, exactly? When you use SOPS to encrypt a file in a supported syntax, SOPS will only encrypt the _values_, not the _keys_. For example, you can take a JSON file like this:

```json
{
    "PULUMI_CONFIG_PASSPHRASE": "supersecret"
}
```

And encrypt it with SOPS such that the key is in plain text but the value is ciphertext (it's an encrypted value), like this:

```json
{
    "PULUMI_CONFIG_PASSPHRASE": "ENC[AES256_CGM,data:<encrypted data>"
}
```

SOPS describes itself as an "editor of encrypted files"; it accomplishes this by decrypting the file on the fly, passing it to an editor (the editor is configurable; GUI editors such as Visual Studio Code or Sublime Text integrate but require the `-w` flag) for changes, and then re-encrypting the file upon closing. You can also encrypt or decrypt files on demand using the `-e` (to encrypt) or `-d` (to decrypt) flags with SOPS.

Pulumi doesn't offer built-in support for SOPS, so to allow the two tools to interoperate as expected you can use `sops exec-env`, which tells SOPS to pass values from the encrypted file as environment variables. This is one of two ways (the other being `sops exec-file`, which writes values to a temporary file) offered by SOPS to pass encrypted values to other processes. If you are familiar with Pulumi's ESC tool and its `esc run` functionality, you'll find `sops exec-env` to be comparable (aside from `esc`'s reliance on Pulumi's SaaS offering).

Let's say that for a particular project I wanted to configure Pulumi's backend using the `PULUMI_BACKEND_URL` environment variable. I could specify a backend in a SOPS-encrypted JSON file, then pass it to Pulumi like this:

```bash
sops exec-env encrypted-config-file.json 'pulumi preview'
```

When invoked in this way, Pulumi will use the decrypted value of `PULUMI_BACKEND_URL` passed to it by SOPS. You could use this technique to pass any of [the supported environment variables][link-5] used to configure the Pulumi CLI.

You could also have your Pulumi program consume the environment variables supplied by `sops exec-env`; for example, in Go you could use the `os.Getenv()` function to pull environment variables into your code. I won't bore you with all the details, but do let me know if you're interested in seeing more.

If you decide to go down this route, just remember that you'll need to preface _every_ Pulumi command with `sops exec-env`. It's not a problem, necessarily, but it will take a bit of getting accustomed to doing this.

Here's the real question, though: is this useful? Is it worthwhile to combine SOPS with Pulumi? What benefits do you gain, if any?

Between using environment variables to configure the Pulumi CLI and passing environment variables directly into your Pulumi programs (using `os.Getenv` in Go, for example), you can bypass any dependency on Pulumi's built-in configuration values functionality (i.e., `pulumi config set`). But is it worthwhile? Does it offer any real value over just using Pulumi's existing functionality?

Being perfectly honest, using SOPS instead of Pulumi's existing functionality isn't hugely life-changing. Pulumi already has the ability to store configuration files as "secrets" in the configuration file, and it handles secret/sensitive values in the project state as well. For a tool like [Terraform][link-6] (or [OpenTofu][link-7]) that lacks this functionality, then I could see a great deal of added value in adding SOPS to the toolchain.

I would file this combination under "Interesting But Not Necessarily Useful". Perhaps I missed something? If you're well-versed in SOPS, please reach out to me and let me know if there's something more I should consider here. You can reach me [on Twitter][link-8], on [the Fediverse][link-9], or via one of a variety of Slack communities I frequent. I'd love to hear from you.

[link-1]: https://github.com/getsops/sops
[link-2]: https://go.dev/
[link-3]: https://github.com/str4d/rage
[link-4]: https://github.com/FiloSottile/age
[link-5]: https://www.pulumi.com/docs/cli/environment-variables/
[link-6]: https://terraform.io
[link-7]: https://opentofu.org/
[link-8]: https://twitter.com/scott_lowe
[link-9]: https://fosstodon.org/@scottslowe
