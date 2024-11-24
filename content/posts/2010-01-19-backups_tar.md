---
title: Backups with tar
date: 2010-01-19
modified: 2011-09-15
tags:
  - linux
  - shell
  - base_command
categories:
  - base_commands
author_profile: true
draft: false
comments: false
slug: backups-tar
---

When an hard disk breaks up or when we do some dirty experiment that go bad, we loose data. Recover data from a broken hard disk is almost impossible and the only hope for our work is to have a backup. To backup some data or even the whole operative system is incredibly simple in Linux, probably more than Windows. In Win you need an external program to backup your files and a backup of Win itself is difficult due to the fact that most system files are not accessibles. You need to buy some proprietary and expensive tool. On Unix all files are accessibles at run time and all the needed tools are all present in all installations. So no problems at all. To do all the work we need only the program tar and the gz or bzip2 library to use compression algorithms. In all installations all this library are installed by default.

For a wider introduction to `tar` read also "Tar archiver how to".

## Backup

To backup the whole system give

```bash
cd /
tar cvpf backup.tar.gz --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/proc --exclude=/sys /
```

We are doing the backup of the root directory except for the directories excluded using the `--exclude=<path>` flag. Adding the `-z` to use gzip or `-j` to use bzip2, the archive will use less memory. For exemple using gzip

```bash
tar cvpzf backup.tar.bz2 --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/proc --exclude=/sys /
```

The `-p` option preserve permissions of original files inside the backup. In this way you will not find all your permissions gone away and screwed when you do the restore.
I find useful to insert the backup date inside the archive name. Using the shell is very simple

```bash
tar cvpzf backup-`date '+%d-%B-%Y'`.tar.gz --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/proc --exclude=/sys /
```

The archive name is like `backup-05-march-2010.tar.gz`.

## Restore

To restore the backup enter

```bash
tar xvpzf backup.tar.gz -C /
```

or

```bash
tar xvpjf backup.tar.bz2 -C /
```

**WARNING!!!**: This will definitely overwrite the content of the destination folder! You must be careful, in this case where the root directory is involved, you must be VERY careful.

## Restore single files or directories

The command allow to pick out from the archive only a single file if we want replacing the old one with the new one

```bash
tar zxvpf backup.tar.gz /etc/apt/sources.list
```

From the root directory this command will overwrite the old version of the `sources.list` file with the one inside the archive. All the other files in the archive are skipped. With the same method we can extract also single directories

```bash
tar zxvpf backup.tar.gz ~/my_data
```

## Options review

To be more clear let's do a review of the options used

| Option         |                                                             |
|:--------------:| ----------------------------------------------------------- |
| `-c`           | create a new archive                                        |
| `-x`           | extract from an archive                                     |
| `-p`           | keep permissions. Valid during both creation and extraction |
| `-f <archive>` | name of the archive                                         |
| `-z`           | use the gzip algorithm to compress/uncompress               |
| `-j`           | use the bzip2algorithm to compress/uncompress               |
| `-d`           | search for differences between archive and filesystem       |
| `-u`           | archive update. Add to the archive only updated files       |
| `-C <folder>`  | go to the directory before doing operations                 |
| `-v`           | verbose                                                     |

For a wider introduction to tar read also "Tar archiver how to".
