---
title: "How to backup a MBR"
date: 2009-03-19
modified: 2011-08-19
tags:
  - linux
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

Many like to do experiments with their own computers or try man different operating systems. However, playing around with the MBR can render it unusable. To avoid problems, better to do a backup.

The MBR consists of the first 512 bytes of the hard disk. Iy has 3 parts:

- the code for booting (446 bytes)
- the partition table (64 bytes)
- the boot code signature (2 bytes)

The `dd` command is perfect to do a backup of the MBR

```bash
dd if=/dev/hda of=/home/user/mbr.bak bs=512 count=1
```

This command requires root privileges.

To restore the MBR we use the same command reversed

```bash
dd if=/backup/path/mbr.bak of=/dev/hda bs=512 count=1
```
