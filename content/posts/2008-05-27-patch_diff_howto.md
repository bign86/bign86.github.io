---
title: "Patch & diff howto"
date: 2008-05-27
modified: 2011-09-20
tags:
  - patch
categories:
  - programming
author_profile: true
draft: false
comments: false
---

Ppatch files are text file containing a list of all differences between two versions of the same file. This information can be used to tranform the old version of the file in the new version reliably and with minimal effort. This is particularly useful to modify source code files without having to do it by hand. To create and use  a patch file we need two commands: `diff` and `patch`.

Let's say that you have two workstations where you work on your project. On both you need to have the source files always up to date to the newest version. You have modified a file on the workstation one and you want to have the same version of the file also on the workstation two. So you need to create a patch file. The syntax is

```bash
diff -Nuar old_file new_file > code.patch
```

The `patch` command works also on two folders as long as the subdirectories tree is exactly the same.\
With the patch file the old version of the file on the workstation two is updated. Copy the `.patch` file and go to the directory where the sources are. Then

```bash
patch < code.patch
```

The old file on the second workstation is now identical to the new file on the first.\
This command works because the patch file was created in unified format (`-u` option) that tells to patch which files are to modify. If the patch was not created in this way specify the file to be patched.

```bash
patch old_file < code.patch
```

## Revert a patch

Patching is reversible thanks to the option `-R`. To revert enter

```bash
patch -R < code.patch
```

## Use diff and patch on folders

You can create a patch file for an entire directory with the same syntax

```bash
diff -Nuar old_dir new_dir > code.patch
patch < code.patch
```

The patch file in this case is divided in sections, each related to a single file in the directory (or subdirectory). The section start with the name of the new file labeled by three plus (`+++`) and the name of the old file labeled by three minus (`---`)

```bash
--- old/path/old_file
+++ new/path/new_file
```

## Strip levels

The old and the new path could be different. This may be a problem especially if the new and the old version of the file are at a different level in the tree. For example in the following case the difference in deepness of the two files probably will prevent you to patch the code.

```bash
--- old/very/very/long/path/old_file
+++ new/path/new_file
```

The `patch` command allows to strip out levels in the path to correctly identify the files to be patched. Consider the case where the diff file contains the full path of the files to patch, but it is different from the location of those files in the current machine, for example because the patch file was created by a different user. `patch` can disregard a number of levels in the path to ensure a common path is found.
For example, say the original file to patch was located in `/home/user/libraries/libfoo/src/code.c` but on the current machine is located in `/home/user/projects/coollibs/libfoo/src/code.c`. The common path (`libfoo/src/code.c`), is found by removing 3 levels of folders from the original path (`/home/user/libraries/`). To patch the code, go to the root of the common path and remove the 3 extra layers

```bash
cd /home/user/projects/coollibs/
patch -p3 < code.patch
```

In general

```bash
patch -pN < code.patch
```

Where `N` is the number of level to strip. Default is 0.
