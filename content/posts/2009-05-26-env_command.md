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

The `env` command shows all the environment variables currently set. It is also a simple way to change values to these variables. To show all the existing variables

```bash
env
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

The real output is likely to be longer. The values for each variable can be modified specifying te variable and a new value

```bash
env VARIABLE=value
```

This allows to set new variables, change existing or delete old unneeded ones. Environment variables are initialized every time you start your machine, and get reset at each reboot.
For example, to modify your `crontab` setting using the editor Emacs instead of the default one you can use `env`

```bash
env EDITOR=emacs crontab -e
```

Some shells, including Bash, don't even need the `env` command so the following line works

```bash
EDITOR=emacs crontab -e
```

There may be the need to call a command with a clean environment with only few manully set variables. The `-i` option (or `--ignore-environment`) starts a clean environment with only the variables that are explicitly defined in the command itself

```bash
env -i PATH=/$HOME/bin LIBDIR=/$HOME/lib make
```

The only variables present will be `$PATH` and `$LIBDIR`.

## Errors

If a command was called through `env`, the return code comes from the called command. But if `env` was called alone and returned an error, it is possible to refer to the following table of returning codes to understand what happened.

| Code                           | Meaning                                                     |
|:------------------------------:| ----------------------------------------------------------- |
| 0                              | All ok                                                      |
| 1                              | Error caused by either insufficient memory or name too long |
| 2                              | One argument is invalid                                     |
| 126                            | The called command exist but cannot be executed             |
| 127                            | The called command doesn't exist                            |
| Too many environment variables | We have more than 512 variables                             |
