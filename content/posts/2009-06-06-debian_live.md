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

If you want to bring always with you an installation of Debian on a CD or USB stick, you need to build a live CD. A live CD is available for download on the Debian web site ready to be used. But what if you want to build a custom live CD with your own needed packages and no unuseful stuff?

The Debian Live Build project provide a way to build a live CD completely customized to your needs. Using this utility to build a simple image is easy, but the huge amount of options increase dramatically both flexibility and complexity.\\
In this how to I will cover tools and options I use to create my own image. The process should be well suited for all common needs, you have only to modify commands with the packages you want to include.

The Live Build project in the past was called Live Helper! The last version of Live Helper is live-helper 2.0.12-2.

## Index

- [Index](#index)
- [1. What you need](#1-what-you-need)
- [2. The fast \& easy way](#2-the-fast--easy-way)
- [3. Adding some customization](#3-adding-some-customization)
  - [3.1 Some examples](#31-some-examples)
- [4. Burn the image](#4-burn-the-image)
- [5. More customizations](#5-more-customizations)
  - [5.1 Some examples](#51-some-examples)
- [6. Include your packages](#6-include-your-packages)
- [7. Language and locale](#7-language-and-locale)
- [8. Persistent live](#8-persistent-live)
- [9. More information](#9-more-information)

## 1. What you need

* A working Debian installation
* The live-build program - you can find it in the main repos
* The debootstrap (or cdebootstrap) package - in the main repos again

## 2. The fast & easy way

To have quickly an image to burn enter

```bash
$ lb config
$ lb build
```

The first command configure the build system while the second creates the image. At the end we will find a file `binary.iso` in the current directory. This is our image ready to be burnt. In this image there are all the basic packages to run a plain live system but no X.org. The image is in the hybrid format so can be burnt on a CD as well copied on a USB stick.\\
In this way the flexibility of the method is almost zero.

## 3. Adding some customization

Usually you want to add some customization during the config stage.

* **Distribution**: you can choose the distribution using `-d <codename>` (squeeze/wheezy/sid).
* **Lists**: with the `-p <list>` you can chose one of the list of packages to characterize your image. The lists are all available in

```bash
$ ls /usr/share/live/build/lists/
debian-forensics        gnome         kde         lxde          studio        xfce-junior
debian-junior           gnome-core    kde-core    minimal       studio-gnome
debian-live-devel       gnome-full    kde-extra   rescue        studio-kde
debian-live-pxe-server  gnome-junior  kde-full    standard      studio-xfce
debian-science          gnustep       kde-junior  standard-x11  xfce
```

If no list is given the standard list is used (minimal system, no X.org).

* **Installer**: with `--debian-installer <installer>` you can introduce the Debian installer inside the image to install Debian on a hard disk. There are different versions of the Debian installer to choose from. cdrom, netinst, netboot, businesscard are the same shipped with the official installed media. live will install exactly your live CD on the disk.
* **Architecture**: with `--architectures <architecture>` you define the architecture of the image that by default is the host architecture. You can crossbuild only if the host is capable to execute binaries of the target architecture natively.
* **Binary images**: using `--binary-images <type>` you tell the builder the type of image you want to build between iso, iso-hybrid, net, tar, usb-hdd. Default is iso-hybrid usable on both CD and USB sticks. usb-hdd is only for USB sticks while iso is only for CD.

After this you can send the build command as before.

### 3.1 Some examples

**1.** Minimal stable system with X.org

```bash
$ lb config -d squeeze -p standard-x11
$ lb build
```

**2.** Testing system, amd64 architecture with minimal Gnome and installable

```bash
$ lb config -d wheezy -p gnome-core --architectures amd64 --debian-installer live
$ lb build
```

**3.** System with Lxde suitable for USB sticks, not for CD, and installable

```bash
$ lb config -d squeeze -p lxde --binary-images usb-hdd --debian-installer live
$ lb build
```

## 4. Burn the image

The burning system is media dependent. A CD suitable image can be burnt with the `wodim` command

```bash
$ wodim binary.iso
```

On a USB stick (check there is enough space available on the stick to hold the entire image) the `dd` command will do the job

```bash
$ dd if=binary.iso of=/dev/sdb
```

Replace `/dev/sdb` with the correct device for you. Use must use the name of the device (`/dev/sdb`) NOT the name of the partition in it (`/dev/sdb1`). Remember that with this operation the content of the stick will be overwritten and all previous data will be lost##

## 5. More customizations

The Live-build program has a lot of options to choose from. Describe them all is beyond the scope of this howto. I explain here some more options I think some people can find useful. For a complete manual go to the reference section.

* **Type of repository**: the `--archive-areas "area1, area2, ..."` option enable main, contrib and non-free repos. The default value is main so only packages from the main branch can enter in the live system. To enable other branches add their names here.
* **Kernel flavour**: with `--linux-flavours <flavour>` you tell the build system what kernel do you want to use in the live system (for example 384, 486, 686).
* **Hostname**: set the hostname of the live system with `--hostname <hostname>`.
* **Memtest**: with `--memtest <tester>` you define what memory tester introduce in the live system. Possibilities are memtest86 and memtest86+. The default value is none, so no tester will be introduced.
* **Username**: what name the principal user of the system will have? You can set it with `--username <name>`.

### 5.1 Some examples

**1.** Only main repository and kernel 686

```bash
$ lb config -d squeeze -p gnome-core --archive-areas main --linux-flavours 686
```

**2.** Testing with Memtest and both username and hostname set

```bash
$ lb config -d wheezy -p lxde --memtest memtest86+ --hostname my_live --username nero
```

**3.** Main and contrib repositories plus memtest

```bash
$ lb config -d wheezy -p gnome-core --archive-areas "main contrib" --memtest memtest86
```

## 6. Include your packages

To include personal packages not present in any of the default lists in `/usr/share/live/build/lists/` you can create a personal list inside the folder `config/chroot_local-packagelists/`. Every line must contain the name of one package and the list file name MUST end with the `.list` suffix.

```bash
$ cd config/chroot_local-packagelists
$ touch my_list.list
```

You can also use the `#include <list_name>` directive to add some package to an existing default list. This is the common behavior of the default lists. For example to add The Gimp to the default gnome list

```bash
# Personal list to include the Gimp package
#include <gnome>
gimp
```

## 7. Language and locale

The booting process of the live system set the locale in terms of language used and keyboard layout. The locale files must be installed in the live image and you have to tell the image which locale use at the boot. To install the correct l10n packages use the `--language <language>` option, while to set the booting locale you need the `--bootappend-live "parameter"` option in this way

```bash
$ lb config -d squeeze --language en --bootappend-live "locales=en_GB.UTF-8 keyboard-layouts=en"
```

## 8. Persistent live

If you are using a rewritable media such a USB stick, you can transform the live system into a persistent one, where modifications are kept in a writable partition on the same USB stick. To enable this feature you need a USB stick larger than the system image, so that after copying the image some space is empty.

First copy the image as described before, then use a partition program to create a new partition. Last create a new filesystem on the partition. Be sure to add the `-L live-rw` option to set the partition label in order to make the partition usable by the live system.

```bash
$ dd if=binary.iso of=/dev/sdb
$ parted /dev/sdb
$ mkfs.ext4 -L live-rw /dev/sdb2
```

The kernel of the live system must be start at the boot with the boot parameter persistent enabled.

## 9. More information

This guide is far away from exhaustive about all configurations possible building your live system. More information can be found

* The official Debian Live-build manual page
* On the man pages of the package

```bash
$ man lb
$ man lb_config
$ man lb_build
```
