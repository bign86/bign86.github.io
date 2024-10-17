---
title: "Repair a broken Linux filesystem"
date: 2013-06-30
modified: 2015-11-04
tags:
  - linux
  - filesystem
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

In this howto I collected all the standard approaches to recover a broken filesystem for the most common filesystems. Since every filesystem usually comes with a set of tools to deal with problems and errors, the software used in this howto should be available to anyone.

## Content

- [Content](#content)
- [General infos](#general-infos)
- [ReiserFS](#reiserfs)
  - [Repair errors on ReiserFS](#repair-errors-on-reiserfs)
  - [Repair a superblock on ReiserFS](#repair-a-superblock-on-reiserfs)
- [Ext2/3/4](#ext234)
  - [Repair errors on Ext](#repair-errors-on-ext)
  - [Repair a superblock on Ext](#repair-a-superblock-on-ext)
- [XSF](#xsf)
  - [Repair errors on XSF](#repair-errors-on-xsf)
  - [Repair a superblock on XSF](#repair-a-superblock-on-xsf)
- [Working on an image](#working-on-an-image)

## General infos

The standard tool under Linux to check and repair filesystems is `fsck` ("FileSystem ChecK"). The tool can be invoked with the option `-t` to specify witch filesystem it has to operate on, or calling directly the correct subprogram. Example:

```bash
# fsck -t ext2
```

or

```bash
# fsck.ext2
```

`fsck` is in reality only a frontend to checkers shipped with each filesystem to standardize the checks at during the boot process. It is also very useful in scripts where we can obtain the filesystem type using some command and then call the appropriate checker using always `fsck` as an interface. We can also always call directly the tools specific for each filesystem. In general is better to call directly the specific tools when we want to do something complicated that `fsck` is not able to handle being a common frontend to several different checkers.

Filesystems cannot be repaired while running. Before doing any operation the filesystem must be unmounted and everything must be done as superuser. To unmount the `/home` residing on `/dev/sda5`

```bash
# umount /dev/sda5
```

or

```bash
# umount /home
```

When the filesystem is repaired you can mount it back

```bash
# mount /dev/sda5
```

If the root filesystem is broken you will of course need a live distribution on CD or usb key to work on it. If you don't know the filesystem type of your device use

```bash
# mount | awk '{print $1 "  " $5}'
```

'''Warning!''' Working directly on the filesystem may be EXTREMELY dangerous for your data. When working on real data ALWAYS backup you partition with your preferred tool.

## ReiserFS

Very old filesystem, but was the working horse of my desktop for 10 years (and is still working fine), so I'm fond of it. The tools are in the `reiserfsprogs` suite.

### Repair errors on ReiserFS

To check the filesystem use

```bash
# reiserfsck --check /dev/sdXY
```

This won't correct anything and will report found errors. At the end of the analysis `reiserfsck` gives us some hint on what has to be done to correct the errors. Small errors that don't require rebuilding the filesystem tree can be corrected with

```bash
# reiserfsck --fix-fixable /dev/sdXY
```

If errors that require a new tree are present we must do a

```bash
#reiserfsck --rebuild-tree /dev/sdXY
```

Occasionally you can experience data losses when rebuilding a tree. On modern kernel this should be anyway very unlikely.

### Repair a superblock on ReiserFS

If the superblock is not readable use

```bash
# reiserfsck --rebuild-sb /dev/sdXY
```

to fix the missing superblock.

## Ext2/3/4

Probably the most used family of filesystems. Likely you have at least one device with one of these. The suite of tools really doing the job is called `e2fsprogs`. Following command are valid for Ext2/3/4 indifferently.

### Repair errors on Ext

A dry-run without actually modifying the partition can be done using

```bash
# e2fsck -n -f /dev/sdXY
```

Then if the simulation goes well, to repair simple errors

```bash
# e2fsck -f /dev/sdXY
```

All recovered files will be placed under the `lost+found` directory on the same filesystem. To automatically say "yes" to all the eventual questions use `-y`

```bash
# e2fsck -f -y /dev/sdXY
```

To show a progress bar use the option `-C` in `fsck` or followed by a 0 in `e2fsck`.

```bash
# e2fsck -f -C 0 /dev/sdXY
```

### Repair a superblock on Ext

The superblock is damaged if you get something like

```bash
# e2fsck /dev/sda5
e2fsck 1.42 (29-Nov-2011)
e2fsck: Superblock invalid, trying backup blocks...
e2fsck: Bad magic number in super-block while trying to open /dev/sda5
```

ExtN filesystems copy the superblock several times in the filesystem itself. To know where all the superblocks are

```bash
# dumpe2fs /dev/sdXY | grep superblock
```

Then you can ask `fsck` to repair the filesystem providing the position of a superblock backup

```bash
# e2fsck -b 98304 /dev/sdXY
```

Sometimes `dumpe2fs` cannot read the partition at all like in this case

```bash
# dumpe2fs /dev/sda5
dumpe2fs 1.42 (29-Nov-2011)
dumpe2fs: Bad magic number in super-block while trying to open /dev/sda5
```

There are non certainties in this case. You can check what would happen if you were creating a new filesystem on the same device using `mke2fs`. With the `-n` option nothing will be really done, but what would happen is shown. You can read from here where `mke2fs` would put the backup superblocks. There is a chance that in your actual partition the superblocks are in the same position.

```bash
# mke2fs -n /dev/sdXY
mke2fs 1.42 (29-Nov-2011)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
61054976 inodes, 244190363 blocks
12209518 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
7453 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
    32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
    4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968, 
    102400000, 214990848
```

Now try to use `e2fsck` using the block listed here. To be relatively sure that the positions listed are the same the same options used to create the original filesystem should be passed again to `mke2fs` plus the `-n` to avoid writing. If you don't know which options where used (for example the block size) the only viable option is to try.

Last resort. If also guessing with `mke2fs` fail you have to use `mke2fs` to rewrite a new superblock using the option `-S`. For this desperate last attempt to work we must match the same block size used to create the partition table. In the case above for example

```bash
# mke2fs -S -b 4096 /dev/sdXY
```

should do the trick.

## XSF

`fsck.xfs` doesn't do anything at all. It's present mainly for compatibility so always use the tools specific for XSF in the suite `xfsprogs`. XSF is known to be very unhappy in case of unexpected hard reset of the machine or sudden power losses. I would avoid XFS for laptops...

### Repair errors on XSF

To repair a broken XFS device use

```bash
# xfs_repair /dev/sdXY
```

The fixing will stop if the log journal of the filesystem contains metadata to replay. To clean to log mount the device and unmount it again

```bash
# mount /dev/sdXY
# umount /dev/sdXY
# xfs_repair /dev/sdXY
```

If the partition is not mountable you need to clean the log manually using the option `-L`. Since this can create data losses, is always better to attempt a mount before using this option

```bash
# xfs_repair -L /dev/sdXY
```

To check the filesystem for consistency use `xsf_check`

```bash
# xfs_check /dev/sdXY
```

The output of `xsf_check` and `xsf_repair` must agree.

### Repair a superblock on XSF

The superblock in XFS is checked and, if needed, repaired in the first step of execution of `xfs_repair`. So to repair it use this tool as explained before.

## Working on an image

Filesystem checks and repair can be done also on an image. Working on an image file prevent us from accidentally damage even more our real partition. First we must do a low-level copy of the damaged file system to an `.img` file

```bash
# dd if=/dev/sdXY of=/mnt/broken_fs.img
```

Be sure that the partition we are copying is NOT mounted. Using `dd` on a mounted filesystem might result in a corrupted image. Now we can run for example `e2fsck` if the filesystem is Ext. When everything is reported clean we can check the filesystem mounting it

```bash
# mount -o ro,loop /mnt/broken_fs.img /mnt/clean_fs
```

When we are sure that everything is ok we can rewrite the partition back in its place

```bash
# dd if=/mnt/broken_fs.img of=/dev/sdXY
```

For example to repair the superblock of the filesystem in the ExtN example we would first create a file with the first 4096 bytes empty and the corrupted filesystem in the following.

```bash
# dd if=/dev/zero of=/mnt/broken_fs.img bs=512 count=8              # first 4096 bytes are empty
# dd if=/dev/sdXY of=/mnt/broken_fs.img bs=512 skip=8 seek=8    # fill the rest with the partition
```

Then you can do the repair as described above

```bash
# e2fsck -f /dev/sdXY
```

Then try to mount the image and check whether the repair was done properly. In the affirmative case you can copy the image back on the partition.
