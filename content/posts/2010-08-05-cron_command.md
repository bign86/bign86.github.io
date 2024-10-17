---
title: "The cron command"
date: 2010-08-05
modified: 2013-12-30
tags:
  - linux
  - shell
  - base_command
  - scheduling
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

Cron is a basilar program on Unix and is installed by default on every distribution with the program `crontab`. Its duty is to do tasks at given time periodically.\\
To know which tasks perform and when it reads some files on the system.

| File                        | Content                                                                                                                                                                                                                          |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/var/spool/cron/crontabs/` | this folder contains one `crontab` file for each user. These files are not for manual modifications but they should be configured with the `crontab` program. This behavior was adopted to be sure the syntax is always correct. |
| `/etc/crontab`              | all the system wide configurations are here. You can modify this file by hand.                                                                                                                                                   |
| `/etc/cron.d`               | packages installed on the system that need to set `cron` jobs, put their files in this folder. Normally there is no need to do modifications here.                                                                               |
| `/etc/cron.hourly`<br>`/etc/cron.dayly`<br>`/etc/cron.weekly`<br>`/etc/cron.monthly` | these folders contain scripts to execute with the frequency indicated by the folder name.                                                                               |

**Permissions**: all the scripts executed by `cron` must not be writable by group or other. Always set the permission to `644`.

## Crontab syntax

Using the correct syntax we can use `crontab` to modify files in `/var/spool/cron/crontabs/` or we can modify by hand the `/etc/crontab` file.
With only one line we tell `cron` which command launch and when. The syntax is

```bash
* * * * *  /usr/local/bin/command.sh
```

Replace the path with yours. This instruction is formed by 6 fields. The sixth is the command with its path. The first 5 are the time we want the command executed and in this example are filled with asterisk.\\
Meaning of each asterisk in the following table

| Field | Meaning          | Values accepted                                                                         |
|:-----:| ---------------- | --------------------------------------------------------------------------------------- |
| 1st   | minute           | 0-59                                                                                    |
| 2nd   | hour             | 0-23                                                                                    |
| 3rd   | day              | 1-31                                                                                    |
| 4th   | month            | 1-12 or first three letters of the month name (case insensitive)                        |
| 5th   | day of the week  | 0-7 (0 and 7 are both Sunday) or first three letters of the day name (case insensitive) |
| 6th   | path and command | -                                                                                       |

**N.B.**: Be aware to insert the absolute path of the command. `cron` runs in an environment where `$PATH=/usr/bin:/bin`, not the usual one, so it's not sure that the path of your command is known. The absolute path is the safe way.

In each field the asterisk `*` means "every possible values" like in the normal behavior of the shell.

## Ways to introduce timings

**Simple (only one time)**:\\
to execute at each month the 10th, 2:30pm the string is

```bash
30 14 10 * *  /usr/local/bin/command.sh
```

**Multiple values**:\\
Instead of an interval we can insert multiple values using commas. In this example we execute in Sunday, Tuesday, Thursday

```bash
30 14 * * 0,2,4  /usr/local/bin/command.sh 
```

**Intervals**:\\
the hyphen `-` is used for intervals of values. To execute every day from Sunday to Tuesday enter

```bash
30 14 * * 0-2  /usr/local/bin/command.sh
```

**Cadenced**:\\
with the slash `/` we can indicate how often do the execution. For example to execute every 5 minutes, doesn't matter what minute is

```bash
*/5 14 * * *  /usr/local/bin/command.sh
```

**Combine**:\\
You can combine all together. To execute from 2:00 pm to 2:30 pm every 5 minutes each day enter

```bash
0-30/5 14 * * *  /usr/local/bin/command.sh
```

**Note on the day setting**:\\
There are two ways to set the day: the 3rd field (day of the month) and the 5th field (day of the week). If both are set with a value different from `*`, the command will be executed every time at least one of the two conditions is met. In the following example the execution start at the 00:00 every 10 and 20 of the month plus every Monday

```bash
0 0 10,20 * 1 /usr/local/bin/command.sh
```

**Shortcuts**:\\
Instead of the first 5 fields you can use a shortcut with a `@` to indicate particular timings

| Shortcut    | Meaning                                             |
| ----------- | --------------------------------------------------- |
| `@reboot`   | at the boot                                         |
| `@yearly`   | at 00:00 January the 1st (is like `0 0 1 1 *`)      |
| `@annually` | as `@yearly`                                        |
| `@monthly`  | at 00:00 every month the 1st (is like `0 0 1 * *`)  |
| `@weekly`   | at 00:00 every week on Sunday (is like `0 0 * * 0`) |
| `@daily`    | at 00:00 every day (is like `0 0 * * *`)            |
| `@midnight` | as `@daily`                                         |
| `@hourly`   | every hour, every day (is like `0 * * * *`)         |

**Syntax exception for the `/etc/crontab` file**:\\
This file follow strictly the `cron` syntax except for the fact that there is filed more. Before the command an extra field indicate the user who wants to execute the command. The command will have the permissions of this user.

```bash
0 0 * * 1  root /usr/local/bin/command.sh
```

Compare this line with the content of your `/etc/crontab` file.

## The crontab command

We have seen the syntax `cron` can understand but not how to write the `crontab` file using the `crontab` command. This command is the only way a normal user without root permissions can use to set up `cron` jobs.

To edit your personal `crontab` file enter

```bash
$ crontab -e
```

Your `crontab` file will be opened in the default text editor. If you don't have a `crontab` file a new empty file is created. Now you can edit the file using the `crontab` syntax explained above.

To see the content of your `crontab` file enter

```bash
$ crontab -l
```

To delete your file

```bash
$ crontab -r
```

If you are the root user, with the `-u` option you can edit other user's files. For example if the root want to see the root's file

```bash
# crontab -u root -l
```

## Default folders

If you have a script that you want to execute daily or monthly, you can put the script inside the correct folder and `cron` will take care of the execution. This is a very fast method but allows for little flexibility (works with the shortcuts method). The folders are `/etc/cron.daily`, `/etc/cron.hourly`, `/etc/cron.monthly`, `/etc/cron.weekly`.

The files present in `cron.*` will be executed using `run-parts`. The naming of these files is important to `run-parts` and a bad file name will cause the script to be silently skipped. The naming convention can be read in the man page

```bash
$ man run-parts
```

Unless we want the script to belong to some particular group of scripts, we must fulfill the simplest naming convention that says (from the man page):

If neither the `--lsbsysinit` option nor the `--regex` option is given then the names must consist entirely of ASCII upper- and lower-case letters, ASCII digits, ASCII underscores, and ASCII minus-hyphens.

## Environment variables

You can define environment variable inside a `crontab` file. For example in mine file there are

```bash
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```

Especially four variables can be very useful:

- `PATH`: in your crontab file you can define a `PATH` variable with some personal non-standard path.
- `MAILTO`: is not present a mail is send to the owner of the crontab file when a job is performed. If present but empty (`MAILTO=""`) mail are disabled (are annoying if you execute a command very often).
- `SHELL`: the default shell is `/bin/sh` that is a link to the standard shell of your box. You can use the shell you prefer setting this variable.
- `HOME`: the default is the users's home from the `/etc/passwd` file. If we want we can modify the home.
