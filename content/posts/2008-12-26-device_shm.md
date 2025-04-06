---
title: "The shared memory device SHM"
date: 2008-12-26
tags:
  - linux
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

In the `/dev` directory there is a special device called `/shm`. What is it?\
This particular device was first introduced in the 2.4.x branch of the kernel and implements the concept of shared memory (SHared Memory). It looks like any other device but in practice it is a folder that uses the `tmpfs` filesystem. Any file created inside this device exists only in the shared memory or in the RAM, while nothing is written to the disk. For this reason, when the machine is restarted, the device is automatically emptied. With this system a program can fill a portion of memory that another process can access directly, if allowed. The result is a significant increase in performance.

In practice, moving a file in the device

```bash
cp -f ~/file /dev/shm
```

the file has been just moved to RAM.\
By default, the size of this filesystem is set to half of the RAM available to the system, but it can be changed. To do this, remount the `shm` device as you would for any other device by using the `mount` command

```bash
mount -o remount,size=2G /dev/shm
```

This device was introduced for compatibility with POSIX shared memory and it is not advisable to use it excessively, instead, it is advisable to use the `tmpfs` filesystem. A use exmple could be to move files saved on slow devices (CDs for example) to RAM to perform read-only operations. To create a persistent directory within the `tmpfs` filesystem that is automatically mounted at each machine startup, we can use the `mount` command again

```bash
mkdir -p ~/cache
mount -t tmpfs -o size=1G, mode=700 tmpfs ~/cache
```

Another exmpale of use is the creation of a cache on a server that hosts a website. The increase in performance can make a huge difference even on virtual machines like VMware.\
For example, to create the cache for a site we can use the following commands

```bash
cd /var/www/sito/
mkdir -p ./tmp
mount -t tmpfs -o size=5G,nr_inodes=5k,mode=744 tmpfs /var/www/sito/tmp
```

If we have a machine with more than 2G of RAM running several virtual machines, this often makes a huge difference in terms of performance. To make these changes available at every reboot of the physical machine, we can add them in `/etc/fstab` by adding this line

```bash
tmpfs /var/www/sito/tmp tmpfs size=5G,nr_inodes=5k,mode=744 0 0
```
