---
title: "The anacron command"
date: 2019-08-30
tags:
  - linux
  - shell
  - base_command
  - scheduling
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

The tool anacron has been developed to be used together with cron. Cron is a daemon that runs jobs at scheduled moments in time. However cron assumes the system to be always on such that each job can get executed at the right time. This is of course not always true, think for example about a laptop.

Anacron is not a daemon, but a program that gets executed at every boot of the system. It reads its control file, `/etc/anacrontab`, and check for jobs to run. Anacron is not accurate in the time schedule as cron but the minimal amount of time increment is one day. The standard `/etc/anacrontab` look similar to this (there might be differences between distributions)

```bash
# /etc/anacrontab: configuration file for anacron
# See anacron(8) and anacrontab(5) for details.
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
RANDOM_DELAY=45
LOGNAME=root
HOME=/root
#period in days   delay in minutes   job-identifier   command
1    5    cron.daily        nice run-parts /etc/cron.daily
7    10    cron.weekly        nice run-parts /etc/cron.weekly
@monthly 30    cron.monthly        nice run-parts /etc/cron.monthly
```

The interesting part of the file are the last lines where are the instructions on the job to execute. Each line is made by 4 fields following the syntax

```bash
<days> <minutes> <ID> <command to execute>
```

**First column**: express the number of days that have to pass before anacron executes the relative command. For example in the second line of our file, `cron.weekly` will be executed if it was last run 7 days or more ago. Instead of a number, the keywords `@daily`, `@weekly` and `@monthly` can be used.

**Second column**: express a delay in minutes. Since anacron gets executed ones at boot, all jobs that need to be run would be run at the same time. The delay allows to spread the jobs over a longer period of time to not execute all of them simultaneously.

**Third column**: an identifier of the job.

**Fourth column**: the command to be used to execute all the scripts present in the usual cron folders. The command `run-parts` is employed like in cron.

If the system gets rebooted several times, anacron will also be executed at each reboot. This means that scheduled jobs might be run many times. To avoid re-executing the same commands again anacron keeps a record of the last execution in for of a timestamp. The timestamps are written in the three files present in the `/var/spool/anacron/` folder. The three files are conveniently called `cron.daily`, `cron.weekly`, and `cron.monthly`.

There is a second problem to solve. The jobs do not have to be executed also if cron itself took care of them recently. How can anacron determine how many days ago cron executed for the last time `cron.daily` or `cron.weekly`? There are two solutions that get adopted by different distributions.

Red Hat/Fedora solution: in the `/etc/cron.daily` `/etc/cron.weekly` and `/etc/cron.monthly` is present a file called `0anacron` that should look similar to this

```bash
#!/bin/sh
#
# anacron's cron script
#
# This script updates anacron time stamps. It is called through run-parts
# either by anacron itself or by cron.
#
# The script is called "0anacron" to assure that it will be executed
# _before_ all other scripts.
anacron -u cron.daily
```

These scripts run `anacron` with the `-u` option. With this option anacron does not execute any job but just updates its timestamps.\\
_Debian/Ubuntu solution_: for Debian and Debian based distributions the solution is contained in the `/etc/crontab` file. The relevant part of this file should look similar to

```bash
# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

Daily, weekly and monthly jobs are called using `run-parts` only if the test represented by

```bash
test -x /usr/bin/anacron
```

fails. In practice this instruction can be interpreted in the following way: check if anacron is installed on the system. If yes cron does nothing, and jobs will be executed by anacron. If it is not installed `run-parts` is executed by cron.

In the Red Hat solution it is always cron executing `run-parts` and anacron is left as a secondary tool to be used when cron missed the execution of some job. In the Debian solution it is always anacron executing `run-parts` unless it is not installed.
