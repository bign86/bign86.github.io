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

Unix systems assign default names to each network device with progressive numbers to distinguish between devices of the same type. Default names can be changed with name of our choice by modifying the configuration files of Udev, the system responsible for the hardware. We assume in the following that all drivers needed to operate a device are correctly intalled.

Udev rules concerning network devices are located in the `/etc/udev/rules.d/70-persistent-net.rules` file. The file syntax is

```bash
PCI device 0x10b7:0x9201 (3c59x)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:26:54:14:ec:46", ATTR{dev_id}=="0x0",
           ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"
```

On each line we find the rule for one network device. The MAC address and the name are indicated respectively in the `ATTR{address}` and `NAME` fields.\
To find the device we are interested in, we can use the `ifconfig` command

```bash
ifconfig
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

The rules in the `/etc/udev/rules.d/70-persistent-net.rules` file, can be overridden by creating another file inside the `/etc/udev/rules.d/` directory. It is important that the file name follows the same syntax as the other files present in the directory

`<NN>-<new_rules>.rules`

The number `<NN>` represent a priority ordering where lower numbers have higher priority and therefore superseed higher number file with lower priority. In the new file we just created, add a line (only one line) for each device to be renamed. For example

```bash
touch 40-newrules-net.rules
echo KERNEL=="old_name_prefix*", SYSFS{address}=="MACaddr", NAME="new_name" > /etc/udev/rules.d/40-newrules-net.rules
```

where `old_name_prefix` is the old device name without the number (for example `eth`, `wlan`), `ACaddr` is the MAC address, `new_name` is the new name we want to give to it (must be lowercase!). For example

```bash
KERNEL=="eth*", SYSFS{address}=="00:26:54:14:ec:46", NAME="cable1"
```

To use new names the network configuration must be updated. For each changed device replace the new name in the `/etc/network/interfaces` file.

To verify that all works as expected, reload the networking with (on Debian and *buntu)

```bash
/etc/init.d/networking restart
ifconfig new_name
```

A similar command is available for any distribution.

Modifying the name of network devices is a simple but not always safe task. Many softwares assume standard interface names and may have unpredictable behaviors these get changed. Therefore, is preferable to adhere to the standard names like `ethN`, `wlanN` whenever possible.
