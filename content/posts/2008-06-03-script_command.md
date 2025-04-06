---
title: "The script command"
date: 2008-05-26
modified: 2011-08-16
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

The script command lets the user create a log of executed commands and output from the current shell session. This may be useful also to verify a script's correctness of execution.\
The command is simple

```bash
script file_name
```

From this moment all shell operations will be logged in `file_name` with starting and end time. To stop the recording we need to exit the shell or use the `Ctrl+D` keys combination.

## Useful options

The `-c` option followed by a command runs the command in a non-interactive shell. The `-a` option appends the current log at the bottom of an existing file

```bash
script -c COMMAND -a file_name
```
