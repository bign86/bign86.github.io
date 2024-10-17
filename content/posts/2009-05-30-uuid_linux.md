---
title: UUIDs in Linux
date: 2009-05-30
modified: 2011-10-21
tags:
   - linux
   - shell
   - system
categories:
   - system
author_profile: true
draft: false
comments: false
math: true
slug: uuids-linux
---

In the kernel branch 2.1 a new way of dealing with devices have been introduced. Instead of the old naming convention for devices, the new system is called UUID, Universally Unique IDentifier.
What are the UUIDs and what do they do? How to use them?

## What is it a UUID?

The UUID is a 32 hexadecimal characters long string (for total of 128 bits) associated with one device or partition. In this way you can identify each CD reader, each partition using only the alphanumeric string associated without ambiguity. The UUIDs are defined and described in the RFC4122.

The big advantage is when you have to do some hardware change. Consider the following example.\
There are two hard disks: `hda` and `hdb`. You want to install another one. If you put the new one between the existing two it will be named `hdb` and the previous `hdb` will be `hdc`.
This change is not know to the system that will try to load partitions written in the `fstab` file. Since the `fstab` file is not updated the system will not find the partitions is searching for and will give errors. If some of the partitions on the old `hdb` are fundamental for the system the whole boot process will fail.\
In this simple case the solution is simple. Before making the change, edit the `fstab` file and update it with the new naming scheme, then add the new disk and reboot. When you have few drive and partitions there are no problems. But if you are dealing with a very big server the problem is also bigger.\
With the UUID you don't have to worry about naming schemes because the UUID are unique for each partitions and independent from the order of the drives in the BIOS chain.

## Check if your system is using UUID

To check you are using UUID print the `/etc/fstab` file

```bash
~# cat /etc/fstab
```

You are using UUID if the output is similar to

```bash
# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
proc            /proc           proc    defaults        0       0
UUID=126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf       /               reiserfs defaults        0       1
UUID=7c066088-292b-514e-9621-a750c803729c       /boot           ext2     defaults        0       2
UUID=86135950-ab3b-6c2a-8e7b-07b61bced1e9       /home           reiserfs defaults        0       2
UUID=b3c4bd58-c9ab-2e4a-b8ce-f326790eae53       /usr            reiserfs defaults        0       2
UUID=96fa5f98-1e4b-42de-a0ba-b7cbe86616c9       none            swap     sw              0       0
UUID=a19b5808-e56b-9d4e-81d9-9fb4f6043d33       /media/cdrom0   udf,iso9660 user,noauto     0       0
```

Instead you are not using UUID if the output is similar to this one

```bash
# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>   <options>       <dump>  <pass>
proc            /proc           proc     defaults        0       0
/dev/sda6       /               reiserfs defaults        0       1
/dev/sda5       /boot           ext2     defaults        0       2
/dev/sda8       /home           reiserfs defaults        0       2
/dev/sda7       /usr            reiserfs defaults        0       2
/dev/sda3       none            swap     sw              0       0
/dev/hda        /media/cdrom0   udf,iso9660 user,noauto     0       0
```

If you are not using UUID in your `/etc/fstab` file doesn't mean that your partitions don't posses an UUID. To know that use the `blkid` command. The output is similar to the following if your partitions have a UUID.

```bash
~# blkid 
/dev/sda1: LABEL="backup" UUID="bed842f4-f181-4bd0-8a07-717e52297851" TYPE="ext3" 
/dev/sdb1: UUID="ba9ff070-601c-4849-9cc4-5dc1b1fe6283" TYPE="ext3" 
/dev/sdb2: UUID="8b20d8c9-2172-4ccd-b38c-da4a998fe360" TYPE="swap" 
/dev/sdb3: UUID="8d14411c-099e-4b21-8e9b-d9b760bfb0e4" TYPE="ext4"
/dev/sdb4: UUID="1ffb8b88-c8f8-417f-8b4b-9cfea0848467" TYPE="reiserfs" 
```

The column `UUID="..."` is absent if your partitions don't have the UUID.

Another way to know if your partitions use UUIDs is to just list them accordingly to the (potential) UUID.

```bash
~$ ls /dev/disk/list-by-uuid
```

## Start using UUID if you have them

The `blkid` command said that you have UUID? Ok, your system is ready to use them immediately (if you are not already). Using again this command find out the UUID for each partition and substitute in the `/etc/fstab` file the old name `/dev/sdX` with the label `UUID="..."` and the number given you by the `blkid` command.

Reboot the system.

## Generate the UUID if you haven't them

If your partitions don't have a UUID assigned, you need to generate a new one. To do that there is a command called `uuidgen` that is a generator. On Debian you will need the `uuid-runtime` package, the `uuid` package on Fedora, but the name should be more or less similar everywhere.

```bash
~# apt-get install uuid-runtime         # on Debian and derivates
~# yum install uuid                     # on RH / Fedora
```

Generate your UUID using the `libuuid` tools like `uuidgen`.

```bash
~# uuidgen
```

There are a couple of options. The `-r` option tells the command to use a random generator to create the ID. This is also the default behavior. The `-t` option tells the command to combine the system clock and the MAC address of a random network device on the machine to generate the ID. For us is the same.\
Also `proc` is able to generate UUIDs using the following command

```bash
~$ cat /proc/sys/kernel/random/uuid
7e3bf4b1-fb64-41ea-aed2-cc242f8385a5
```

Now assign the new UUID to the partition using a command specific for the filesystem. Here a small example

```bash
~# reiserfstune -u <new uuid> /dev/sdXN         # ReiserFS
~# tune2fs -U <new uuid> /dev/sdXN              # EXT2/3/4
~# jfs_tune -U <new uuid> /dev/sdXN             # JFS
~# xfs_admin -U <new uuid> /dev/sdXN            # XFS
```

where you must substitute `<new uuid>` with your newly generated UUID. For example to assign a UUID to the sdb3 partition formatted in Ext4 use

```bash
~# tune2fs -U 7e3bf4b1-fb64-41ea-aed2-cc242f8385a5 /dev/sdb3
```

Note that you have to assign the UUID to the PARTITION not to the physical device. So the target is for example sdb3 NOT `sdb`.\
Now update the `/etc/fstab` file like in the previous section.

## Some useful commands to use with UUID

* **1.** Get the UUID for a partition

   ```bash
   ~# vol_id --uuid /dev/sda6
   ID_FS_UUID=126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf
   ```

* **2.** Get attributes for a partition, label, UUID, or filesystem

   ```bash
   ~# blkid /dev/sda6
   /dev/sda6: LABEL="/" UUID="126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf" TYPE="reiserfs"
   ```

* **3.** List disks by UUID

   ```bash
   ~# ls -l /dev/disk/by-uuid | grep sda6
   lrwxrwxrwx 1 root root 10 11. Mag 12:47 126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf-> ../../sda6
   ```

* **4.** Get the partition given the UUID

   ```bash
   ~# findfs UUID=126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf
   /dev/sda6
   ```

* **5.** Mount using the UUID instead of the classic name

   ```bash
   ~# mount -U 126ae4cc-416b-2fa4-8a1c-3b45fc4be1cf /destination/folder/
   ```

## Curiosity

The UUID is a 16 bytes number, 128 bits. The number of possibilities is \(2^{16*8} = 2^{128} = 3.4\cdot 10^{38}\).
If you generate one billion of billions of UUID (\(10^{18}\)) each nanosecond (\(10^{-9}\) sec.) you are generating \(10^{18+9}=10^{27}\) UUID each second. But you will ever need 340 billions of years (\(3.4\cdot 10^{11}\) years) to run out of possibilities.
