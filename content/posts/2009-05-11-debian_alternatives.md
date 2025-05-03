---
title: "Update-alternatives in Debian"
date: 2009-05-11
modified: 2011-10-03
tags:
  - linux
  - debian
  - system
categories:
  - debian
author_profile: true
draft: false
comments: false
---

Usually on a Linux box are present at the same time more than one software providing the same functionality or service. For example, more than one browser, more than one movie/audio player, more than one text editor are installed. Normally, this is considered an advantage, as you can choose which software to use case by case. One of these applications is considered by the system the "default" one to use. But what if you want to change the default application? The way is the update-alternatives system.

## How it works

Update-alternative classify softwares using a generic name to indicate their class on the base of the service they provide. The generic name points directly to the default program for that class.\\
If you have installed many X11 browsers, say Iceweasel, Epiphany, Konqueror, and Midori, update-alternatives will put them all under the class "x-www-browser". update-alternatives sets one of your browsers to be the default one, say Epiphany, so that any time you do something that require a browser, Epiphany is started without having to explicitly call it.

## List classes

The classes that update-alternatives know are in the folder `/etc/alternatives/`.\
To list the alternatives for a class you need to use the option `--list` with the syntax `update-alternatives --list <class>`.

```bash
update-alternatives --list x-www-browser
/usr/bin/epiphany
/usr/bin/iceweasel
/usr/bin/konqueror
/usr/bin/midori
```

The `--display` option provides more informations on the selected class

```bash
update-alternatives --display x-www-browser
```

## Change the default application

How to change the default? The `--config` option lists the known alternatives and allow to choose between them

```bash
update-alternatives --config x-www-browser

There are 4 alternatives which provide `x-www-browser'.

  Selection    Alternative
-----------------------------------------------
          1    /usr/bin/epiphany
          2    /usr/bin/iceweasel
*+        3    /usr/bin/konqueror
          4    /usr/bin/midori
```

you only have to enter the number correspondent to your choice.

## Non standard applications

If you installed an application not present in the packages repository, it may be locatedon a local path not visible to update-alternatives. The option `--install` allows to add the local application to update alternatives. The syntax is

```bash
update-alternatives --install <bin_link_to_the_class> <class_name> <application_path> <priority>
```

For example to add a personal locally compiled version of Iceweasel located in `/usr/local/bin/`

```bash
update-alternatives --install /usr/bin/x-www-browser x-www-browser /usr/local/bin/iceweasel 50
```

An alternative can also be removed with

```bash
update-alternatives --remove x-www-browser /usr/local/bin/iceweasel
```
