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

Links are connections to files or folders that allow to shorten up the paths to access resources or create entry points with different names. In Unix, the command to create a link is `ln`

```bash
ln [options] [source_file] [link_name]
```

This creates a special file (the link) that refers to the real file (`source_file`).\
There are two different types of link: the _hard_ link is a connection to the data of another file, the _soft_ link (or _symbolic_ link) is a connection to the name of another file.

## Hard Links

The hard link is a connection to the data of a file. When a file is created and some memory is used to store its content, a hard link is created with a name (the file name) to access it. When the hard link is removed, the memory is considered freed. The file has been deleted.\
There can be several hard links pointing to the same data with different names. These are counted and the memory is not considered free as long as there is at least one hard link pointing to it.

To create an hard link

```bash
ln source_file link_name
```

This creates a new hard link (a new file) named `link_name` pointing to the same content already accessed through `source_file`. The reference counting (the first field from `ls` after permissions, before the owner field) increases by 1

```bash
ls -l | grep source_file
-rw-r--r--  1 nero nero   10181  1 ott  2010 source_file

ln source_file link_name

ls -l | grep source_file
-rw-r--r--  2 nero nero   10181  1 ott  2010 source_file
```

## Limitations

* A hard link must refer to data on the same filesystem volume.
* There cannot be two hard links pointing to the same directory. In other words directories must have reference counting 1. This is to avoid ambiguities in referring to parent directories and avoid endless loops in recursions.

## Soft Links

The soft link (or symbolic link or symlink) does not refer to the data of a file, but to its name and contains the relative path to the file. The soft link can be used to create connections to files on a different filesystem and can also point to directories.\
Note that if a file or directory is deleted and hence the memory freed, the soft links pointing to them are left orphan as they don't partecipate to the reference counting.

Permissions on a soft link are irrelevant, as access is regulated by the permissions on the pointed file. Hence, permissions for a soft link are usually `rwxrwxrwx` (or `777`).

To create a soft link use the `-s` option

```bash
ln -s source_file link_name

ls -l | grep link_name
lrwxrwxrwx  1 nero nero   10181  1 ott  2010 link_name -> source_file
```
