---
title: "The chmod command"
date: 2008-05-26
modified: 2011-08-16
tags:
  - linux
  - shell
  - base_command
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---


With the `chmod` command you can change permission settings for both files and directories on the system. Principal permissions are 3 referred in order to the owner of the file or directory, the group and the rest of the world. There are two ways of using this command: octal and symbolic.

## Octal mode

You must specify the permissions using 3 numbers from 0 to 7 calculated summing the particular permissions you want to give to that user. The values are

| Perm | Meaning     |
|:----:| ----------- |
|  4   | read (r)    |
|  2   | write (w)   |
|  1   | execute (x) |

If you want to give read and write permissions to all users you must specify 6 = read + write.

```bash
$ ls -l
-rw-r-----  1 nero   nero        108 21 jan  2008 my_File
$ chmod 666 my_file
$ ls -l
-rw-rw-rw-  1 nero   nero        108 21 jan  2008 my_File
```

## Symbolic mode

First you must specfy who is affected, owner (`u`), group (`g`), others (`o`). Then you have to say if you want to add (`+`) or subtract (`-`) permissions and what permission to add or drop using the 3 letters set (`rwx`) instead of 3 numbers. You can also say the exact permissions mask you want to apply with the equals sign (`=`). With this all permissions you have specified are added, all others are removed.

To give group permissions to read and write you use

```bash
$ chmod g+rw my_File
```

while the command

```bash
$ chmod g=rw my_File
```

states that if the execute (`x`) permission is present it has to be removed.

If you want to apply modifications to all kind of users you can specify all (`a`). Note that if you do not state the user in the command, all is applied. So

```bash
$ chmod a+r my_File
```

and

```bash
$ chmod +r my_File
```

are exactly the same.

## Final note

The symbolic method is more straightforward to use but the `chmod` command internally understand only the octal method. So if you use the symbolic mode the program will calculate for you the octal mask.
