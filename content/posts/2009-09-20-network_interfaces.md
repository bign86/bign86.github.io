---
title: "Change name to network interfaces"
date: 2009-09-20
modified: 2011-09-07
tags:
  - linux
  - shell
  - base_command
  - networking
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

Every Unix system assign a default name to each network device and a progressive number to distinguish between devices of the same kind. We can change this default name with one we choose. To do this we have to modify the configuration files of Udev, the system responsible for the hardware. For Udev to be able to work on a device, drivers needed for that device must be present inside the system so we will suppose network devices to be working.

Udev rules concerning network devices are in the `/etc/udev/rules.d/70-persistent-net.rules` file. The file syntax is simple

```bash
# PCI device 0x10b7:0x9201 (3c59x)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:26:54:14:ec:46", ATTR{dev_id}=="0x0",\
           ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"
```

On each line we find the rule for each network device. The MAC address and the name are indicated respectively in the `ATTR{address}` and `NAME` fields.\\
To find the device we are interested in, we can use the `ifconfig` command

```bash
# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:26:54:14:ec:46 
          inet addr:192.168.0.3  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe80::226:54ff:fe14:ec46/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:53839 errors:0 dropped:0 overruns:0 frame:0
          TX packets:36178 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:71524572 (68.2 MiB)  TX bytes:3622015 (3.4 MiB)
          Interrupt:21 Base address:0x6000
```

The MAC address is specified in the `Waddr` field (hardware address).

Now we can bypass the rules inside the `/etc/udev/rules.d/70-persistent-net.rules` file, creating another file inside the `/etc/udev/rules.d/` directory. The new file name must follow the syntax

`<NN>-<new_rules>.rules`

like other files. The number `<NN>` must be lower than the one of the other file. The number is the rule order of priority. Lower numbers mean higher priority. In this new file we will add a new line (only one line) for each device we are interested in to rename. For example

```bash
# touch 40-newrules-net.rules
# echo KERNEL=="old_name_prefix*", SYSFS{address}=="MACaddr", NAME="new_name" > /etc/udev/rules.d/40-newrules-net.rules
```

where `old_name_prefix` is the old name of the device without the number (for example `eth`, `wlan`), `ACaddr` is the MAC address, `new_name` is the name we want to give to it (must be lowercase!). For example you can enter

```bash
KERNEL=="eth*", SYSFS{address}=="00:26:54:14:ec:46", NAME="cable1"
```

Now you need to update the network configuration to use new names. For each device changed substitute the new name in the `/etc/network/interfaces` file.

To verify that all went good, reload the networking with (on Debian and *buntu)

```bash
# /etc/init.d/networking restart
# ifconfig new_name
```

A similar command is available for any distribution.

Modify the name of a network devices is a very simple task but it is not always safe. Many programs assume standard names for interfaces and can have unpredictable behavior with different names. For this reason is always better to adhere to standard names like `ethN`, `wlanN` etc.
