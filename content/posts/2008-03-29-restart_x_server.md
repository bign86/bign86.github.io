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

Some quick methods to force the restart of your X server.

### 1

From a X11 session:

```bash
Ctrl + Alt + Backspace
```

This combination might be disabled on your window manager. To enable it in Gnome go to `System -> Preferences -> Keyboard` and click the _Options_ button in the _Layouts_ tab. Enable "Key sequence to kill the X server".\
In KDE go to `Menu -> System Settings -> Input devices -> Keyboard` and enable the "Key sequence to kill the X server" option.

### 2

Another quick method to restart the X system is

```bash
/etc/init.d/gdm restart
```

Substitute `gdm` with `gdm3`, `kdm` or any other manager you are using. The same can be done in two steps with

```bash
/etc/init.d/gdm stop
/etc/init.d/gdm start
```

### 3

Using the `service` command the syntax is

```bash
service <script> <action>
```

where `<script>` is the same script you find in the `/etc/init.d/` directory and `<action>` is one of the accepted actions. For example

```bash
service gdm restart
```

or

```bash
service gdm stop
service gdm start
```

### 4

In Fedora, the run level can be changed directly. Runlevels 3 does not have a X11 server running. Therefore use

```bash
init 3
```

from a terminal _outside_ the X server (`[Ctrl + Alt + F1]`).

### 5

Use the `killall` command and restart the X server with `startx`. The difference with the previous options is that in this case you will go directly inside the graphical session logged with the same user that run `startx`. With methods 1 and 2 you have to login again.

```bash
killall gdm
startx
```

As done above, substitute `gdm` with `gdm3`, `kdm` or any other manager in execution.
