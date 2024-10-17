---
title: "The date command"
date: 2012-10-28
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

The date command is the basic command to obtain the current time and date on a Unix system. The GNU version of the command is the one widely installed on every Unix system. date is a very flexible command that allows also for simple date arithmetics. It can be used also to calculate elapsed time in the execution of a script or a function.

## The GNU date is invoked with the standard command

```bash
$ date
```

The default output format depend on the locale. The standard US is

```bash
$ <DOW> <Month> <DOM> <hh.mm.ss> <TZ> <Year>
```

where

| Label | Meaning                                            |
|:-----:| -------------------------------------------------- |
| DOW   | day of the week, ex. Monday                        |
| DOM   | day of the month, ex. 01                           |
| TZ    | time zone, ex. CEST (Central European Summer Time) |

For a complete list of the available time zones check the List of zones of Wikipedia.
With date we can set the system time using the option `-s` or `--set` followed by a string representing the new date and time

```bash
$ date --set='STRING'
```

`STRING` must be in the standard format. If you want to use a different format, you have to explicitly tell to date the used format (see the "Format the output" section for format strings). For example to set the time in the format `hh:mm:ss`

```bash
$ date +'%T' --set='14:43:23'
```

Remember that in order to change the system time you need to have root permissions.

## Compute a date in human readable form

What is for me the coolest function of date is the option `-d` or `--date` that allows to ask for a date using a human readable string as query

```bash
$ date --date='STRING'
```

date recognize correctly several common words we can use to express the desired time. A non-comprehensive list is

now, second(s), minute(s), fortnight(s) hour(s), day(s), week(s), month(s), year(s),\\
this, next, last, ago, today, tomorrow, yesterday

plus numbers, names of days of the week and months both long and short version (both "Mon" and "Monday" are recognized). Also the minus sign `-` is used to go backwards in time. All these keywords can be combined to obtain the desired result.
Few examples

```bash
$ date --date='now'
Sun Oct 28 14:48:00 CET 2012

$ date --date='yesterday'
Sat Oct 27 15:48:00 CEST 2012

$ date --date='tomorrow'
Mon Oct 29 14:48:00 CET 2012

$ date --date='this Tuesday'
Tue Oct 30 00:00:00 CET 2012

$ date --date='this Tue'
Tue Oct 30 00:00:00 CET 2012

$ date --date='next Wed'
Wed Oct 31 00:00:00 CET 2012

$ date --date='last Wed'
Wed Oct 24 00:00:00 CEST 2012

$ date --date='4 hours'
Sun Oct 28 18:48:00 CET 2012

$ date --date='4 hours 20 minutes ago'
Sun Oct 28 18:28:00 CET 2012

$ date --date='4 hours ago 20 minutes ago'
Sun Oct 28 10:28:00 CET 2012

$ date --date='last year'
Fri Oct 28 15:48:00 CEST 2011

$ date --date='1 day 1 hour 10 minutes 10 seconds'
Mon Oct 29 15:58:10 CET 2012
```

The string may contain indications of "am" or "pm" and be even more complex. From the info page of date (`$ info date`). For example, `--date="2004-02-27 14:19:13.489392193 +0530"` specifies the instant of time that is 489,392,193 nanoseconds after February 27, 2004 at 2:19:13 PM in a time zone that is 5 hours and 30 minutes east of UTC.

## Format the output

The output format of date can be modified in the way we prefer. The simplest format is the standard UTC. There is a quick option for this format: `-u` or `--utc` or `--universal`

```bash
$ date -u
Sat Oct 27 21:51:37 UTC 2012
```

Please note that the standard output format depend on the locale of the machine.

More complex formats must be defined using a plus `+` followed by a string containing the specifiers. These are many, refer to the official pages for the complete list. Here few examples

```bash
$ date +'%m-%d-%y'
10-28-12

$ date +'%m-%d-%Y'
10-28-2012

$ date +'%D'
10/28/12

$ date +'%T'
15:09:25

$ date +'%r'
03:09:25 PM

$ date +'%H-%M'
15-09
```

Another very important way of representing dates is the Epoch. The Epoch is an integer number increased by 1 each second that has been defined to be 0 at 00:00:00 01-01-1970. So the Epoch time represent the seconds elapsed since the new year 1970. To output in Epoch format use `%s`

```bash
$ date +'%s'
```

The Epoch format is particularly useful in script since is very simple to add and subtract dates. Then the Epoch can be converted back to human readable format using the `--date` option

```bash
$ date --date='@123456789'
Thu Nov 29 22:33:09 CET 1973
```

## Convert a date between formats

The `--date` and `+'STRING'` options can be combined to convert a date from one format to another. For example

```bash
$ date --date='2012-10-23 13:32:04' +'%D %r'
10/23/12 01:32:04 PM
```

## More informations

For more informations in particular about format specifiers refer to man and info pages

```bash
$ man date
$ info date
```
