---
title: "Search for open ports in Linux"
date: 2009-09-13
modified: 2011-10-16
tags:
  - linux
  - system
  - networking
categories:
  - system
author_profile: true
draft: false
comments: false
---

Which ports are open on our Linux box? Here three different approaches, `netstat`, `nmap` and `lsof`.

## Netstat

To list all the open ports on the local machine

```bash
# netstat -anp
```

The output is divided in two main sections. The first is labeled by

`Active Internet connections (servers and established)`

while the second is labeled by

`Active UNIX domain sockets (servers and established)`

We are interested in the first one. Where the State is `LISTEN` the port is open, where is `ESTABLISHED` there is traffic on the port.

## Nmap

Nmap is a powerful program and listing open ports is a very simple task with it. To do a `SYN` scan on your own machine enter

```bash
# nmap -sS -O 127.0.0.1
```

The output is extremely clear. It lists the ports, the current state and the service related on each line. It should be similar to this one

```bash
Starting Nmap 5.21 ( http://nmap.org ) at 2011-09-21 13:21 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000012s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
25/tcp  open  smtp
111/tcp open  rpcbind
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
631/tcp open  ipp

Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds
```

## Lsof

The `lsof` command is to list all open files on the machine, but you may want to use it to list open ports in this way

```bash
# lsof -i -n
```

The output is something like this

```bash
COMMAND    PID        USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
rpcbind    908        root    6u  IPv4    4718      0t0  UDP *:sunrpc 
rpcbind    908        root    7u  IPv4    4721      0t0  UDP *:659 
rpcbind    908        root    8u  IPv4    4722      0t0  TCP *:sunrpc (LISTEN)
rpcbind    908        root    9u  IPv6    4725      0t0  UDP *:sunrpc 
rpcbind    908        root   10u  IPv6    4728      0t0  UDP *:659 
rpcbind    908        root   11u  IPv6    4729      0t0  TCP *:sunrpc (LISTEN)
rpc.statd  923       statd    4u  IPv4    4771      0t0  UDP *:675 
rpc.statd  923       statd    7u  IPv4    4778      0t0  UDP *:54476 
rpc.statd  923       statd    8u  IPv4    4782      0t0  TCP *:42910 (LISTEN)
rpc.statd  923       statd    9u  IPv6    4786      0t0  UDP *:48953 
rpc.statd  923       statd   10u  IPv6    4790      0t0  TCP *:56599 (LISTEN)
avahi-dae 1176       avahi   13u  IPv4    5149      0t0  UDP *:mdns 
avahi-dae 1176       avahi   14u  IPv6    5150      0t0  UDP *:mdns 
avahi-dae 1176       avahi   15u  IPv4    5151      0t0  UDP *:60910 
avahi-dae 1176       avahi   16u  IPv6    5152      0t0  UDP *:34156 
... (output truncated)
```

_Final note_: list the active ports does not mean list the vulnerable ports. If a firewall is running protecting some of the ports, is possible that it will not block a scan coming from the local machine. So to be sure that an active port is vulnerable you should always do a scan from outside the local machine and even the intranet.
