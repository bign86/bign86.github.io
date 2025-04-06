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

To upload large quantities of data on an online service or to transport large archives on a small devices, you may be forced to spilt the big archive/file in smaller pieces. How to do this with rar? And how to merge this pieces back?

## Split

To split the content of a folder in pieces the command is

```bash
rar a -v100M r archive.rar folder/
```

This breaks the content of `folder/` in 100Mb pieces. Recursion can be disabled omitting the `r` option or inserting the `r-` option.\
Playing with options allows to create smaller pieces (500Kb for example) and find higher compression ratios

```bash
rar a -v500k -m5 archive.rar folder/
```

The default compression level is `-m3`. `-m5` is the highest but slowest, while `-m0` creates very large files very fast.

## Merge

To merge parts of a single file back, the command is

```bash
unrar x archive.part01.rar
```
