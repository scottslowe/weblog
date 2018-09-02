---
author: slowe
categories: Explanation
comments: true
date: 2018-09-01T19:00:00Z
tags:
- Fedora
- Linux
- Productivity
title: Better XMind-GNOME Integration
url: /2018/09/01/better-xmind-gnome-integration/
---

In December of 2017 I wrote about [how to install XMind 8 on Fedora 27][xref-1], and at the time of that writing I hadn't quite figured out how to define a MIME type for XMind files that would allow users to double-click on an XMind file in Nautilus and open that file in XMind. After doing a bit of additional research and testing, I've found a solution and would like to share it here.<!--more-->

The solution I'll describe here has been tested on Fedora 28, but it should work on just about any distribution with the GNOME desktop environment.

First, you'll want to define the MIME type by creating an XML file in the `~/.local/share/mime/packages` directory, as outlined [here][link-1]. I called my file `application-vnd-xmind-workbook.xml`, but I don't know if the filename actually matters. (I derived the filename from [this list of XMind file types][link-2].) The contents of the file should look something like this:

``` xml
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
  <mime-type type="application/vnd.xmind.workbook">
    <comment>XMind Workbook</comment>
    <glob pattern="*.xmind"/>
    <glob pattern="*.XMIND"/>
    <glob pattern="*.XMind"/>
  </mime-type>
</mime-info>
```

You'll note that multiple glob patterns are included to help deal with case sensitivity issues. The specific values used in the `mime-type` element is again taken from [the XMind wiki][link-2].

Next, you'll want to adjust the desktop launcher for XMind. Add the `MimeType=application/vnd.xmind.workbook` line to the desktop launcher file. I'm using a desktop launcher that is user-specific, and found in the `~/.local/share/applications` directory. It should look something like this when you're done:

``` text
[Desktop Entry]
Comment=Create and share mind maps
Exec=/opt/xmind/XMind_amd64/XMind %F
Path=/opt/xmind/XMind_amd64
Name=XMind
Terminal=false
Type=Application
Categories=Office;Productivity
Icon=/home/username/.local/share/icons/xmind.png
MimeType=application/vnd.xmind.workbook
```

You'll note that this example points to an icon stored in a user-specific directory; be sure to adjust this as needed to point to the location of the icon you want to use for XMind.

It also appears that the `Path=` statement in the desktop file is important; it specifies the working directory to run the program in (see [this reference][link-3]). I saw at least one reference (here's [one example][link-4]) of needing to switch to the correct directory in order to get XMind to open files specified on the command line.

Finally, update the MIME and application databases, respectively:

    update-mime-database ~/.local/share/mime/packages
    update-desktop-database ~/.local/share/applications

You can run the `gio` commands outlined [here][link-1] (look toward the bottom of the page) to test to make sure that the MIME type and application are correctly defined and linked, if you like.

For a custom icon (otherwise GNOME will use the generic text document icon for XMind files) the process is a bit trickier. I use the [Numix icon theme][link-5], so I ran the following commands:

    cd /usr/share/icons/Numix/48/mimetypes
    sudo ln -s inode-symlink.svg application-vnd.xmind.workbook.svg
    sudo gtk-update-icon-cache /usr/share/icons/Numix

After that, the icons for XMind files immediately changed. For readers using a different theme, you'll need to substitute the correct paths and the correct filenames; the trick, of course, is to end up with a filename that matches the new MIME type defined earlier.

With these changes in place, you should now be able to double-click an XMind mind map and have it launch XMind (if not already running) and open the selected file (as one would expect). Enjoy!

[link-1]: https://help.gnome.org/admin/system-admin-guide/stable/mime-types-custom-user.html.en
[link-2]: https://github.com/xmindltd/xmind/wiki/FileTypes
[link-3]: https://developer.gnome.org/desktop-entry-spec/
[link-4]: https://github.com/xmindltd/xmind/issues/200
[link-5]: https://github.com/numixproject/numix-icon-theme
[xref-1]: {{< relref "2017-12-15-installing-xmind-8-on-fedora-27.md" >}}
