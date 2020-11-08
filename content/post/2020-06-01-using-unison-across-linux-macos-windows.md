---
author: slowe
categories: Explanation
comments: true
date: 2020-06-01T09:40:00-07:00
tags:
- Linux
- macOS
- Windows
title: "Using Unison Across Linux, macOS, and Windows"
url: /2020/06/01/using-unison-across-linux-macos-windows/
---

I recently wrapped up an instance where I needed to use the [Unison file synchronization application][link-1] across Linux, macOS, and Windows. While Unison _is_ available for all three platforms and _does_ work across (and among) systems running all three operating systems, I did encounter a few interoperability issues while making it work. Here's some information on these interoperability issues, and how I worked around them. (Hopefully this information will help someone else.)<!--more-->

The use case here is to keep a subset of directories in sync between a MacBook Air running macOS "Catalina" 10.15.5 and a Surface Pro 6 running Windows 10. A system running Ubuntu 18.04.4 acted as the "server"; each "client" system (the MacBook Air and the Surface Pro) would synchronize with the Ubuntu system. I've used a nearly-identical setup for several years to keep my systems synchronized.

One thing to know about Unison before I continue is that you need compatible versions of Unison on both systems in order for it to work. As I understand it, compatibility is not just based on version numbers, but also on the Ocaml version with which it was compiled.

With that in mind, I already had a working setup using Unison 2.48 so I started there. Unison 2.48.4 was installed and running on the Ubuntu system, and I installed Unison 2.48.15 on the new MacBook Air. I'd used Unison 2.48.15 on macOS for quite a while, so I didn't test the installation right away, instead moving on to the Surface Pro. From this page hosting [Windows binaries for Unison][link-2], I downloaded a Unison 2.48.4 binary for Windows. Should be all set, right?

Unfortunately, I ran into a couple problems:

* The Windows binary has [an issue][link-3] where it won't recognize the preinstalled OpenSSH binary on Windows 10. So, I had to copy `ssh.exe` to the same directory as the Unison binary.
* The Windows binary didn't like paths with spaces in the name; no style of quoting seemed to help. The only workaround I could find was to use `dir /X` to get the auto-generated short name, and use that in the Unison profile.

Once past those two issues I managed to successfully get Unison to synchronize files between the Surface Pro and the Ubuntu system. Moving on to the MacBook Air---which I honestly suspected would be the easy step---I found Unison 2.48 crashed on macOS 10.15. Nothing would make it run without crashing.

Some continued research led me to find Windows and macOS builds of a newer version of Unison, version 2.51. The changelog referenced APFS support, which is what was being used on the MacBook Air. That should do it, right?

* Unison 2.51 wouldn't interoperate with the existing Unison 2.48 binary on the Ubuntu system. (Recall that I mentioned earlier that compatible versions of Unison were needed on both systems.)
* There were no packaged versions of Unison 2.51 for Ubuntu.

Fortunately, cloning [the GitHub repository][link-4] and building from source was pretty straightforward. I changed the filename of the new version (I used `unison-2.51.2`) and changed the "servercmd" setting in the Unison preferences on both the Surface Pro and the MacBook Air. Success! I was able to run Unison to synchronize files between the Surface Pro, the Ubuntu system, and the MacBook air running macOS 10.15.

Lessons learned?

1. Unison's support on Windows for paths with spaces is tricky. Use `dir /X` to get the auto-generated short name and use that instead.
2. On Windows 10, you'll need to copy `ssh.exe` from `C:\Windows\System32\OpenSSH` to the directory where the Unison executable resides. Otherwise, Unison will be unable to initiate the SSH connection to the remote system.
3. If you're running macOS 10.15, be sure to use Unison 2.51. (This _may_ apply to all systems running APFS, but I haven't verified that yet.)
4. Pre-compiled binaries for Unison 2.51 are available for both Windows and macOS, so that's probably the best version to use. For Linux, it's very likely you'll need to build from source in order to get a version 2.51 binary.

I hope this information is helpful to others who may need to get Unison working across multiple systems. If you have corrections, suggestions for improvement, or feedback on how I can improve this post, please contact [me on Twitter][link-5]. Thanks!

[link-1]: https://www.cis.upenn.edu/~bcpierce/unison/index.html
[link-2]: https://www.irif.fr/~vouillon/unison/
[link-3]: https://github.com/bcpierce00/unison/issues/218
[link-4]: https://github.com/bcpierce00/unison/
[link-5]: https://twitter.com/scott_lowe
