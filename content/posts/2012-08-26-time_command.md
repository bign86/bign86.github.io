---
title: The time command
date: 2012-08-26
tags:
   - linux
   - shell
   - base_command
categories:
   - base_commands
author_profile: true
draft: false
comments: false
slug: time-command
---

`time` is a very basic command whose purpouse is to give the user the execution time of another command. However, there are several output formats thefore a bit of extra attention on portability is required when writing scripts.

## POSIX format time

The POSIX standard defines three kinds of time

|        |                                                  |
|:------:| ------------------------------------------------ |
| _real_ | Real total time taken by the process to complete |
| _user_ | Time the process spent in user space             |
| _sys_  | Time the process spent in kernel space           |

The time is expressed in seconds up to at least one decimal number. The exact precision displayed depends on the OS.

## Bash built-in time

Bash incorporates its own version of time. The usage is very easy

```bash
time <command>
```

In the output we will see the time taken by `<command>` to complete with the format

```bash
time cp large_file ..
real    0m2.557s
user    0m0.004s
sys    0m0.380s
```

expressed in minutes and seconds up to the millisecond. The `-p` option formats the output following the POSIX standard.

```bash
time -p cp large_file ..
real 4.56
user 0.00
sys 0.36
```

It is not possible to modify any further the format from the command line. However, time reads from the `TIMEFORMAT` environment variable how to display the output. The options are

|   Option   |                                            |
|:----------:| ------------------------------------------ |
| `%[p][l]R` | Elapsed time in seconds                    |
| `%[p][l]U` | Number of CPU seconds spent in user mode   |
| `%[p][l]S` | Number of CPU seconds spent in system mode |
| `%P`       | CPU percentage                             |

where

| `p` | Precision digits from 0 up to 3. Default is 3 |
| `l` | Longer format `MMmSS.FFs`                     |

The Bash built-in version of time is the one called when no options are passed to the command.

## Formatting examples

Examples of valid format strings for the `TIMEFORMAT` variable.

1. POSIX standard output

   ```bash
   export TIMEFORMAT=$'real %2R\nuser %2U\nsys %2S'
   time cp large_file ..
   real 2.64
   user 0.00
   sys 0.31
   ```

2. Standard time output

   ```bash
   export TIMEFORMAT=$'\nreal\t%3lR\nuser\t%3lU\nsys\t%3lS'
   /usr/bin/time cp large_file ..
   real    0m2.082s
   user    0m0.000s
   sys    0m0.256s
   ```

3. Only seconds no decimal digits

   ```bash
   export TIMEFORMAT=$'%0E'
   time cp large_file ..
   3
   ```

4. BSD `/usr/bin/time` format

   ```bash
   export TIMEFORMAT=$'\t%1R real\t%1U user\t%1S sys'
   time cp large_file ..
       2.0 real    0.0 user    0.2 sys
   ```

5. System-V `/usr/bin/time` format

   ```bash
   export TIMEFORMAT=$'\nreal\t%1R\nuser\t%1U\nsys\t%1S'
   time cp large_file ..
   real    2.1
   user    0.0
   sys    0.2
   ```

6. ksh time format

   ```bash
   export TIMEFORMAT=$'\nreal\t%2lR\nuser\t%2lU\nsys\t%2lS'
   time cp large_file ..
   real    0m2.00s
   user    0m0.00s
   sys    0m0.24s
   ```

## GNU time

The implementation of time provided by GNU is more flexible and allows to specify the output format during the calling or using the variable `TIME`.
This version of time is usually in the `/usr/bin/` folder. To call it

```bash
/usr/bin/time cp large_file ..
0.00user 0.26system 0:02.06elapsed 12%CPU (0text+0data 964max)k
0inputs+529232outputs (0major+302minor)pagefaults 0swaps
```

The standard output gives many informations

| Snippet                       | Description                                                                    |
|:-----------------------------:| ------------------------------------------------------------------------------ |
| `0.00user`                    | time in seconds spent in user space                                            |
| `0.26system`                  | time in seconds spent in kernel space                                          |
| `0:02.06elapsed`              | total time elapsed in the format `hours:minutes.seconds`                       |
| `12%CPU`                      | percentage of CPU dedicated to the process in the elapsed time                 |
| `(0text+0data 964max)k`       | infos regarding the memory used by the process                                 |
| `0inputs+529232outputs`       | counting of input/output operations in the filesystem performed by the process |
| `(0major+302minor)pagefaults` | number of major/minor pagefault errors                                         |
| `0swaps`                      | number of time the swap memory has been used                                   |

## Formatting the output

### POSIX output

To have the output following the POSIX standard you can use the option `-p` (like in the built-in one) or `--portability`

```bash
/usr/bin/time --portability cp large_file ..
real 4.56
user 0.00
sys 0.36
```

### Specify the format at call time

The format can be passed at call using the option `-f <format>` or `--format="format"` where `format` is a string enclosed in double quotes that list the fields to print. This option works using a `printf` style string.

```bash
/usr/bin/time -f "user time: %U\nmemory: %M" cp large_file ..
user time: 0.00
memory: 968
```

### Using the variable `TIME`

By default, `time` reads the format from the variable `TIME` if defined. The content of `TIME` must be the same we would put after the option `-f`. The previous result could have been obtained using the following

```bash
export TIME="user time: %U\nmemory: %M"
/usr/bin/time cp large_file ..
user time: 0.00
memory: 968
```

### Formatting options

The format string follows the `printf` style, the usual escape characters `\n`, `\t`, `\\` are recognized. The literal `%` symbol must be escaped by writing it twice: `%%`.
Printable fields are organized in three sections. Here a list of the most useful (?) ones.

| Type   | Format                 | Description                                                                                                                         |
|:------:|:----------------------:| ----------------------------------------------------------------------------------------------------------------------------------- |
| Time   | `%E`, `%e`, `%S`, `%U` | Real time elapsed `hours:minutes.seconds`, Real time elapsed in seconds, Seconds spent in kernel space, Seconds spent in user space |
| Memory | `%M`, `%F`, `%R`, `%W` | Memory used by the process in Kbyte. Major pagefaults, Minor pagefaults, Number of swap operations                                  |
| I/O    | `%I`, `%O`, `%k`       | Input operations, output operations, number of signals sent to the process                                                          |

To have the complete list of all the conversions and fields check the man page

```bash
man time
```

### Exmaples of Formatting

Examples of format strings to put in the `TIME` variable or after the `-f` option to obtain the desired output.

1. POSIX standard output

   ```bash
   real %e
   user %U
   sys %S
   ```
   
   output
   
   ```bash
   /usr/bin/time cp drivers.run ..
   real 2.66
   user 0.00
   sys 0.30
   ```

2. Standard GNU time output

   ```bash
   %Uuser %Ssystem %Eelapsed %PCPU (%Xtext+%Ddata %Mmax)k\n%Iinputs+%Ooutputs (%Fmajor+%Rminor)pagefaults %Wswaps
   ```
   
   output
   
   ```bash
   /usr/bin/time cp drivers.run ..
   0.00user 0.26system 0:02.13elapsed 12%CPU (0text+0data 964max)k
   40inputs+529232outputs (0major+304minor)pagefaults 0swaps
   ```

3. Only seconds

   ```bash
   %e
   ```

   output
   
   ```bash
   /usr/bin/time cp drivers.run ..
   2.66
   ```

Credits go to:\
[Unix guide for very good examples](http://www.unixguide.net/unix/bash/G4.shtml)\
[Official Bash manual](http://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)
