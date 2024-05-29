---
author: slowe
categories: Tutorial
comments: true
date: 2024-05-29T08:30:00-06:00
tags:
- CLI
- Encryption
- Linux
- SSL
- TLS
title: Getting Barrier Working Between Arch Linux and Ubuntu
url: /2024/05/29/getting-barrier-working-between-archlinux-and-ubuntu/
---

I recently had a need to get Barrier---an open source project aimed at enabling mouse/keyboard sharing across multiple computers, aka a "software KVM"---running between Arch Linux and Ubuntu 22.04. Unfortunately, the process for getting Barrier working isn't as intuitive as it should be, so I'm posting this information in the hopes it will prove useful to others who find themselves in a similar situation. Below, I'll share how I got Barrier working between an Arch Linux system and an Ubuntu system.<!--more-->

Although this post specifically mentions [Arch Linux][link-3] and [Ubuntu][link-4], the process for getting [Barrier][link-2] running should be pretty similar (if not identical) for other Linux distributions and for macOS. I don't have any Windows-based systems on which to test these instructions, but they should be adaptable to Windows as well. Note that there may be slight differences in the flags for the commands listed here when they are run on platforms other than Linux.

## Installing Barrier

Both Arch and Ubuntu 22.04 have the latest release of Barrier, version 2.4.0, available in their repositories, so the installation is straightforward.

For Arch, just install with `pacman`:

```bash
pacman -Ss barrier
```

There's also a "barrier-headless" package in Arch that has _only_ the binaries and not the GUI component.

For Ubuntu, you'd just `apt`:

```bash
apt install barrier
```

There does not appear to be a non-GUI package available for Ubuntu.

## Fixing Barrier's SSL/TLS Configuration

The core of the issue with Barrier is in its SSL/TLS configuration. Barrier expects certain SSL/TLS assets to exist, but doesn't create those assets automatically. To make matters worse, the documentation doesn't indicate that these assets need to be manually created. Fortunately, once you _do_ create the assets, Barrier seems to work as expected.

On the server (this is the system that will be sharing the mouse and keyboard), you'll want to create an SSL/TLS certificate. Here's the `openssl` command you need to use:

```bash
openssl req -x509 -nodes -days 365 -subj /CN=Barrier -newkey rsa:4096 -keyout Barrier.pem -out Barrier.pem
```

As far as I am aware, the subject _must_ match what's described above, but the duration of the certificate (365 days) and the key length (4096 bits) can be varied if desired. A quick review of the project's source code indicates that the file _must_ be named `Barrier.pem`, and that the file _must_ be stored (on Linux, at least) in the `$HOME/.local/share/barrier/SSL` directory. You'll likely need to create the `SSL` directory yourself.

Repeat this step on each client (a client is a system that will be controlled by the keyboard and mouse connected to the server) you'll be using with Barrier. You can use the same command as above, and the same restrictions/limitations apply.

At this point, all systems should have an SSL/TLS certificate.

Next, on both the server and all clients, create a subdirectory in the `$HOME/.local/share/barrier/SSL` directory called `Fingerprints`. You'll use this directory to store a file used by Barrier that contains the SHA256 fingerprint of the SSL/TLS certificate.

To generate the SHA256 fingerprint of the SSL/TLS certificate, you can use this command (the below command assumes you are running it from the `$HOME/.local/share/barrier/SSL` directory where `Barrier.pem` is found):

```bash
openssl x509 -fingerprint -sha256 -noout -in Barrier.pem | cut -d"=" -f2
```

On every system (the server and all clients), store this SHA256 fingerprint in the `Fingerprints` directory in a file named `Local.txt`. You can use shell redirection like this:

```bash
openssl x509 -fingerprint -sha256 -noout -in Barrier.pem | cut -d"=" -f2 > Fingerprints/Local.txt
```

Alternately, you can store the fingerprint as a shell variable and then use the shell variable later. Given that there's one more small, mostly undocumented detail, you might prefer this second approach:

```bash
FINGERPRINT=$(openssl x509 -fingerprint -sha256 -noout -in Barrier.pem | cut -d"=" -f2)
```

The "small, mostly undocumented" detail is [described in the Arch Linux wiki][link-1]; that's fortunate because it didn't appear to be documented _anywhere_ else. Barrier expects the text "v2:sha256:" to precede the SHA256 fingerprint in `Local.txt` (and some other files you'll create shortly).

With that in mind, using a shell variable to store the SHA256 fingerprint is useful because you can just do this after running the previous command:

```bash
echo "v2:sha256:$FINGERPRINT" > Fingerprints/Local.txt
```

If you chose the first approach, just edit the file to add "v2:sha256:" before the SHA256 fingerprint in the file.

At this point, all systems have an SSL/TLS certificate and all systems have the SHA256 fingerprint of that certificate in a file, prepended with "v2:sha256:". Make sure both of these steps are complete before proceeding!

The next steps are different for the server and for the clients.

### On the Barrier Server

The Barrier server needs to be informed of the SHA256 fingerprints for **every client that will connect to the server.** Fortunately, each client has a `Local.txt` file that has the fingerprint along with the required "v2:sha256:" text.

Take the contents of each client's `Local.txt` file and append it to a file on the server named `TrustedClients.txt`. This file should reside in the `Fingerprints` subdirectory alongside the server's `Local.txt`. When you're finished, the `TrustedClients.txt` file will contain a line, starting with "v2:sha256:" and followed by the SHA256 fingerprint, for each and every client that will connect to the server. If you have two clients, then you'll have two fingerprints in this file. If you have five clients, then you'll have five fingerprints in this file. If you have only a single client, then this file will have only a single fingerprint. There are numerous ways to get the client fingerprint files over to the server; choose whichever method you prefer.

### On the Barrier Client

Just as the Barrier server needs to know about the SHA256 fingerprints of the clients, the clients need to know about the SHA256 fingerprint of the server. This information should be placed in a file named `TrustedServers.txt` in the `Fingerprints` subdirectory, alongside the client's `Local.txt` file.

You have (at least) two options for making this happen. First, you can copy the `Fingerprints/Local.txt` file from the server to each client. To be honest, this is probably the easiest way.

The second way is to use `openssl s_client` to retrieve the certificate from the server and generate the fingerprint. (The Barrier server needs to be running for this to work.) The command looks something like this (the command below assumes it is being run from the `$HOME/.local/share/barrier/SSL` directory):

```bash
echo -n | openssl s_client -connect <hostname>:24800 2>/dev/null | \
    openssl x509 -fingerprint -sha256 -noout | cut -f2 -d'=' > Fingerprints/TrustedServers.txt
```

Whichever approach you choose, the end result is the same: the fingerprint of the **server's** SSL/TLS certificate should be in a file named `TrustedServers.txt` in the `Fingerprints` subdirectory. Repeat this process on all clients.

At this point:

1. Each system should have an SSL/TLS certificate, named `Barrier.pem`, found in the correct location (on Linux systems that's `$HOME/.local/share/barrier/SSL`).
2. Each system should have a file named `Local.txt` that contains the SHA256 fingerprint of this certificate, preceeded by the text "v2:sha256:". This file should reside in the `Fingerprints` subdirectory (so the full path on a Linux system should be `$HOME/.local/share/barrier/SSL/Fingerprints`).
3. The Barrier server should have a `TrustedClients.txt` file in the `Fingerprints` subdirectory that contains the contains of each and every client's `Local.txt` file (a separate line for each client).
4. All Barrier clients should have a `TrustedServers.txt` file in the `Fingerprints` subdirectory that contains the contents of the server's `Local.txt` file.

If this is not the case, then go back and repeat the necessary step(s). Once all of the above statements are true, you're ready to run Barrier on the server and on all the clients, and the connections between the server and the clients will be authenticated (only trusted clients will be able to connect, and only to trusted servers) and encrypted.

## Summary

Let's summarize the steps required to make Barrier work---all the stuff described above:

1. Generate a `Barrier.pem` SSL/TLS certificate on the server and on each client. Store that certificate in the `$HOME/.local/share/barrier/SSL` directory (which you will likely need to create). Note that the path may be different on non-Linux systems!
2. On all systems involved (server and each client), put the SHA256 fingerprint of the certificate---prepended by the text "v2:sha256:"---into the `Fingerprints` subdirectory of the `SSL` directory where the certificate is found. Use the filename `Local.txt`.
3. On each client, add the server's SHA256 fingerprint into a file named `TrustedServers.txt` in the `Fingerprints` subdirectory (alongside `Local.txt`). Every fingerprint needs to have the text "v2:sha256:" preceeding the fingerprint, one line per trusted server.
4. On the server, add _all the clients'_ SHA256 fingerprints into a file named `TrustedClients.txt` in the `Fingerprints` subdirectory (alongside `Local.txt`). Every fingerprint needs to have the text "v2:sha256:" preceeding the fingerprint, one line per trusted client.

Once these steps are complete, you should be able to launch Barrier on the server and on the clients and proceed without further issues. (You'll still need to handle things like the Barrier configuration file on the server, though---that isn't addressed by these steps.)

## Closing Notes

It's worth noting that Barrier appears to no longer be maintained, and the replacement project (known as [Input Leap][link-5]) isn't quite ready for regular use yet. The following quote is from the Input leap repository's README:

> But for now, we advise sticking with Barrier v2.4.0/v2.3.4&#8230;

Keep this in mind if you decide you want to try/use Barrier yourself.

I hope this information is useful for folks. I had to spend time combing GitHub issues and spelunking through the code to assemble the information above, but there's still no guarantee that I have it all correct. If you see an error or a mistake, let me know so I can fix it! Feel free to reach out to me [on Twitter][link-6], in [the Fediverse][link-7], or via one of the many Slack communities I frequent. All constructive feedback is welcomed.

[link-1]: https://wiki.archlinux.org/title/Input_Leap#Set_up_encryption_on_server
[link-2]: https://github.com/debauchee/barrier
[link-3]: https://archlinux.org/
[link-4]: https://ubuntu.com/
[link-5]: https://github.com/input-leap/input-leap
[link-6]: https://twitter.com/scott_lowe
[link-7]: https://fosstodon.org/@scottslowe
