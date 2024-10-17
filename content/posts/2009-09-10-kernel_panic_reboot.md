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

A kernel panic is a very bad and (fortunately) very rare event in Linux because it is a very stable system. A kernel panic means that something irreversible is happened and the kernel doesn't know how to fix and/or handle the problem. It is the parallel in Linux of the BSoD in Windows. The only possible exit from this bad situation is a reboot.

## Sysctl panic key

The `sysctl` system has a way to automate the reboot event. With root privileges, open the `/etc/sysctl.conf`  file and add the following line

```bash
kernel.panic = 10
```

In case of a kernel panic the system will reboot in 10 seconds. The default behavior is to not reboot. Now read the new setting with

```bash
# sysctl -p
```

To see if the option has been read by the system, use the `sysctl` command

```bash
# sysctl -a | grep kernel.panic
```

If you prefer you can use a different approach (with same result)

```bash
# echo "10" > /proc/sys/kernel/panic
```

When the system reboot you can use the log files to understand what is happened at take the appropriate steps to correct the problem.

## SysRQ

The magic SysRQ system is a key combination useful to perform various operations like halt or reboot the system safely. This is particularly useful when the system locks, the X server crashes or the keyboard is not working.\\
To enable the SysRQ system open the `/etc/sysctl.conf` file with root privileges and append the following line

```bash
kernel.sysrq = 1
```

Now use

```bash
# sysctl -p
```

The key combination is `[Alt + SysRQ + KEY]`. Using a graphical environment this combination can be not working as expected or (especially on a laptop) the SysRQ key can be not present. In those cases the `[SysRQ = Print Screen]` key and the key combination is `[Ctrl + Alt + SysRQ + KEY]` or `[Alt Gr + SysRQ + KEY]`.
If you are on a laptop probably you have to use also the `[Fn]` key. The `[KEY]` key change with your aim. Options are

| Option | Meaning                                       |
|:------:| --------------------------------------------- |
| e      | Send a `SIGTERM` to all processes except init |
| b      | Reboot without syncing the disks              |
| s      | Attempt to sync all filesystems               |
| o      | Shutdown                                      |
| u      | Re-mount all filesystems in read-only         |

On the net: You can do the same thing on a remote server using the ipt_sysrq extension for Iptables that allows you to send a SysRQ call over the network. In this way you can sync all filesystems and reboot the server without have to go there personally, comfortably from your desk.

_Note_: The SysRQ system has to be compiled inside the kernel to be able to use it enabling the SysRQ system. At kernel compile time you need to enable the key `CONFIG_MAGIC_SYSRQ`.

## With GRUB

You can achieve the goal also with GRUB. You have to pass a parameter to the boot process like in this example

```bash
linux   /vmlinuz-3.0.0-1-amd64 root=UUID=6fa03cf0-8607-4ede-85b3-a3fb3b64c2ec ro nomodeset=1 quiet panic=10
```

The `panic=10` instruction tells the kernel to reboot after 10 seconds like we did before.

## With LILO

There is also the same for LILO for people still using it. Search for the `/etc/lilo.conf` file and add the following at the end

```bash
panic = 10
```

_Final thought_:\\
A reboot often is not a solution. You haven't FIXED the problem. While it is possible that the problem is in the software, Linux is very stable and the cause of a kernel panic is often bad hardware. A reboot does not fix the hardware, of course. Always check the log files to know what has happened in your system.
