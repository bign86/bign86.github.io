---
title: "The shared memory device SHM"
date: 2008-12-26
tags:
  - linux
  - system
categories:
  - system
author_profile: true
draft: false
comments: false
---

All'interno della directory `/dev` è possibile trovare un device chiamato `/shm`. Di che cosa si tratta?\\
Questo particolare device è stato introdotto per la prima volta nel ramo 2.4.x del kernel e implementa il concetto di memoria condivisa (SHared Memory). Appare come un qualunque altro dispositivo ma in pratica si tratta di una cartella che utilizza il filesystem `tmpfs`. Qualunque file creato all'interno di questo device esiste solo in memoria condivisa ovvero nella RAM, ma nulla viene scritto su disco. Per questo motivo al riavvio della macchina il device è automaticamente svuotato. Con questo sistema un programma può riempire una porzione di memoria alla quale un altro processo può accedere se gli è permesso direttamente in RAM. Il risultato è un sensibile aumento di prestazioni.

Quindi ad esempio se spostiamo un qualsiasi file all'interno

```bash
$ cp -f ~/file /dev/shm
```

abbiamo semplicemente spostato il file in RAM.
Di default la dimensione di questo filesystem è impostata su metà della RAM disponibile al sistema ma può ingrandirsi quanto vogliamo. Per farlo è sufficiente rimontare il device `shm` come si farebbe per un qualunque altro dispositivo semplicemente utilizzando il comando mount

```bash
# mount -o remount,size=2G /dev/shm
```

Questo device tuttavia è stato introdotto per compatibilità con la memoria condivisa POSIX e non è del tutto consigliabile farne un uso eccessivo. Molto più utile è utilizzare direttamente il filesystem `tmpfs`.\\
Ad esempio potremmo spostare in RAM file salvati su dispositivi lenti (CD per esempio) e sui quali facciamo tantissime operazioni di accesso in read-only. Volendo creare con il filesystem `tmpfs` una directory persistente montata in automatico ad ogni avvio della macchina possiamo utilizzare sempre il comando `mount`

```bash
$ mkdir -p ~/cache
$ mount -t tmpfs -o size=1G, mode=700 tmpfs ~/cache
```

Un altro campo di utilizzo è sicuramente la creazione di una cache su un server che ospita un sito internet. L'aumento di prestazioni può fare tantissima differenza anche su macchine virtuali come VMware.

Ad esempio per creare la cache per il nostro sito possiamo semplicemente utilizzare i seguenti comandi

```bash
# cd /var/www/sito/
# mkdir -p ./tmp
# mount -t tmpfs -o size=5G,nr_inodes=5k,mode=744 tmpfs /var/www/sito/tmp
```

Se abbiamo una macchina dotata di più di 2G di RAM con molte macchine virtuali attive questo spesso fa una enorme differenza in termini di prestazioni. Per rendere queste modifiche disponibili ad igni riavvio della macchina possiamo impostarle all'interno di `/etc/fstab` aggiungendo questa riga

```bash
tmpfs /var/www/sito/tmp tmpfs size=5G,nr_inodes=5k,mode=744 0 0
```
