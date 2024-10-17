---
title: "Restart the X server"
date: 2008-03-29
modified: 2011-10-03
tags:
  - linux
  - X
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

How to force the restart of your X server when needed? Some quick methods...

### 1

This a graphical methods:

```bash
Ctrl + Alt + Backspace
```

This combo might be disabled on your window manager. To enable it in Gnome go to `System -> Preferences -> Keyboard`. Go to the Layouts tab and click the Options button. Enable "Key sequence to kill the X server".\\
In KDE go to the `Menu -> System Settings -> Input devices -> Keyboard` and you will find the "Key sequence to kill the X server" option.

### 2

A quick method to restart the X system in Debian is to go to one terminals (for example `[Ctrl + Alt + F1]`) and type

```bash
# /etc/init.d/gdm restart
```

Substitute `gdm` with `gdm3`, `kdm` or any other manager you are using. In alternative you can do the same in two time with

```bash
# /etc/init.d/gdm stop
# /etc/init.d/gdm start
```

### 3

An another version of the former method use the command service. The syntax is

```bash
service <script> <action>
```

where `<script>` is the same script you find in the `/etc/init.d/` directory and `<action>` is one of the accepted actions. For example

```bash
# service gdm restart
```

or

```bash
# service gdm stop
# service gdm start
```

### 4

In Fedora simply change the run level to one without the X server running. Usually you need to change from the 5 to the 3 entering

```bash
# init 3
```

from a terminal outside the X server (`[Ctrl + Alt + F1]`).

### 5

Use the `killall` command and restart the X server with `startx`. The difference with the previous is that in this case you will go directly inside the graphical session logged with the same user that run `startx`. With the methods 1 and 2 you will have to login with the user you want.

```bash
# killall gdm
# startx
```

As before substitute `gdm` with `gdm3`, `kdm` or any other manager you are using.
