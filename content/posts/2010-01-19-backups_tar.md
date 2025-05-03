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

When a hard disk breaks up we loose data. As recovering data from a broken hard disk is almost impossible in most cases, the best solution is to have a updated backups. To backup data or even the whole operating system, is incredibly simple in Linux since all required tools are generally available in every Linux installation. To set up a working backup system, the only tools needed are `tar` and the `gz` or `bzip2` libraries to use compression algorithms.

## Backup

To backup the whole system use

```bash
cd /
tar cvpf backup.tar.gz --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/proc --exclude=/sys /
```

This creates a backup of the whole root directory `/` with the exception of the directories explicitly excluded using the `--exclude=<path>` flag. Adding the `-z` to use `gzip` or `-j` to use `bzip2`, the archive uses less memory. For exemple

```bash
tar cvpzf backup.tar.bz2 --exclude=/lost+found --exclude=/media --exclude=/mnt --exclude=/proc --exclude=/sys /
```

The `-p` option preserves the file permissions in the backup. This is important when restoring a full system where reset all permissions would be a daunting task.
I find useful to insert the backup date inside the archive name

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

**WARNING!!!**: Restoring the archive as shown overwrites the content of the destination folder! Be careful, but whenever the root directory is involved, be extremely careful.

## Restore single files or directories

`tar` allows to cherry-pick out from the archive single files

```bash
tar zxvpf backup.tar.gz /etc/apt/sources.list
```

this command will overwrite `sources.list` with the backup'd copy in the archive. No other files are touched. With the same method we can extract also single directories

```bash
tar zxvpf backup.tar.gz ~/my_data
```

## Options review

Here is a review of the options used in this howto

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
