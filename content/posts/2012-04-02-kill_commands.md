---
title: "Kill commands"
date: 2012-04-02
modified: 2016-03-05
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

Running scripts might get stuck, start eating a lot of resources or simply never complete due to a number of reasons. When it happens, we want to terminate the script.
The termination of a process involves sending a specific signal to the correct PID. To know more about signals in Bash read the article Signals in Bash here.

## Find out the PID

To kill a process usually you need its PID number. On Unix there are many ways to find it. If you know the process name, use the simple `pidof` utility

```bash
$ pidof gedit
2465
```

If you are not sure of the program name you can use the `ps` utility in the following way

```bash
$ ps aux
```

The output is really long since it lists all the processes running on the machine. Scroll down the whole output can be very long, so if you have an idea on the name use `grep` to filter what you need

```bash
$ ps aux | grep gedit
nero    2465  1.6  2.1 451260 44708 ?        Sl   12:35   0:08 gedit /home/nero/www/whos_pos
```

Another tool is `pgrep`. This command grep out running processess returning process name and PID.

```bash
$ pgrep -l gedit
2465 gedit
```

## Killing a process

### 1. kill

To kill a process the standard tool is the `kill` command that has the following syntax followed by the PID number of the process to kill.

```bash
$ kill <SIGNAL> <PID>
```

If the signal is omitted by default `kill` sends the 15 (`SIGTERM`). If the process is completely dead and doesn't respond to this, the only solution is to terminate it at kernel level using the signal 9 (`SIGKILL`) in this way

```bash
$ kill -9 <PID>
```

### 2. pkill

`pkill` works at same way as the `kill` command but require the process name or part of it instead.

```bash
$ pkill <program name>
```

Also a part of the name is enough, so be careful. If for example you send

```bash
$ pkill g
```

this wont kill just Gedit but everything containing a "g", like for example everything belonging to Gnome.

### 3. killall

There is another utility to kill processes on Linux that is `killall`. This tool doesn't need the PID of the process, but the exact name is required. If there are more processes with the same name, all of them will be killed.The syntax is extremely simple

```bash
$ killall <SIGNAL> <program name>
```

`killall` has the same behavior of `kill`. The signal is optional and the stardard signal sent is `SIGTERM` but you can change it from the command line using the number of the signal or the name as seen for `kill`.

```bash
$ killall -9 <program name>
$ killall -SIGKILL <program name>
```

### 4. xkill

This has the easiest syntax, just

```bash
$ xkill
```

The cursor on the monitor will transform into a cross. Click on an application to terminate it. Of course it requires a running X window.

### 5. skill

Similar to `pkill` but deprecated because old and unportable. Use something else instead.
