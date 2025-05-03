---
title: "Debian live howto"
date: 2009-06-06
tags:
  - linux
  - debian
  - live
categories:
  - debian
author_profile: true
draft: false
comments: false
---

To always have with you a Debian system on a CD or USB stick, you need to build a live CD. A live CD image ready to be used is available for download on the Debian web site. But what if you want to build a custom live CD with your own needed packages and no unnecessary stuff?

The Debian Live Build project (previously known as Live Helper) provides a way to build a live CD completely customized to your needs. The tool is easy to use, but comes equipped with a huge number of options, dramatically increasing flexibility but also complexity.\\
In this howto are covered tools and options I use to create my own images. The process described here should be appropriate for most common cases.

## Index

- [Index](#index)
- [1. What you need](#1-what-you-need)
- [2. The fast \& easy way](#2-the-fast--easy-way)
- [3. Adding some customization](#3-adding-some-customization)
  - [3.1 Packages lists](#31-packages-lists)
- [4. Burn the image](#4-burn-the-image)
- [5. Some examples](#5-some-examples)
- [6. Persistent live](#6-persistent-live)
- [7. More information](#7-more-information)

## 1. What you need

- A working Debian installation
- The `live-build` program - available in the main repo
- The `debootstrap` (or `cdebootstrap`) package - available in the main repo

## 2. The fast & easy way

The two commands below are the easiest and quickest way to create a non-customized image

```bash
lb config
lb build
```

The first command configures the build system while the second creates the image, producing the file `binary.iso` in the current directory. This is the system image ready to be burnt with all the basic packages required to run a plain live system without the X server. The image is in hybrid format, therefore it can be burnt on a CD as well as copied on a USB stick.

## 3. Adding some customization

At the config stage, the system can be customized. For example:

- **Distribution**: the distribution to use for the image can be chosen using `-d <codename>` (squeeze/wheezy/sid).
- **Installer**: the option `--debian-installer <installer>` allows to add the Debian installer to the image making the live CD installable on a hard drive. There are several versions of the Debian installer: `cdrom`, `netinst`, `netboot`, `businesscard`. The `live` installer will install on the disk an exact copy of your live CD.
- **Architecture**: By default the generated image adopts the architecture of the host system, but the option `--architectures <architecture>` allows to define a different target architecture. Crossbuilding is possible only if the host is capable to execute binaries of the target architecture natively.
- **Binary images**: the option `--binary-images <type>` allows to specify whether a `iso`, `iso-hybrid`, `net`, `tar`, or `usb-hdd` image should be built. Default is `iso-hybrid`, usable on both CD and USB sticks. `usb-hdd` is only for USB sticks while `iso` is only for CD.
- **Type of repository**: the `--archive-areas "area1, area2, ..."` option enables the `main`, `contrib` and `non-free` repositories. By default, only `main` is enabled.
- **Kernel flavour**: the option `--linux-flavours <flavour>` allows to select the flavour of the installed kernel (for example 386, 486, 686).
- **Hostname**: `--hostname <hostname>` sets the system hostname.
- **Memtest**: a memory tester can be installed in the system with `--memtest <tester>`. Options are `memtest86` and `memtest86+`. By default no tester is installed.
- **Username**: `--username <name>` allows to set in advance the pricipal user of the system.
- **Language**: the option `--language <language>` specifies which languages to install in the system, in the form of l10n packages.
- **Locale**: the system locale is set using the option `--bootappend-live "locales=<locale> keyboard-layouts=<layout>"` as in the following example

```bash
lb config -d squeeze --language en --bootappend-live "locales=en_GB.UTF-8 keyboard-layouts=en"
```

### 3.1 Packages lists

To define which packages to install on the system there are several pre-compiled lists. These default lists are available in the `/usr/share/live/build/lists/` folder.

```bash
ls /usr/share/live/build/lists/
debian-forensics        gnome         kde         lxde          studio        xfce-junior
debian-junior           gnome-core    kde-core    minimal       studio-gnome
debian-live-devel       gnome-full    kde-extra   rescue        studio-kde
debian-live-pxe-server  gnome-junior  kde-full    standard      studio-xfce
debian-science          gnustep       kde-junior  standard-x11  xfce
```

To select a list for installation use the `-p <list>` option indicating the name of the selected list.

To include personal packages not present in any of the default lists, it is possible to create a personal list in the folder `config/chroot_local-packagelists/`. Every line must contain the name of one package and the list file name MUST end with the `.list` suffix.

```bash
cd config/chroot_local-packagelists
touch my_list.list
```

Another option, is the `#include <list_name>` directive that allows to add packages to an existing default list. For example, to add The Gimp to the default gnome list use

```bash
# Personal list to include the Gimp package
#include <gnome>
gimp
```

## 4. Burn the image

How to burn the image depends on the chosen media. A CD can be burnt with the `wodim` command

```bash
wodim binary.iso
```

The `dd` command can be used to copy the image on a USB stick

```bash
dd if=binary.iso of=/dev/sdX
```

with `/dev/sdX` target device. Make sure to use the name of the device (`/dev/sdb`) NOT the name of a partition (`/dev/sdb1`). Be careful, as with this operation the content of the stick is overwritten and all existing data lost.

## 5. Some examples

**1.** Minimal stable system with X server

```bash
lb config -d squeeze -p standard-x11
lb build
```

**2.** Testing system, installable, with amd64 architecture and minimal Gnome

```bash
lb config -d wheezy -p gnome-core --architectures amd64 --debian-installer live
lb build
```

**3.** Installable system suitable for USB sticks (not CD) with Lxde

```bash
lb config -d squeeze -p lxde --binary-images usb-hdd --debian-installer live
lb build
```

**4.** Only main repository and kernel 686

```bash
lb config -d squeeze -p gnome-core --archive-areas main --linux-flavours 686
```

**5.** Testing with Memtest and both username and hostname set

```bash
lb config -d wheezy -p lxde --memtest memtest86+ --hostname my_live --username nero
```

**6.** main and contrib repositories plus memtest

```bash
lb config -d wheezy -p gnome-core --archive-areas "main contrib" --memtest memtest86
```

## 6. Persistent live

If the live CD system is installed on a writable media such as a USB stick larger than the image itself, data can be made persistent by writing files to a partition on the same USB stick.\
First, copy the image as described before. Then use a partitioning program to create a new partition on the empty space and initialize a filesystem. Make sure to add the `-L live-rw` option to set the filesystem partition label in order to make it usable by the live system.

```bash
dd if=binary.iso of=/dev/sdb
parted /dev/sdb
mkfs.ext4 -L live-rw /dev/sdb2
```

At boot, the kernel of the live system must have the boot parameter `persistent` enabled.

## 7. More information

This guide is far from being exhaustive about the available configurations for your live system. More information can be found on

- The official Debian Live-build manual page
- The man pages of the live-build package

```bash
man lb
man lb_config
man lb_build
```
