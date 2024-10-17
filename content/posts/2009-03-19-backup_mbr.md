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

Many people like to do experiments with their own computers or like to try many differnt OS. Playing with your MBR can make it unusable. In this situation a backup is what we need.

The MBR is consist in the first 512 byte of our hard disk. Is formed by 3 parts:

- the code for booting (446 bytes)
- the partitioning table (64 bytes)
- the boot code signature (2 bytes)

So the first 512 bytes are what we want to save. The `dd` command is perfect for this purpose

```bash
# dd if=/dev/hda of=/home/user/mbr.bak bs=512 count=1
```

This command requires root privileges.

To restore the MBR we use the same command reversed

```bash
# dd if=/backup/path/mbr.bak of=/dev/hda bs=512 count=1
```

This operation can be done using only the `dd` command, present in all live distributions, so you can restore your MBR using a CD or a USB pen drive.

The `dd` command is useful also if you want to destroy your MBR, covering it with zeros (first command) or random data (second command)

```bash
# dd if=/dev/zero of=/dev/hda bs=512 count=1
# dd if=/dev/urandom of=/dev/hda bs=512 count=1
```

_WARNING_: this is generally not advisable because we are destroying the partitioning table. It's better to preserve the table deleting only the first 446 bytes containing the boot code

```bash
# dd if=/dev/zero of=/dev/hda bs=446 count=1
```

Then we need to restore a backup or reinstall the MBR. To reinstall there are specific commands like these.
