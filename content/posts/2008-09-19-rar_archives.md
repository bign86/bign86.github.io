---
title: "Split/merge Rar archives"
date: 2008-09-19
modified: 2011-08-28
tags:
  - linux
  - shell
categories:
  - archiving
author_profile: true
draft: false
comments: false
---

To upload many data on an online service or to transport a big archive in a small device you can be forced to spilt your big archive in smaller pieces. How to do this with rar? And how to merge this pieces back?

## Split

To split the content of a folder in pieces the command is

```bash
$ rar a -v100M r archive.rar folder/
```

In this way you are breaking the content of `folder/` in 100Mb pieces. You can also disable recursion omitting the `r` option or inserting the `r-` option.\\
Playing with options you can create smaller pieces (500Kb for example) compressed with the highest compression level

```bash
$ rar a -v500k -m5 archive.rar folder/
```

The default compression level is 3. The 5 is the highest and slower while 0 creates very large files but very fast.

## Merge

If you many parts of a bigger file to merge the command is very simple

```bash
$ unrar x archive.part01.rar
```

Rar will join the files for you.
