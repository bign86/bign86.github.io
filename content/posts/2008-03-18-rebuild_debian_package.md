---
title: "Rebuild packages in Debian"
date: 2008-03-18
modified: 2011-09-06
tags:
  - linux
  - debian
  - package
  - system
categories:
  - debian
author_profile: true
draft: false
comments: false
---

There are many reasons why you might want to rebuild a Debian package. You can enable some options or features disabled from Debian admins, backporting a package from another suite or apply a patch. This is definitely a skill you want to learn sooner or later.

## Getting prerequisites

First we need the tools necessary to compile. You can install all them with

```bash
# apt-get install dpkg-dev build-essential fakeroot devscripts
```

A quick look to those package

* `dpkg-dev`: contains developing program used to handle source packages and to create binary `.deb` packages
* `fakeroot`: creates an environment where commands running appear to have root privileges
* `build-essential`: meta-package to be sure to have all compilers (gcc, g++) and the make package to compile
* `devscripts`: tons of scripts to simplify the life of a Debian Maintainer

## Getting the source code of the package to recompile

To rebuild we need source code of the package, that can be fetched from the repositories using APT. Make sure you have source repositories enabled in your `/etc/apt/sources.list` file, like

```bash
deb-src http://ftp.debian.org/debian testing main contrib non-free
```

You should have a source repository for every binary repository present in your `sources.list`. If some are missing add them and update the packages database.

Now we can fetch the desired package using the tool `apt-get source`. Suppose the package we want is foo

```bash
$ apt-get source foo
```

This command will download the package in the current directory. Is possible to indicate the distribution you want to download from using this syntax

```bash
$ apt-get source foo/stable
```

If you want you can create a new directory and go into it before grabbing the package.

In order to successfully compile the package we need the set of build dependency. You need the `apt-get build-dep` command that support the same syntax used to download the source. It will do all the check and will install all required packages for you

```bash
$ apt-get build-dep foo/stable
```

## Modify the sources

You know why you want to recompile foo and the time to do whatever changes you desire to source code is come! Apply patch, modify options, do what you need.\\
It is a good practice to change the version number in order to make clear that you have modified the package. This is really important if you want to distribute your modified version to other people. In the devscripts package we installed before there is a fantastic tool to do this

```bash
$ dch --local "bar"
```

Replace bar with something telling people that you have modified the package. The `--local` option will add the string to the package name.

## Rebuild the package

You are ready to create your own version of the foo package. The main tool in Debian to do the rebuild is `dpkg-buildpackage`. From the source directory enter

```bash
$ dpkg-buildpackage
```

There are some useful option you should really be aware of. Using this command you will be asked to enter your GPG key to sign the source package and the `.changes` file, twice. If you don't need this step skip it with

```bash
$ dpkg-buildpackage -us -uc
```

If you don't want to create also the source package to distribute, you can build the binary package only with the option `-b`. The `-r` option tells `dpkg` which gain-root-command it must use to gain root permissions if needed. Then the command is

```bash
$ dpkg-buildpackage -us -uc -b -rfakeroot
```

Possible gain-root-commands are `fakeroot`, `sudo`, `super`. You CAN'T use `su` because you cannot pass arguments to it. If no `-r` option is invoked, the default `fakeroot` is used.

_Link_: ["Building the package"](http://www.debian.org/doc/manuals/maint-guide/build.en.html) page in the Maintainers Guide
