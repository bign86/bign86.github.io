---
title: "Creating sockets in Bash"
date: 2013-02-24
modified: 2015-07-12
tags:
  - shell
  - linux
  - networking
  - sockets
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

Bash shell has a built-in feature that allows to open TCP/UDP sockets using a simple syntax. This is very useful when tools like `netcat` are not installed or we don't have the permission to use it.\
The syntax is

```bash
exec {file-descriptor}<>/dev/{protocol}/{host}/{port}
```

* `{file-descriptor}`: 0, 1 and 2 are reserved for `stdin`, `stout` and `stderr` respectively. At least 3 must be used. The Bash manual suggest to be careful in using descriptors above 9 since there could be conflict with descriptors used internally by the shell.
* `<>`: the file is open for both reading and writing
* `{protocol}`: tcp or udp
* `{host}`: ip address or domain name of the host
* `{port}`: logic port

Sockets can be closed using

```bash
exec {file-descriptor}<>&-
```

To send a message through the socket

```bash
echo -e -n "$MSG_OUT" >&3 
```

or

```bash
printf "$MSG_OUT" >&3 
```

To read a message from the socket

```bash
read -r -u -n $MSG_IN <&3
```

Output can be printed recursively

```bash
while read LINE <&3
do
    echo $LINE >&1
done
```

Or read entirely in one variable

```bash
OUTPUT=$(dd bs=$BYTES count=1 <&3 2> /dev/null)
```

## Example

```bash
exec 3<>/dev/tcp/127.0.0.1/1234
```

We are opening a socket for reading and writing to the 1234 port in the loopback interface.

The `/dev/tcp` and `/dev/udp` files aren't real devices but are keywords interpreted by the Bash shell. Being a "bashism" this solution is not portable even if (I've not checked this personally) seems that ksh and zsh shells have the same feature enabled.

```bash
exec 3<>/dev/tcp/www.google.com/80
echo -e "GET / HTTP/1.1\n\n" >&3
cat <&3
```

It's good practice to always close file descriptors

```bash
exec 3<&-
exec 3>&-
```

## Enable/disable net redirections

More the feature must be enabled in Bash at compile time. To enable it if you want to compile the Bash yourself include the flag

```bash
--enable-net-redirections
```

while to disable it explicitly use

```bash
--disable-net-redirections
```

Each distribution may or not have the feature enabled in their precompiled Bash. I noted that Debian is reported to NOT have this feature enabled. In my Debian box (Debian Wheezy) this feature IS enabled.

## Notes

As said before this is a built-in feature that needs to be enabled in Bash at compile time but also ksh and zsh seem to have it. System administrators might want to disable this feature since could represent a security concern. In general the use of specific tools to create sockets like `netcat` and `socat` are preferable if possible.
