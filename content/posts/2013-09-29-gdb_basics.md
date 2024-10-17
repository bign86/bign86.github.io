---
title: "Gdb debugging basics"
date: 2013-09-29
tags:
  - c
  - gdb
  - debugging
categories:
  - programming
author_profile: true
draft: false
comments: false
---

Here the basic commands everyone should be aware of to be able to use gdb in a quick reference. Remember to always compile your code with debugging symbols, `-g` option in gcc or g++. It's usually good practice to avoid optimizations when we want to debug the code.

### Content

- [Start \& stop](#start--stop)
- [Show me the source code](#show-me-the-source-code)
- [Breakpoints and watchpoints](#breakpoints-and-watchpoints)
- [Working with variables](#working-with-variables)
- [Backtrace](#backtrace)
- [Logging](#logging)

## Start & stop

1. **Start the debugger**: to start simply call it followed by the project we are going to debug. An empty prompt will be displayed but our executable will not be started yet

   ```bash
   $ dbg my_exec
   (gdb)
   ```

2. **Run the project**: start running the executable using `run`. Many commands have a short version made by the first character in the command. In this case `r`

   ```bash
   (gdb) run               # simple run
   (gdb) r par1 par2       # pass parameters to run
   (gdb) r < input_file    # read in a file with the parameters
   ```

3. **Quit the debugger**: use the `quit` command

   ```bash
   (gdb) quit     # quit
   (gdb) q        # quit shortcut
   ```

## Show me the source code

1. **List a piece of source**: reached a breakpoint I want to see the source code. The list command shows us 10 lines around the breakpoint

   ```bash
   (gdb) list     # shows 10 lines around the breackpoint line
   (gdb) l 20     # shows line 20
   (gdb) l foo    # shows function 'foo'
   ```

2. **Execute one line**: execute the current line stopping at the next

   ```bash
   (gdb) next     # go to next line
   (gdb) n        # go to next line shortcut
   ```

3. **Execute one instruction**: execute a single instruction (not a line, stepping into a function for example is an instruction)

   ```bash
   (gdb) step     # step over
   (gdb) s        # step over shortcut
   ```

4. **Execute to the end of the current function**: execute what is left of the current function, then step out and stop

   ```bash
   (gdb) finish
   ```

## Breakpoints and watchpoints

1. **Set a breakpoint**: gdb can set breakpoints at lines and functions with break

   ```bash
   (gdb) break 231    # breakpoint at line 231
   (gdb) b foo        # breakpoint at 'foo' function
   ```

2. **Set a conditional breakpoint**: we can stop the execution of the code when a condition changes using watch. These breakpoints are called watchpoints

   ```bash
   (gdb) watch <condition>     # set a conditional breakpoint on <condition>
   (gdb) watch flag == true    # example: stop execition when flag != true
   ```

3. **Resume the execution**: resume execution after a breakpoint

   ```bash
   (gdb) continue     # resume execution
   (gdb) c            # resume execution shortcut
   ```

4. **Remove a breakpoint**: you need the number of the breakpoint to be removed

   ```bash
   (gdb) delete NUM
   (gdb) d NUM
   ```

5. **Which breakpoints do I have set?**: to keep track of the breakpoints use

   ```bash
   (gdb) info breakpoints
   ```

## Working with variables

1. **Print a variable**: to see the content of a variable

   ```bash
   (gdb) print var     # print the value of the variable <var>
   (gdb) p var         # print the value of the variable <var> shortcut
   ```

2. **Set a variable**: set a variable to a value or using another variable

   ```bash
   (gdb) set var = 3.2       # set <var> equal to 3.2
   (gdb) set var1 = var2     # set <var1> using <var2>
   ```

3. **Keep me informed about a variable**: to always have the value of a variable printed when the execution stops use

   ```bash
   (gdb) display var    # display the value of the variable <var>
   (gdb) undisplay var  # stop showing the value of the variable <var>
   ```

4. **Which variables are in the stack?**: will give all the variables currently in the stack with their values

   ```bash
   (gdb) info locals
   ```

## Backtrace

1. **Give me the backtrace**: print the backtrace using `backtrace` or `bt`

   ```bash
   (gdb) backtrace
   (gdb) bt          # shortcut
   ```

2. **Move the stack**: to move in the backtrace use

   ```bash
   (gdb) up
   (gdb) down
   ```

## Logging

1. **I want to save my session**: logging is disabled by default by can be enabled using

   ```bash
   (gdb) set logging on
   ```
