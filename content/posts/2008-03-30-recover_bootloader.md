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

There are several way to destroy the MBR installation of your bootloader. A common occurrence wih dual-boot system is that a fresh Windows installation wipes away the existing bootloader. Once the bootloader is gone Linux becomes (almost) unreachable. How to fix the problem?

To proceed are needed a Linux distribution bootable from CD/DVD/USB and how to use a tool like `fdisk`, `df` or `cfdisk`. RIPLinux, small and reliable, is perfect for the job.

## 1. Preparations

Boot the live system and get to a terminal. Extract the partition table using one of the many tools. For example

```bash
fdisk -l
```

Now, we need to mount some folders from the system we want to recover.\
The first is the root partition (`/`), that we mount in  `/mnt/`. Next, the `dev/` and `proc/` folders are fundamental to have a running system and must be mounted as well.

```bash
mount -t auto -w /dev/hdb2 /mnt/
mount -o bind /dev /mnt/dev
mount -t proc /proc /mnt/proc
```

Here it is assumed the root in in `hdb2`, change according to your system. The `boot/` folder must also be mounted. If `boot/` resides in a different partition then root, mount it as well. For example, if separated in `hdb1`

```bash
mount -t auto /dev/hdb1 /mnt/boot
```

To rewrite the MBR we need to `chroot` in the system now mounted in `/mnt/`

```bash
chroot /mnt/
```

The root (`/`) is now the one of the system to recover. We are in! We can now start to recover the bootloader.

## 2. Recover the bootloader

### Grub2

Grub2 is very close to Grub in the recovery process. When you are in the chroot environment update Grub2 and install it in the MBR with

```bash
update-grub
grub-install /dev/hdb
```

(supposing `/dev/hdb` is the right disk)\
Always check if everything is ok

```bash
grub-install --recheck /dev/hdb
```

Now you can exit from the chroot and reboot the system.\
Grub2 should always be able to detect correctly a Windows installation. If for some reason Windows has not been detected, use `os-prober` to search for it and then follow the previous steps.

```bash
os-prober
update-grub
```

### Grub

Now deprecated in favor of Grub2, but maybe someone is still using it. To reinstall

```bash
grub-install /dev/hdb
```

If you want more control on operations start the Grub terminal with

```bash
grub
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
nano /mnt/root/boot/grub/menu.lst
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
lilo
lilo -q
```

The first command reinstalls Lilo while the second checks for errors.
