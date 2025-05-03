---
title: "Userdel and deluser"
date: 2009-09-12
modified: 2011-08-31
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

In Unix there are two commands to delete users and groups: `userdel` e `deluser`. Despite the same purpose they are quite different in their use.

## Userdel

This is ​​​​​a low-level command potentially harmful. It deletes the user from the system with all the files belonging to him/her.

```bash
userdel -f user
```

The `-f` op​​​​​tion forces the removal even if the user is connected to the system. The user directory inside `/home/` and all the mails will be deleted even if another user has the files currently open.

Inside the `/etc/login.defs` file you can define

```bash
USERGROUPS_ENAB yes
```

With this option enabled the `-f` option deletes the group with the same name of the user (if any) even if another user is part of it.

## Deluser

This is a very simple command

```bash
deluser user
```

The configuration file for this program is `/etc/deluser.conf`.
Without options the user is deleted but all files, mail and home directory remain. Many options allow to modify the command behavior. This a non-comprehensive list

| Option              | Meaning                                                                              |
|:-------------------:| ------------------------------------------------------------------------------------ |
| `--backup`          | do a backup of all the files of the user                                             |
| `--backup-to`       | give the place to save the archive with the backup. Default is the current directory |
| `--remove-home`     | remove the user home                                                                 |
| `--remove-all-file` | remove all the user files                                                            |
| `--group`           | remove the user group                                                                |
| `--system`          | user and group are removed only if of the system                                     |
| `--conf`            | change the configuration file to use                                                 |

The following command

```bash
delgroup
```

is an alias for

```bash
deluser --group
```

_Note_: using the option `--force` you can remove the root (`UID 0`) account. Pay attention!
