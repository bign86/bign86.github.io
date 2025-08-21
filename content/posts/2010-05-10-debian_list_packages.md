---
title: List packages in Debian
date: 2010-05-10
modified: 2011-09-09
tags:
   - linux
   - debian
   - package
   - system
categories:
   - debian
author_profile: true
draft: false
comments: false
slug: list-packages-debian
---

It is often useful to have a list of installed software on your Linux box. For example if you are installing a distribution on many machines, in this way you can customize packages only in one and replicate the same system everywhere. Or, if your box crashes in a unrecoverable way, you can set up a new one in few minutes.\
This howto works with `dpkg` present in Debian and derivated.

## Create the list of packages

With `dpkg` you can list all the packages currently  installed in your system because `dpkg` keep all infos about your packages in the `/var/lib/dpkg/` directory.

```bash
dpkg -l | grep ii
ii  cron                            3.0pl1-106             process scheduling daemon
ii  cups                            1.3.11-1+b1            Common UNIX Printing System(tm) - server
ii  cups-bsd                        1.3.11-1+b1            Common UNIX Printing System(tm) - BSD comman
ii  cups-client                     1.3.11-1+b1            Common UNIX Printing System(tm) - client pro
ii  cups-common                     1.3.11-1               Common UNIX Printing System(tm) - common fil
ii  cups-driver-gutenprint          5.2.4-1                printer drivers for CUPS
ii  cvs                             1:1.12.13-12           Concurrent Versions System
ii  dash                            0.5.5.1-2.3            POSIX-compliant shell
ii  dbus                            1.2.16-2               simple interprocess messaging system
ii  dbus-x11                        1.2.16-2               simple interprocess messaging system (X11 de
ii  debconf                         1.5.27                 Debian configuration management system
ii  debconf-i18n                    1.5.27                 full internationalization support for debcon
ii  debhelper                       7.3.12                 helper programs for debian/rules
ii  debian-archive-keyring          2009.01.31             GnuPG archive keys of the Debian archive
ii  debianutils                     3.2.1                  Miscellaneous utilities specific to Debian
ii  deborphan                       1.7.28                 program that can find unused packages, e.g. 
ii  defoma                          0.11.10-1              Debian Font Manager -- automatic font config
ii  deskbar-applet                  2.26.2-1               universal search and navigation bar for GNOM
...
```

The list is very long. You can redirect this list to a fie to create a simple selection.

```bash
dpkg -l | grep ii | awk '{print $2} > installed
```

But `dpkg` has a specific tool for this purpose. It's very simple to use and create a simple list of installed packages with only one command

```bash
dpkg --get-selections > installed
```

Is convenient to create the list with only the names of packages, one for each line of the file. This is the simplest format and you can be sure that will be read easily when you will install the packages in this list.\
Enter

```bash
dpkg --get-selections | grep install | awk '{print $1}' > installed
```

## Restore of the packages

Now we want to replicate the original system using ours list of packages. All the programs present on every Debian system can do that after importing the selection with `dpkg`.

* With `dselect`

   ```bash
   dpkg --set-selections < installed
   dselect install
   ```

* With `aptitude`

   ```bash
   aptitude install < installed
   ```

* With `apt`

   ```bash
   dpkg --set-selections < installed
   apt-get dselect-upgrade
   ```

If we have created the simplest list with only the file name we can use the shell and `xargs` to pass to `apt` the packages to install

```bash
cat installed | xargs apt-get install
```

All these commands will do the work but the one I prefer is the third using `dpkg` and `apt`.
