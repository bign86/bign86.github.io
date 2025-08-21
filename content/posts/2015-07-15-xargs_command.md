---
title: "The xargs command"
date: 2015-07-15
modified: 2016-03-06
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

Xargs is used to build argument lists from the standard input and to execute commands on each element of the list. It turns out to be very useful, in particular with commands as `rm`, `grep`, `find`. In general it is worth considering the use of `xargs` any time we have a command that builds a list of objects (like file names found with ls or find) that needs to be piped as input for another command.\
The behavior of `xargs` can be mymicked by the single backquotes (``) as we will see in examples. However, the backquotes method has few limits, as the size of the argument list that can be built or the difficulty in handling spaces and other non-standard characters.

### Standard use

The syntax of `xargs` can be syntetically written

```bash
<command_1> | xargs <command_2>
```

`xargs` pass the list of objects created by `<command_1>` as a whole to `<command_2>`. A few examples to explain. We can print the names of the files in the current directory with `ls`

```bash
ls
file1 file2 file3
```

The standard command executed by `xargs` is `echo`

```bash
ls | xargs
file1 file2 file3
```

White spaces and newlines are delimiters of the objects in the list. We could decide to print the file names in the current directory preceded by the string "Found:". We use `ls` and `xargs`

```bash
ls | xargs echo Found:
Found: file1 file2 file3
```

The list of names is used as a whole object and passed to `echo`. The same result could have been obtained using the backquotes as in

```bash
echo Found: `ls`
Found: file1 file2 file3
Argument list too long
```

Sometimes when executing a command the error "Argument list too long" appears because too many arguments are being submitted at ones to the command. A typical example is

```bash
rm -f *
```

when the number of files in the current directory is huge. The problem is easily solved with `xargs` and find:

```bash
find . -maxdepth 1 -type f -print0 | xargs -0 rm -f
```

### Break the input and delimiters

But `xargs` offers more options than the backquote. We can break the list into single arguments and call the argument command for each of them. For example, we may want to append the string "Found:" at the beginning of each file name. To obtain this we need to use the option `-I`

```bash
$ ls | xargs -r -I {} echo Found: {}
Found: file1
Found: file2
Found: file3
```

The `{}` is a standard name for list items. The same could have been written using `-i`

```bash
ls | xargs -i echo Found: {}
Found: file1
Found: file2
Found: fil
e3
```

Note that the label of the items (in this case `{}`) can be modified to be more expressive, like

```bash
ls | xargs -I filename echo Found: filename
Found: file1
Found: file2
Found: file3
```

A specific delimiter can be enforced using the option `-d <delim>`, for example

```bash
xargs -a file -d\n -I {} echo Output: {}
```

The elements of the input list can be passed in a maximum number per line using the option `-n <num>`.

```bash
ls | xargs
file1 file2 file3 file4 file5 file6
ls | xargs -n 2
file1 file2
file3 file4
file5 file6
```

### No action if argument list is empty

In a command like

```bash
find <something> | xargs <some action>
```

`xargs` will execute `<some action>` even if the list of arguments provided by find is completely empty. This is almost always not what we want. To avoid it we must use the `-r` option

```bash
find . -type f -print0 | xargs -0 -r rm
```

### White spaces

White spaces are delimiters for `xargs`, causing problems in handling filenames. To achieve the correct behavior the option `-0` exist that forces `xargs` to consider only newlines as delimiters.

```bash
ls
file1.txt file2.txt file3.txt file 4.txt
find . -name "*.txt" | xargs rm -f
ls
file 4.txt
```

This happens since the strings "file" and "4.txt" have been passed to `rm` separately. To correct the behavior use the following

```bash
find . -name "*.txt" -print0 | xargs -0 rm -f
```

A similar example is a safe command to search for files and echos their names

```bash
find . -maxdepth 1 -type f -print0 | xargs -0 -I {} echo Found: {}
Found: file1
Found: file2
Found: file3
Found: file 4    # note the white space
```

### Read from a file

The `-a` option allows to read from a file instead of the standard input. If we have a file "foo" containing

```bash
string1
string2
string3
```

we can do

```bash
$ xargs -a file -n 1 -I {} echo Output: {}
Output: string1
Output: string2
Output: string3
```

### Input and output redirection

Input and output can be redirected using the usual `< >` symbols. For example print the output of a list of files using `cat`. The list is in the _list.txt_ file

```bash
xargs cat < list.txt
```

Now we redirect the previous output to a new file. This can be done to concatenate files

```bash
xargs cat < list.txt > new_file.txt
```

### Note about speed with find

The `-exec` option of find can be used in place of `xargs` with similar results. For example, we can find all the files in the current directory and remove all the writing permissions in the following way

```bash
find . -type f -exec chmod -w {} \;
```

Alternatively `xargs` can be used

```bash
find . -type f | xargs chmod -w
```

These two alternatives give equivalent results, but the second one is faster. The reason is that in the first case `chmod` is called for each single file found, while in the second case is called only once for the whole list of files. There is a third alternative

```bash
find . -type f | xargs -n1 -I {} chmod -w {} 
```

In this last case `chmod` is again called once for each element in the list, so there shouldn't be much difference comparing with the first option.

## Examples

- Print a list of found files

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0
   ```

- Print a list preceded by a string

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 echo "String"
   ```

- Print a list where each single element is preceded by "String"

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 -n 1 -I {} echo "String" {}
   ```

- Check which file have a string inside and print filename and line

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 grep string
   ```

- Check which file have a string inside and print only the line

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 -n 1 -I {} grep string {}
   ```

- Print first line of each file

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 head -n 1
   ```

- Print last line of each file

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 tail -n 1
   ```

- Search & remove

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 rm
   ```

- Search & remove one by one

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 -n 1 -I {} rm -p {}
   ```

- Search for mp3 files and remove. They may have white spaces

   ```bash
   find . -maxdepth 1 -name '*.mp3' -print0 | xargs -0 -r rm
   ```

- Count lines in files

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 wc -l
   ```

- Move files

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 -I {} mv {} dest/folder/
   ```

- Move files between original directory to backup directory appending `.bak` to the file name

   ```bash
   find src_dir/ -maxdepth 1 -type f -print0 | xargs -0 -n 1 -I {} mv src_dir/{} dest_dir/{}.bak
   ```

- Copy files

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 -n 1 -I {} cp {} dest/folder/
   ```

- Tar files

   ```bash
   find . -maxdepth 1 -type f -print0 | xargs -0 tar czf archive.tar.gz
   ```

- Fetch files from internet given the urls in a list

   ```bash
   xargs -a urls.txt wget -c
   ```
