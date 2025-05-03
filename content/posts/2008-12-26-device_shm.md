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

In the `/dev` directory there is a special device called `/shm`. What is it?\\
This particular device was first introduced in the 2.4.x branch of the kernel and implements the concept of SHared Memory. It looks like any other device but in practice it is a folder using the `tmpfs` filesystem. A file created inside this device exists only on the shared memory, while nothing is written to the disk. For this reason, when the machine is shutoff or restarted, the device is automatically emptied. With this system a program can fill a portion of memory that another process can access directly, if allowed. The result is a significant increase in performance.

In practice, moving a file in the device

```bash
cp -f ~/file /dev/shm
```

the file has been just moved to RAM.\
By default, the size of this filesystem is set to half of the RAM available to the system, but it can be changed. To do this, remount the `shm` device as you would for any other device by using the `mount` command

```bash
mount -o remount,size=2G /dev/shm
```

A use example could be to move files saved on slow devices (CDs for example) to RAM to perform read-only operations. To create a persistent directory within the `tmpfs` filesystem that is automatically mounted at each machine startup, use the `mount` command as

```bash
mkdir -p ~/cache
mount -t tmpfs -o size=1G, mode=700 tmpfs ~/cache
```

Another usage example is the creation of a cache on a server hosting a website. The increase in performance can make a huge difference even on virtual machines like VMware.\\
To create the cache for a website use the following commands

```bash
cd /var/www/website/
mkdir -p ./tmp
mount -t tmpfs -o size=5G,nr_inodes=5k,mode=744 tmpfs /var/www/website/tmp
```

On a machine with more than 2G of RAM running several virtual machines this often makes a huge difference in terms of performance. To make these changes available at every reboot of the physical machine, we can add them in `/etc/fstab` by adding this line

```bash
tmpfs /var/www/website/tmp tmpfs size=5G,nr_inodes=5k,mode=744 0 0
```
