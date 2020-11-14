---
author: slowe
categories: Explanation
comments: true
date: 2011-11-21T09:00:00Z
slug: some-oss-with-my-mac-part-2-unison
tags:
- macOS
- Storage
- OSS
title: 'Some OSS with my Mac, Part 2: Unison'
url: /2011/11/21/some-oss-with-my-mac-part-2-unison/
wordpress_id: 2472
---

(This is Part 2 of a two-part series on some open source software I've incorporated into my primarily Mac-based home office setup. Read [Part 1][1].)

In Part 1 of this series, I talked about how I'd leveraged Synergy to provide  a shared mouse and keyboard across the three computers (two Macs and one Linux laptop) in my home office. In this part, I will discuss the solution I employ for keeping files synchronized between the two Macs in my home office. I'll give you a hint: It's not [Dropbox](http://www.dropbox.com/).

No, it's another open source application. It's called [Unison](http://www.cis.upenn.edu/~bcpierce/unison/) (not to be confused with the [Mac Usenet reader of the same name](http://www.panic.com/unison/)), and it provides intelligent two-way synchronization of files between multiple computers across multiple platforms. In my case, I use it only to keep files synchronized between my two Mac laptops, but this is not due to a limitation in Unison; this is only because my Linux laptop is used for other purposes and I don't really benefit from having all these documents available on it.

Specifically, I used a pre-compiled version of Unison available [here](http://alan.petitepomme.net/unison/index.html) for Mac OS X. While this provides a pretty GUI for Unison, there are some oddities to getting it to work as expected that I wanted to document here.

First, let's take a quick look at "the big picture" for getting this running:

1. Configure public key SSH authentication between the computers involved. (More on this in a moment.)

2. Install Unison. (This is as simple as mounting the .DMG and copying Unison into the destination of your choice. I put it in `/Applications/Utilities`.)

3. Create the Unison profiles. The Unison GUI only gets you part of the way there---this is one of the oddities.

## Configuring SSH Public Key Authentication

I'm not going to go into a lot of detail here; there are tons of sites that provide excellent instructions on how to create a public/private key pair and use that key pair for SSH authentication (here's [one](http://www.petefreitag.com/item/532.cfm)). The primary goal for which we are striving is passwordless logins for Unison, so that it can leverage SSH to encrypt the connections between the computers during file synchronization operations. (You _do_ want secure file synchronization, right?)

In my case, I'm leveraging what's known as a "star" topology. That is, my two Mac laptops (a 2011 13" MacBook Pro and a 2009 15" MacBook Pro) don't synchronize directly with each other; they synchronize through a third computer, which acts as the "server" in the center of my three-node "star." Why this topology? I didn't/don't want to have to enable SSH connections into either of my Mac laptops, but I already have SSH connections established to the server. (In this case, the server is a Mac Mini running Snow Leopard Server.)

For my setup, then, I needed to configure public key authentication between the laptops and the Mac Mini. For your setup, it will depend upon your topology, and that will drive the specific steps you must take.

## Installing Unison

As I mentioned above in the overview, installing Unison is like installing any other Mac app: mount the .DMG, copy the application to the desired location. The prebuilt Mac binaries of Unison that I used are no different, so I'll skip any more details on that step and move instead to creating the Unison profiles.

## Creating the Unison Profiles

While the Unison GUI provides a very sparse UI for creating profiles, you will _most assuredly_ need to supplement the GUI with editing the profile yourself by hand. Profiles created by Unison are stored in `~/Library/Application Support/Unison` as *.prf files that can be edited by any text editor (I use [TextMate](http://macromates.com/)).

Follow this general procedure to get a Unison profile set up properly:

1. Launch Unison and create the profile. Supply the necessary information (the local root, remote username, remote host, and remote root).

2. Quit Unison.

3. Navigate to `~/Library/Application Support/Unison` and open the .PRF file that corresponds to the profile you just created.

4. **At the very least**, you must add a `servercmd` statement to the profile, or Unison won't work at all (you'll get "fatal error"-type message about not being able to connect to the server). This is where I was stuck for quite some time---see below for more information.

5. Launch Unison, select the profile you just edited, and open it. It should now work.

Now, let's talk about the `servercmd` statement. This statement tells Unison where to find the Unison executable on the remote host. With the prebuilt binaries of Unison that I use (and these are the binaries to which most Google searches will point you, found [here](http://alan.petitepomme.net/unison/index.html)), it will prompt you to install a command-line tool. If you opt to install this command-line tool, it places an executable at `/usr/bin/unison`. Naturally, you would think that this would be the value you'd specify for `servercmd`, right? You'd be wrong.

The executable at `/usr/bin/unison` simply launches the Unison GUI. If you try to run that via SSH (as Unison will attempt to do when you try to open the profile), it will report an error---you can't launch a GUI app on a remote system via SSH. This is the root cause of the "unable to connect" or "fatal error" or "server not responding" error messages with Unison: it doesn't know how/where to find the server-side executable.

The fix is to specify the full path the Unison executable that is buried inside the Mac application package. Let's say you install Unison (which is an application bundle; bundles look like folders at the command line) to `/Applications/Utilities`, like I did. In that case, the right binary to point to is this one:

	/Applications/Utilities/Unison.app/Contents/MacOS/Unison

When you specify that value for the `servercmd` statement in the Unison profile, everything works (assuming that your public key authentication via SSH is working as expected).

Therefore, you have two options to make Unison work. Both options involve editing the .PRF file and adding a `servercmd` statement:

1. In Option 1, you specify the full path to the Unison executable inside the application bundle (like specified above) in the `servercmd` statement.

2. In Option 2, you create a symlink between `/usr/bin/unison` and the full path to the Unison executable, and specify `/usr/bin/unison` for the `servercmd` statement. This is the route I chose.

I highly recommend you read [this page](https://alliance.seas.upenn.edu/~bcpierce/wiki/index.php?n=Main.WikiSandbox#osx) on the Unison Wiki, as it provides a wealth of other information on what should or should not be included in your Unison profile. It also reminds you that you **must** edit your .PRF file in order to make Unison work.

Here's a sanitized version of the Unison profile that I'm using to keep files synchronized:

```
root = /Local/Path/To/Files
root = ssh://username@remote.host.com//Remote/Path/To/Files
servercmd = /usr/bin/unison
fastcheck = true
sortnewfirst = true
confirmbigdeletes = true
ignore = Name .FBCIndex
ignore = Name .FBCLockFolder
ignore = Name .Apple*
ignore = Name *.tmp
```

There are a few more ignore statements in there, but you get the idea. Refer to the Wiki page for a more detailed listing of suggested ignore statements. Oh, and the double forward slash in the second `root` statement is intentional, not a typo (this is the right syntax).

Keep in mind that this profile assumes that you've created a symlink (using `ln -s`) to the Unison executable inside the application bundle, as I described earlier. If you didn't create the symbolic link, the `servercmd` statement must have the full path all the way inside the application bundle.

Once you have your profile configured correctly, you can run the Unison GUI app, open the profile, and you should be off to the races. (In other words, it should work just fine.) In my case, I did an initial synchronization of around 6GB of data in a matter of minutes, and subsequent synchronization operations have been astonishingly fast.

If there are any questions, tips, clarifications, or corrections, please speak up in the comments. Thanks, and I hope you find this useful!

[1]: {{< relref "2011-11-18-some-oss-with-my-mac-part-1-synergy.md" >}}
