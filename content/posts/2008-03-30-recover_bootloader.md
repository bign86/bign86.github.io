---
title: "How to recover the bootloader"
date: 2008-03-30
modified: 2009-07-29
tags:
  - linux
  - bootloader
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

There are several way to destroy the MBR installation of your bootloader. The most common is probably a fresh Windows installation. Microsoft has this bad habit of wiping away any existing bootloader already present on your machine. Once the bootloader is gone Linux is unreachable (not exactly but we want our GRUB back!) How to fix the problem?

What you need is a Linux distribution bootable from CD/DVD/USB, the knowledge of your partition table or alternatively how to use a tool like `fdisk`, `df` or `cfdisk`. Personally I use RIPLinux, small and reliable.

## 1. Prepare to recover

Boot your live system and get a terminal. If you don't know exactly your partition table use the tool you prefer to get it. For example

```bash
# fdisk -l
```

Now we need to mount some folders from the old system.\\
The first is the root (`/`) partition of the system we want to recover. Let's suppose it's in the `/dev/hdb3` partition (change the device accordingly at your partition table). We mount it in `/mnt/`.

```bash
# mount -t auto -w /dev/hdb3 /mnt/
```

To rewrite the MBR we need to `chroot` in the mounted system. The `dev/` and `proc/` folders are fundamental to have a running system and therefore we mount them too. Mount them `/mnt/` folder

```bash
# mount -o bind /dev /mnt/dev
# mount -t proc /proc /mnt/proc
```

We need also the `boot/` partition. This is not a problem if we have the `boot/` folder on the same physical partition of `/` since at this stage is already mounted. If this is not the case you'll need to mount it separately using

```bash
# mount -t auto /dev/hdb2 /mnt/boot
```

Chroot in the `/mnt/` folder.

```bash
# chroot /mnt/
```

The root (`/`) now is the one of the system to recover. We are in! The only thing to do is recover the bootloader.

## 2. Recover the bootloader

### Grub2

Grub2 is very close to Grub in the recovery process. When you are in the chroot environment update Grub2 and install it in the MBR with

```bash
# update-grub
# grub-install /dev/hdb
```

(supposing `/dev/hdb` is the right disk)\\
Always check if everything is ok

```bash
# grub-install --recheck /dev/hdb
```

Now you can exit from the chroot and reboot the system.\\
Grub2 should always be able to detect correctly a Windows installation. If for some reason Windows has not been detected, use `os-prober` to search for it and then follow the previous steps.

```bash
# os-prober
# update-grub
```

### Grub

Now deprecated in favor of Grub2, but maybe someone is still using it. To reinstall

```bash
# grub-install /dev/hdb
```

If you want more control on operations start the Grub terminal with

```bash
# grub
```

In the terminal type

```bash
> root (hd1,0)
> setup (hd1)
> exit
```

where as usual you need to enter the correct partitions. The root `(hdX,Y)` partition is the bootable one, where the `boot/` folder resides.

To check for Windows without `os-prober` open the configuration file `menu.lst`:

```bash
# nano /mnt/root/boot/grub/menu.lst
```

The Windows entry is like this one (This is a Windows XP entry, don't know about newer Windows versions)

```bash
title MS win**********
root (hd0,0)
savedefault
makeactive
chainloader +1
```

Done! Reboot the system.

### LILO

Recover the old Lilo is very simple. You need only to run

```bash
# lilo
# lilo -q
```

The first command reinstalls Lilo while the second checks for errors.
