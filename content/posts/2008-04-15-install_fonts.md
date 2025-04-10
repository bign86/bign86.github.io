---
title: "Install fonts on Linux"
date: 2008-04-15
modified: 2012-06-01
tags:
  - linux
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

On the internet there are zillions of different fonts, of any kind. They are almost always packaged in some archive with different extensions. But how to make this fonts available to our user or maybe to the whole system?

Where to put our fonts? There are two choices. To make the fonts available to all users, use a system folder like `/usr/share/fonts` or `/usr/local/share/fonts`. To make the fonts available only to your user place them in the `/home/user/.fonts` in your home. If the folder is missing, create it. The commands below are the same regardless of the fonts location.

## Move your fonts in the preferred folder

```bash
mv *.ttf ~/.fonts                      # < with user permissions
mv *.ttf /usr/local/share/fonts        # < with root permissions
```

If you want you can change permissions

```bash
chmod 644 ~/.fonts/*.ttf
```

Update the fonts database with

```bash
cd ~/.fonts
mkfontscale && mkfontdir
fc-cache
```

* `mkfontscale`: creates a list of scalable fonts for X. Has many options that are beyond the scope of this post.
* `mkfontdir`: creates a index of the fonts in the directory.
* `fc-cache`: builds the fonts cache.

## Adding new directories to the fonts path

If the fonts are _not_ lcoated in one of the standard folders above, the path needs to be added to the system font path. In many distributions (like Red Hat) the command `chkfontpath` will do it

```bash
chkfontpath --add /folder/you/have/created
```

In distributions where `chkfontpath` is not available (Debian and *buntu) then we must modify the `fontconfig` package options. The system-wide xml-based `fonts.conf` configuration file is found in

```bash
cd /etc/fonts
less fonts.conf
```

Inside, there should be a list of font directories similar to the following

```xml
<!-- Font directory list -->
    <dir>/usr/share/fonts</dir>
    <dir>/usr/X11R6/lib/X11/fonts</dir> <dir>/usr/local/share/fonts</dir>
    <dir>~/.fonts</dir>
```

While a new directory can be added here, it should not be done as suggested in the file itself

```xml
<!--
    DO NOT EDIT THIS FILE.
    IT WILL BE REPLACED WHEN FONTCONFIG IS UPDATED.
    LOCAL CHANGES BELONG IN 'local.conf'.
```

Instead, create a file `local.conf` in the same directory and add the following lines

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <!-- Local system customization file -->
    <dir>/folder/you/have/created</dir>
</fontconfig>
```

## Reload the xfs server

If you have a `xfs` server running on your machine, restart it to read the new fonts

```bash
/etc/init.d/xfs restart
```

## Useful link

[The Debian fonts page](http://wiki.debian.org/Fonts)\
[The Arch Linux fonts page](https://wiki.archlinux.org/index.php/Font_Configuration)\
[The fontconfig manual at Freedesktop.org](http://www.freedesktop.org/software/fontconfig/fontconfig-user.html)
