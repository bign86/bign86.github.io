---
title: Signals in Bash
date: 2012-02-26
modified: 2015-07-12
tags:
  - shell
  - linux
  - bash
  - system
categories:
  - base_commands
author_profile: true
draft: false
comments: false
slug: signals-bash
---

Signals are one of the most important parts of the shells available on Unix. Although dealing with signals can be avoided writing small scripts, they became very important to introduce a new level of flexibility and to tackle situations where something is going wrong. Since are widely used in the shell any administrator should understand them very well. First I talk about the various signals available and then I present the trap function used to handle them in scripts.

Signals are integer numbers cast from a process, the operative system or a user and directed to a running process. The meaning of each signal is built-in in the shell and is something that any process understand. The process receiver of the signal must respond to the signal properly and when this doesn't happen the shell can take default actions, usually killing the process.
A detailed list of the signal available can be read using

```bash
kill -l
 1) SIGHUP          2) SIGINT          3) SIGQUIT         4) SIGILL          5) SIGTRAP
 2) SIGABRT         7) SIGBUS          8) SIGFPE          9) SIGKILL        10) SIGUSR1
1)  SIGSEGV        12) SIGUSR2        13) SIGPIPE        14) SIGALRM        15) SIGTERM
2)  SIGSTKFLT      17) SIGCHLD        18) SIGCONT        19) SIGSTOP        20) SIGTSTP
3)  SIGTTIN        22) SIGTTOU        23) SIGURG         24) SIGXCPU        25) SIGXFSZ
4)  SIGVTALRM      27) SIGPROF        28) SIGWINCH       29) SIGIO          30) SIGPWR
5)  SIGSYS         34) SIGRTMIN       35) SIGRTMIN+1     36) SIGRTMIN+2     37) SIGRTMIN+3
6)  SIGRTMIN+4     39) SIGRTMIN+5     40) SIGRTMIN+6     41) SIGRTMIN+7     42) SIGRTMIN+8
7)  SIGRTMIN+9     44) SIGRTMIN+10    45) SIGRTMIN+11    46) SIGRTMIN+12    47) SIGRTMIN+13
8)  SIGRTMIN+14    49) SIGRTMIN+15    50) SIGRTMAX-14    51) SIGRTMAX-13    52) SIGRTMAX-12
9)  SIGRTMAX-11    54) SIGRTMAX-10    55) SIGRTMAX-9     56) SIGRTMAX-8     57) SIGRTMAX-7
10) SIGRTMAX-6     59) SIGRTMAX-5     60) SIGRTMAX-4     61) SIGRTMAX-3     62) SIGRTMAX-2
11) SIGRTMAX-1     64) SIGRTMAX   
```

The list quite is long only but some of them are really widely used. These are the fundamental ones

| Code | Signal | Meaning                   | Shortcut |
|:----:|:------:| ------------------------- | -------- |
| 1    | `HUP`  | hang up                   |          |
| 2    | `INT`  | interrupt from keyboard   | Ctrl+C   |
| 3    | `QUIT` | clean quit                |          |
| 5    | `TRAP` | trap for trace breakpoint |          |
| 6    | `ABRT` | abort (dirty  quit)       |          |
| 9    | `KILL` | kill                      |          |
| 15   | `TERM` | terminate                 |          |
| 18   | `CONT` | continue a paused process |          |
| 19   | `STOP` | pause the process         |          |
| 20   | `TSTP` | pause a running process   | Ctrl+Z   |

* `SIGHUP`: if the receiver is a daemon force it to read again its configuration files. If the receiver is a shell, the shell send a `SIGHUP` to all processes and exit.
* `SIGINT`: sent by the terminal requesting the process to stop. It is the standard kill signal from keyboard that can be ignored by the target process. If a process doesn't quit with this signal, use `SIGKILL` instead.
* `SIGQUIT`: similar to `SIGINT` but the process can create a core dump. If the target is ann interactive terminal it ignores `SIGQUIT`.
* `SIGTRAP`: used for debugging. The debugger send this signal when some condition is met.
* `SIGABRT`: dirty termination of the process. Usually used by processess on themself when something goes wrong.
* `SIGKILL`: terminate the process at kernel level. Cannot be trapped, blocked or ignored. A process or terminal killed in this way have no chance to cleanly exit since is the kernel itself to terminate the execution.
* `SIGTERM`: terminate the execution. Is the standard signal sent by the kill command, and since it can be trapped, allow the script to execute code to terminate its own execution cleanly. If the receiver terminal is interactive it ignores `SIGTERM` so that the command

```bash
kill 0
```

will not kill the terminal.

* `SIGCONT`: ask a paused process to continue its execution. Process can be paused by `SIGTSTP` or `SIGSTOP`. This signal cannot be trapped, blocked or ignored.
* `SIGSTOP`: pause the execution of the process. This signal cannot be ignored.
* `SIGT STP`: ask the processes to pause cleanly without terminating the process. This signal can be ignored.

## Use signals: trap function

To handle your signals inside scripts exists the trap function. This function catch signals and execute a list of commands depending on the signal cast.

The syntax is very simple

```bash
trap <CODE> <SIGNALS>
```

* `<CODE>`: is a command or a list of commands that you want to execute when a specific signal is caught.
* `<SIGNALS>`: is one or more signals caught by that particular trap.

You can specify one `trap()` command for each signal to execute different task for different signals.

Signals in `<SIGNALS>` can be chosen between the standard signals or between the following:

* `DEBUG`: if the signal is `DEBUG` the list of commands is executed every time the shell does something.
* `0, EXIT`: if the signal is `EXIT` or its equivalent 0 the list of commands is executed every time the shell exit.
* `ERR`: if the signal is `ERR` the list of command is executed everytime a command return with a non-zero return status that is commonly an error. This is not applied if the non-zero value comes out from a if statement, a loop or logical operators (`&&`, `||`).

## Examples: using the trap function

This script will continue to loop forever printing "Hello!".

```bash
#!/bin/bash
# Do nothing when you catch the SIGINT signal
 
trap  SIGINT
 
while true
do
    echo "Hello!"
done
```

If you try to kill it with a `SIGINT` (Ctrl+C) it will do anything continuing the loop. You have to use the `SIGKILL` signal to get rid of this process.

This other script is an example of how to capture few signals and execute a functions when one signal is caught.

```bash
#!/bin/bash
# Script that print the current date in a loop. It can be blocked only
# using a SIGINT, SIGQUIT or SIGTERM signals
 
# Execute the exit_code function when catching signals num 2 3 15
trap exit_code() SIGINT SIGQUIT SIGTERM
 
exit_code()
# Function that exit the script with code 1
{
    echo "Signal caught! Exiting..."
    exit 1
}
 
while true
do
    echo $(date)
done
 
exit 0
```
