---
title: "Reboot from kernel panic"
date: 2009-09-10
modified: 2010-06-23
tags:
  - linux
  - debian
  - system
categories:
  - debian
author_profile: true
draft: false
comments: false
---

A kernel panic is a kernel shutdown caused by an irreversible error the kernel doesn't know how to fix or handle. It is the Linux counterpart of the BSoD in Windows. It happens rarely on Linux, as it is a very stable system. The only possible exit from this bad situation is a reboot.

## Sysctl panic key

The default behavior in case of a kernel panic is to wait for human intervention. The `sysctl` system, however, allows the kernel to automatically reboot itself if properly configured. With root privileges, open the `/etc/sysctl.conf`  file and add the following line

```bash
kernel.panic = 10
```

With this configuration the system reboots after a kernel panic in 10 seconds. Update the settings with

```bash
sysctl -p
```

To see if the option has been read by the system, use the `sysctl` command

```bash
sysctl -a | grep kernel.panic
```

The following approach can also be used

```bash
echo "10" > /proc/sys/kernel/panic
```

When the system reboots, the log files can help understanding what has happened to the system, and take the appropriate steps to correct the problem.

## SysRQ

The SysRQ system is a combination of keys used to halt or reboot the system safely. This comes particularly handy when the system locks or the X server crashes. For more details, see [this post about SysRQ](2008-09-19-sysrq.md).\
To enable the SysRQ system open the `/etc/sysctl.conf` file with root privileges and append the following line

```bash
kernel.sysrq = 1
```

Again, update the settings of `sysctl`

```bash
sysctl -p
```

The key combination is `[Alt + SysRQ + <KEY>]` where `[KEY]` may be one of the keys listed in the table below

| Option | Meaning                                       |
|:------:| --------------------------------------------- |
| e      | Send a `SIGTERM` to all processes except init |
| b      | Reboot without syncing the disks              |
| s      | Attempt to sync all filesystems               |
| o      | Shutdown                                      |
| u      | Re-mount all filesystems in read-only         |

Note that in some graphical environments or on specific platforms (like some laptops) the combination of keys might be different. For example, the following may be the combinations to use: `[Ctrl + Alt + SysRQ + KEY]` or `[Alt Gr + SysRQ + KEY]`. If you are on a laptop, it may be that the `[Fn]` key must be used in addition to the others.

Over the network: You can do the same thing on a remote server using the `ipt_sysrq` extension for Iptables that allows you to send a SysRQ call over the network. In this way you can sync all filesystems and reboot the server remotely.

_Note_: The SysRQ system has to be compiled inside the kernel to be able to use it enabling the SysRQ system. At kernel compile time you need to enable the key `CONFIG_MAGIC_SYSRQ`.

## With GRUB

You can achieve the same goal also with GRUB. You must pass a parameter to the boot process like in this example

```bash
linux   /vmlinuz-3.0.0-1-amd64 root=UUID=6fa03cf0-8607-4ede-85b3-a3fb3b64c2ec ro nomodeset=1 quiet panic=10
```

The `panic=10` instruction tells the kernel to reboot after 10 seconds like we did before.

## With LILO

There is also the same for LILO for people still using it. Search for the `/etc/lilo.conf` file and add the following at the end

```bash
panic = 10
```
