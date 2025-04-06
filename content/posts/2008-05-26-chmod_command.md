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


The `chmod` command changes permission settings of both files and directories on the system. Permissions are 3: read (`r`), write (`w`), execute (`x`) respectively. These permissions can be granted to the user owning the file (`u`), the group of the owner (`g`), everybody else (`o`), respectively. The result is a set of 9 permissions that can be changed using a symbolic or octal syntax.

## Symbolic mode

First specify who is affected by the change: owner (`u`), group (`g`), others (`o`). Then if a permission is added (`+`) or subtracted (`-`). Last specify which permissions are changed: read (`r`), write (`w`), execute (`x`). You can also say the exact permission mask you want to apply with the equal sign (`=`).

For example, to add to the group read and write permissions use

```bash
chmod g+rw my_file
```

If the execution permission for the group was present, is retained. The command

```bash
chmod g=rw my_file
```

instead enforce the selected mask, adding read `r` and write `w` if necessary and removing execution `x` if present.

It is also possible to apply a change to all (`a`) types of users with

```bash
chmod a+r my_file
```

Note that if you do not state which type of user to apply the changes to, all is applied. Therefore, the following command is the same as the one above

```bash
chmod +r my_file
```

## Octal mode

With this format, numbers are used instead of letters to specify the permissions as per the following table

| Perm | Meaning       |
|:----:| ------------- |
|  4   | read (`r`)    |
|  2   | write (`w`)   |
|  1   | execute (`x`) |

To give a given set of permissions to a user type, the relative values must be summed, resulting in a triplet of numbers between 0 and 7 for the 3 types of users.

For example, to give read and write permissions to all users the mask is 666 (read (4) + write (2) for all)

```bash
ls -l
-rw-r-----  1 nero   nero        108 21 jan  2008 my_file

chmod 666 my_file
ls -l
-rw-rw-rw-  1 nero   nero        108 21 jan  2008 my_file
```

To give all permissions to the owner, only read to the group and nothing to others

```bash
ls -l
-rw-r-----  1 nero   nero        108 21 jan  2008 my_file

chmod 740 my_file
ls -l
-rwxr-----  1 nero   nero        108 21 jan  2008 my_file
```
