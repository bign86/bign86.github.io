---
title: "The env command"
date: 2009-05-26
modified: 2011-08-17
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

Using the `env` command you can see all the environment variables currently set. It is also a simple way of change values of this variables. To show all the existing variables

```bash
$ env
SHELL=/bin/bash
TERM=xterm
GTK_RC_FILES=/etc/gtk/gtkrc:/home/nero/.gtkrc-1.2-gnome2
WINDOWID=46137423
USER=john
USERNAME=nero
DESKTOP_SESSION=default
PATH=/usr/local/bin:/usr/bin:/bin:/usr/games
GDM_XSERVER_LOCATION=local
PWD=/home/john
LANG=it_IT.UTF-8
GNOME_KEYRING_PID=2755
GDM_LANG=it_IT.UTF-8
GDMSESSION=default
HISTCONTROL=ignoreboth
HOME=/home/john
SHLVL=1
GNOME_DESKTOP_SESSION_ID=Default
_=/usr/bin/env
```

After the command is an example output extract. Your output is likely to be more long, depending on your environment. After every variable there is a equals sign and a value that is the current value. You can modify the value specifying one variable and a new value

```bash
$ env VARIABLE=value
```

This permit us to set new variables, change existing or delete old unneeded ones we have defined time ago. Fortunately environment variables are initialized every time you start your machine, so are reset with a reboot.
For example to modify your `crontab` setting using the editor Emacs instead of the default one you can use `env`

```bash
$ env EDITOR=emacs crontab -e
```

Some shells, including the Bash, don't even need the `env` command so this line will work

```bash
$ EDITOR=emacs crontab -e
```

There is sometimes the need to call a command with a clean environment with only few variables we want to set manually. The `-i` option (or `--ignore-environment`) start a clean environment with only ours variables

```bash
$ env -i PATH=/$HOME/bin LIBDIR=/$HOME/lib make
```

The only variables present will be `$PATH` and `$LIBDIR`.

## Errors

For any error `env` reports a code number. If we called a command with `env` the return code is the same of the called command. If we called `env` alone there is a table of returning codes

| Code | Meaning                                                         |
|:----:| --------------------------------------------------------------- |
| 0    | All ok                                                          |
| 1    | Error caused by\\* insufficient memory\\* name too long         |
| 2    | One argument is invalid                                         |
| 126  | The called command exist but cannot be executed                 |
| 127  | The called command doesn't exist                                |
| Too many<br>environment<br>variables | We have more than 512 variables |
