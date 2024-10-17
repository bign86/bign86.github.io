---
title: "MAC address spoofing on Linux"
date: 2009-06-20
modified: 2011-09-20
tags:
  - linux
  - system
  - networking
categories:
  - system
author_profile: true
draft: false
comments: false
---

The MAC address is a unique number assigned by the manufacturer at each network interface produced and stored inside the read-only memory of the interface. Usually the first three octets of the address identify univocally the manufacturer of the device.\\
On modern hardware the MAC address can be changed and doing it is far from complex. This technique is called MAC spoofing.

### Ifconfig

The simplest way is using the `ifconfig` command. First shut down the interface, change the address and start again the device

```bash
# ifconfig eth0 down
# ifconfig eth0 hw 01:02:03:04:05:06
# ifconfig eth0 up
```

Instead of `eth0` enter your interface and put the preferred MAC in the place of `01:02:03:04:05:06`.

### Ip

`ifconfig` is now deprecated in favor of the command `ip`. Using `ip` the command to change the address is

```bash
# ip link set eth0 address 01:02:03:04:05:06
```

To permanently change the MAC address modify the `/etc/network/interfaces` (on Debian and derivates). Open the file with an editor and search the section of the device to be changed. Add this line below

```bash
hwaddress ether 01:02:03:04:05:06
```

substituting with the preferred new address.

On Red Hat and RH based distributions the `/etc/network/` folder doesn't exist. In its place the is the `/etc/sysconfig/network-scripts/` folder containing one file for each device like `ifcfg-eth0` for the `eth0` device. To change the MAC address permanently open the correct file for your device. It should be similar to this one

```bash
#
# File: ifcfg-eth0
#
DEVICE=eth0
IPADDR=192.168.0.10
NETMASK=255.255.255.0
BOOTPROTO=static
ONBOOT=yes
#
# The following settings are optional
#
BROADCAST=192.168.0.255
NETWORK=192.168.0.0
```

Add this line at the end

```bash
MACADDR=01:02:03:04:05:06
```

### MAC Changer

If you don't want to deal with configuration files, MAC Changer is a tool that make changing the MAC address even simpler. The syntax to choose your preferred address is

```bash
# macchanger --mac=01:02:03:04:05:06 eth0
```

If haven't a preferred address but you want only to change the current, enter

```bash
# macchanger --another eth0
```

MAC Changer knows the list of manufacturers and their codes (first three octets). To list all codes used by a single vendor use

```bash
# macchanger --list=Dell
```

replacing Dell with the vendor you are interested in.
