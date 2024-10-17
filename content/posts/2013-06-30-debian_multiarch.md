---
title: "Multiarch in Debian"
date: 2013-06-30
modified: 2015-11-04
tags:
  - linux
  - debian
  - multiarch
  - system
categories:
  - debian
author_profile: true
draft: false
comments: false
---

A Debian system can now deal with more than one architecture at the same time. The new multiarch feature has been introduced for a couple of very good reasons:

* _Mixing architectures_: we need to mix 32- and 64-bits applications and libraries.
* _Mixing instruction sets_: processors can emulate instruction sets from other processors. For example i386 processors are able to emulate ia64.

To use the new feature we need dpkg >= 1.16.2 and apt >= 0.8.13 (better >= 0.9.4).

## Set up multiarch features

First we want to know our native architecture

```bash
$ dpkg --print-architecture
```

Then we want to know which foreign architectures are available

```bash
$ dpkg --print-foreign-architectures
```

With superuser privileges we can now add a new architecture

```bash
# dpkg --add-architecture <arch>
```

where `<arch>` must be one of the supported architectures.\\
To remove an enabled architecture instead

```bash
# dpkg --remove-architecture <arch>
```

## Work with packages

Packages can be installed with `dpkg` or APT. In the latter the architecture has to be explicitly indicated after semicolon

```bash
# dpkg -i package_<arch>.deb
# apt-get install package:<arch>
```

Package removal under multiarch is architecture selective so that the same package can be removed for one architecture but not for another

```bash
# dpkg -r package:<arch>
# apt-get remove package:<arch>
```

## Developers notes

### Cross-building

Multiarch supports cross-building. We can install all the required dependencies for a cross-build using

```bash
# apt-get build-dep -a <arch> package
```

Apt will build all the required dependencies in the correct architecture. This works only if all the packages are correctly marked with the multiarch tags. It can still be not the case for all of them. A possible workaround is to manually install the package in the correct architecture using the apt command seen before. (By late 2015 all packages are correctly marked).

### Libraries headers

Under multiarch architecture-dependent headers can be put in several different locations. Notably

* `/usr/include/<arch>/*.h`: the default location for most headers. Correct if you are meant to include `<foo.h>` without `pkg-config`.
* `/usr/include/<arch>/<package>/*.h`: useful if you have multiple co-installable versions of the same library. Correct if you are meant to include `<foo/foo.h>` without `pkg-config`, or `<foo.h>` with `-I.../<packagename>` provided by `pkg-config`.
* `/usr/lib/<arch>/<package>/include/*.h`: for private header files. Is something of a workaround for Debian-style multiarch being relatively new.
