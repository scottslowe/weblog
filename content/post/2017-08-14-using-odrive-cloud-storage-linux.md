---
author: slowe
categories: Tutorial
comments: true
date: 2017-08-14T12:00:00Z
tags:
- Linux
- CLI
- Storage
- Microsoft
- Fedora
title: Using ODrive for Cloud Storage on Linux
url: /2017/08/14/using-odrive-cloud-storage-linux/
---

A few months ago, I stumbled across a service called [ODrive][link-1] ("Oh" Drive) that allows you to combine multiple cloud storage services together. Since that time, I've been experimenting with ODrive, testing it to see how well it works, if at all, with my [Fedora Linux][link-3] environment. In spite of _very_ limited documentation, I think I've finally come to a point where I can share what I've learned.<!--more-->

Before I proceed any further, I do feel it is necessary to provide a couple of disclaimers. First, while I'm using ODrive myself, I'm _not_ using their paid (premium) service, even though it offers quite a bit more functionality. Why? Maybe this is a "chicken-and-egg" scenario, but I have a really hard time paying for a premium service where Linux client functionality is very limited and the documentation is extraordinarily sparse. (ODrive, if you're reading this: put some effort into your Linux support and your docs, and you'll probably get more paying customers.) Second, I'm providing this information "as is"; use it at your own risk.

OK, with those disclaimers out of the way, let's get into the content. For Linux users, [this page][link-2] is about the extent of ODrive's documentation. While this information is sufficient to briefly/quickly test out the ODrive Sync Agent, it doesn't provide any sort of documentation/recommendations for how to put ODrive to work. Based on the information on this one page and based on my own trial-and-error experience, here's some additional information you may find helpful.

First, when downloading and unpacking the Sync Agent and ODrive CLI binaries, I'd recommend putting them in a directory in your home folder. Fedora, for example, already has `~/.local/bin`in the PATH, so that might be a good location (it's what I'm using). It doesn't make any sense (in my opinion) to put them somewhere else, because the agent _must_ run in the same user context as the ODrive CLI binary in order for it to work. (More on that point in a second.)

Next, you're probably going to want to have the ODrive Sync Agent run automatically in the background. Their documentation doesn't describe this at all, and ODrive was mysteriously silent when I tried to reach them on social media for clarification/recommendations. I ended up using a systemd unit to have the Sync Agent run in the background. However, this has to be a "user-mode" systemd unit running in the context of your user account. On Fedora 25 (which runs systemd 231), that means using `systemctl --user` to manage the unit, and storing the unit in `~/.local/share/systemd/user`. (I'm not a systemd expert, so there may be other locations that are supported.)

Here's a sample systemd unit you could use:

```
[Unit]
Description=odrive Sync Agent daemon

[Service]
ExecStart=/path/to/your/home/dir/.local/bin/odriveagent

[Install]
WantedBy=default.target
```

Obviously, you'll want to customize the `ExecStart` location with the correct location of the binaries on your particular system. Save this systemd unit as `odriveagent.service` (or similar) in the appropriate location for user-mode units on your distribution (on Fedora 25, that's `~/.local/share/systemd/user`). Then, run `systemctl --user daemon-reload` to reload the systemd units, and then `systemctl --user start oddriveagent` to actually start the Sync Agent. Of course, you'll probably want to run `systemctl --user enable odriveagent` to have the ODrive Sync Agent unit start automatically when you log in.

Once this user-mode systemd unit is running, run `odrive status` to verify that the ODrive CLI binary is able to properly communicate with the Sync Agent, and then authenticate the ODrive Sync Agent using the instructions ODrive provides.

Another area that was very sparse and unclear in the documentation was around the use of `odrive mount`. The [Sync Agent page][link-2] only makes brief passing reference to creating a local directory and then using `odrive mount`. What _wasn't_ explained was the relationship between the various storage services that are mapped into ODrive and the "remote mount point" referenced when discussing `odrive mount`.

In ODrive's web interface, you'll connect ODrive to various storage services---[Google Drive][link-4], [Dropbox][link-5], [OneDrive][link-6], [Box][link-7], etc. The web interface doesn't make clear that these storage services are essentially mapped in as "subdirectories" under the root of your ODrive. By default, these "subdirectories" are named according to the storage provider. So, you map Google Drive into your ODrive, and it will create a "subdirectory" in the ODrive web interface named "Google Drive." Map OneDrive into ODrive, and you'll get a "subdirectory" in the web interface named "OneDrive". After working with ODrive for a little while, I also found that you can rename the "subdirectory" assigned to each storage service mapped into ODrive. (This, in my opinion, is a feature that should be made more clear.)

When you use `odrive mount`, you're creating the equivalent of a filesystem mount point, mounting a remote cloud storage provider onto a local directory. The example that ODrive provides on their web site says to use this command:

    odrive mount /path/to/local/dir /

This command mounts the "root" of the ODrive to a local path. Each of the storage services comes in as a subdirectory according to the name assigned (either by default or by you when you renamed it) found in the ODrive web interface. So, if you have Google Drive mapped in to ODrive and have named it GDrive, then you'll see a GDrive folder under the mount point that represents your Google Drive.

If you're like me, you'll probably start thinking about wanting to use multiple ODrive mount points, so that you can map storage services into your local filesystem in more flexible ways. For example, to create provider-specific mounts you could do something like this:

    odrive mount ~/Local-GDrive /GDrive
    odrive mount ~/AmazonDrive /AmznDrv

This is neat, _except for the fact that this is only a Premium feature._ (You have to have a paid subscription.) This is **not** clear from their documentation; it's only by trying it and failing will you discover this little nugget of information. After digging around for a while, I found a brief, unclear mention of this fact in their features comparison list. (By the way, I have _no_ problem with ODrive charging for premium features---my complaint is the incredibly sparse documentation and lack of clarity.)

Thus, if you're not paying for their service, you're limited to a single mount point, and therefore the _only_ use of an ODrive mount point that makes sense is to mount the root of the ODrive on your local filesystem.

So, you've got the binaries installed, a systemd agent running under your user account, you've authenticated your ODrive Sync Agent (per their instructions), and have an ODrive mount point configured. Now what? Well, since there's no GUI **whatsoever** on Linux, it's all CLI.

I'm a huge CLI fan (in case you hadn't guessed), so normally a CLI-only solution wouldn't be a problem. In fact, I've complained before about products that don't offer a good CLI in addition to their GUI (see [this post about switching to VirtualBox][xref-2] and [this post about a CLI for Dropbox][xref-1]). In the case of ODrive on Linux, though, the CLI is hobbled. For example, there's _no_ support for recursion or wildcards in the Linux ODrive CLI.

The reason the lack of support for recursion or wildcards/filename globbing is such an issue is because the ODrive Sync Agent won't, by default, automatically start syncing files to your local system from the cloud storage provider. In my case, I already had files in my OneDrive for Business (OD4B) account. ODrive will "see" the files and directories in OD4B, and will create placeholder files to represent them: a `.cloud` file for every file, and a `.cloudf` file for every directory. In order to actually sync these files and directories down to your system, you need to use the `odrive` command line against one of these placeholder files, like this:

    odrive sync cloud-folder.cloudf

This will then sync _this folder_ down to your local system, but not any files or subfolders in it. You'll need to repeat this process for subfolders, or for individual files in the folder. Naturally, you can see why recursion (syncing an entire directory tree) or filename globbing (grabbing all files) would be quite useful. Lack of support for either of these features means that there is a fair amount of manual work needed when you're adding a cloud storage service where there's already content present.

The good news is that if you add a file or folder locally, then that file or folder is automatically synced up to the cloud storage service as expected. It's only when content is added first on the cloud storage service that you have to manually use the `odrive sync` command to have it synchronize down to your Linux system.

There are some quasi-workarounds to this; you can, for example, write a small script that might make it easier to sync multiple files or folders from the command line. On Linux systems running GNOME (like my Fedora box), you can put scripts into `~/.local/share/nautilus/scripts` and actually have them accessible via a right-click on a file or folder in the GUI file manager. (I have a prototype script like this written, but I still need to do some additional testing/debugging).

## Summary

ODrive has the potential to be a very useful, particularly if you need to access services like OD4B where there is no native Linux client from the provider. However, its usefulness/usability is severely hampered by a lack of thorough documentation, no GUI functionality, and a hobbled CLI. That being said, if you can live with the current limitations, it may still win a place on your Linux system. (It has on mine.)

Feel free to hit me up on Twitter if you have questions, comments, or corrections about this post. Thank you for reading!



[link-1]: https://www.odrive.com/
[link-2]: https://docs.odrive.com/docs/odrive-sync-agent
[link-3]: https://getfedora.org/
[link-4]: https://www.google.com/drive/
[link-5]: https://www.dropbox.com/
[link-6]: https://onedrive.live.com/
[link-7]: https://www.box.com/
[xref-1]: {{< relref "2012-12-08-why-a-cli-for-dropbox-on-mac-os-x.md" >}}
[xref-2]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
