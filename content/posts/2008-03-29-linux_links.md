---
title: "Hard and soft links in Unix"
date: 2008-03-29
modified: 2011-08-25
tags:
  - bash
  - shell
  - linux
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

Links are connections between files and folders inside the filesystem that are very useful as they allow to shorten up the paths to access resources or create entry points with different names. In Unix the command to create a link is `ln`.\\
The syntax is

```bash
ln [options] [source_file] [link_name]
```

In this way you create a special file (the link) that refer to the real file that is `source_file`.\\
There are two different types of link in Unix. The hard link is a connection with the data of another file, while the soft link (or symbolic link) is a connection with the name of another file. There are few differences between these two kinds of link.

## Hard Links

The hard link is a connection with the data inside a file. Every time a file is created on the disk an amount of memory is used. To allow the system to access such data a name for that data (the file name) is required. So an hard link is created. When the hard link referred to that data is removed, the memory is considered freed and rewritable. You have deleted a file.

The same data can be pointed by many hard links. In practical the same file has different names. The number of hard links pointing to the same file is counted (a integer variable called reference counting) and the file cannot be deleted (the memory can not be freed) until there is at least one hard link pointing to it. Therefore, to completely delete a file in the filesystem and free its memory, you need to delete all the hard links pointing to it.

To create an hard link in Unix is simple

```bash
$ ln pointed_file link_name
```

We have associated a second hard link to the same file. We can see the reference counting (the first field after permissions on the file, before the owner field) increased by 1

```bash
$ ls -l | grep physical_data
-rw-r--r--  1 nero nero   10181  1 ott  2010 physical_data
$ ln physical_data new_link
$ ls -l | grep physical_data
-rw-r--r--  2 nero nero   10181  1 ott  2010 physical_data
```

## Limitations

* An hard link must refer to physical data on the same filesystem volume.
* There can't be two hard links pointing to the same directory or a directory must have always reference counting 1. This is to avoid ambiguities in referring to parent directories and endless loop in recursions.

## Soft Links

The soft link (or symbolic link or symlink) does not refer to the data of a file, but to its name and contains the relative path to that file. As such, the link is independent from the pointed file. If the pointed file is modified the link is unaffected, and if the link is modifiesd or deleted, the file is unaffected.

The soft link has more flexibility than the hard link. It can be used to create connections to files on a different filesystem and can point also to directories. The independence from the file means that if the file is deleted, the link doesn't know and continue to point to a non existing file. In this case the link is orphan.

Permissions on a symlink in Unix are irrelevant, as access is regulated by the permissions on the pointed to file. Hence, permissions for a symlink are usually `rwxrwxrwx` (or `777`).

To create a symlink use the `-s` option

```bash
$ ln -s pointed_file link_name
$ ls -l | grep link_name
lrwxrwxrwx  1 nero nero   10181  1 ott  2010 link_name -> pointed_file
```
