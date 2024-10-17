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

The script command let the user to log all the commands and the output from the current shell session. This is useful when we want to remember operations done during the session. We can also check the work of a script we have written.
The command is very simple

```bash
$ script file_name
```

From this moment all shell operations will be logged in the `file_name` file with starting and end time. To stop the recording we need to exit the shell or with the `Ctrl+D` combination.

## Useful options:

The `-c` option followed by a command run the command in a non interactive shell. The `-a` option append the current log at the bottom of an existing file

```bash
$ script -c COMMAND -a file_name
```
