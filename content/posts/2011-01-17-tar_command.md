---
title: "The tar command"
date: 2011-01-17
modified: 2011-10-23
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

The tar program is for create archives of files and was developed to transfer the files to a tape archive (from here the name "Tape ARchiver"). The modern tar can also use external libraries with compression algorithms to compress/uncompress an archive. This howto is made mainly using practical examples, after a quick introduction to tar.

For a quick introduction on how make backups using tar read "Backups with tar".

## Basic usage

### Create a new archive and extract from it

To create a new archive use the command tar with the `-c` option. The `-f` option must be followed by the archive name. The last argument is the object that must be put in the archive, in this case a directory.

```bash
$ tar cf new.tar dir/
```

The `new.tar` file contains the `dir/` directory.\\
To extract the content of an archive to the current direcotry use the option `x`.

```bash
$ tar xf new.tar
```

### Use compression algorithms

The `tar` command can use compression algorithms. The most common are the gzip and the bzip2 algorithms. To use them creating or extracting an archive add the option `-z` for the gzip algorithm or the `-j` option for the bzip2 algorithm keeping the same syntax.

```bash
$ tar czf new.tar.gz dir/       # create using gzip
$ tar cjf new.tar.bz2 dir/      # create using bzip2
```

To decompress

```bash
$ tar xzf new.tar.gz
$ tar xjf new.tar.bz2
```

There is a convention on extensions. The file extensions above are in the long form. The contracted form is `.tgz` instead of `.tar.gz` and `.tbz` or `.tbz2` or even `.tb2` instead of `.tar.bz2`. Note that the correct extension is not mandatory but is very useful to make clear immediately what compression algorithm is required to decompress the archive.\\
This a table of supported compression algorithms with the needed options to create/extract and the aliases.

| Algorithm | Tar option | Long format                       | File extension |
| --------- | ---------- | --------------------------------- | -------------- |
| Gzip      | `-z`       | `--gzip`,  `--gunzip`, `--ungzip` | .tar.gz, .tgz  |
| Bzip2     | `-j`       | `--bzip2`                         | .tar.bz2, .tbz |
| Xz        | `-J`       | `--xz`                            | .xz            |
| Lzma      |            | `--lzma`                          | .lzma          |
| Lzop      |            | `--lzop`                          | .lzop          |

The Gzip algorithm is quite standard and for the most you can stick with it. Avoid Lzma in favour of the newer Xz when possible. A good benchmark between these algorithms is available on stephane.lesimple.fr.

## Useful features

### Read an archive without extracting

The `-t` option is to print the content of an archive without actually extracting it. The `--list` option is an alias.

```bash
$ tar tzf archive.tar.gz
```

### Append new files to an existing archive

The `-r` (or you can use also the alias `--append`) option is to append new files to an existing archive. For example to add `my_file` and `dir/` to the archive use

```bash
$ tar rf archive.tar my_file dir/
```

_Important_: the archive must not be compressed. If a compression algorithm has been used on the archive first remove it and then use the append mode on the `.tar` archive without compression. Otherwise this error is displayed

```bash
tar: Cannot update compressed archives
```

Last the `-v` option that stand as usual for verbose. The `tar` command will print the name of the files processed.

### Change directory

To specify shorter path in the command line is useful to tell tar to change the working directory. This can be done with the `-C` or `--directory` option followed by the directory to change into.

```bash
$ tar cvf archive.tar -C ~/very/long/path ./folder1 ./folder2
```

### Exclude files and directories

Suppose you want to archive you entire current folder with all the subfolders but one. You can exclude it with the `--exclude` option.

```bash
$ tar cvf archive.tar --exclude=subfolder .
```

### Preserve file permissions

When you want to extract something keeping the old permissions on the extracted files you can use the `-p` or `--preserve-permissions` or `--same-permissions` option. For the root user this is particularly useful so is enabled by default.

```bash
$ tar xzf archive.tgz --preserve-permissions .
```

## Examples

Examples are a quick way to show how to use tar.

**1.** Compress `.jpg` files in a directory

```bash
$ tar cvzf photo.tar.gz photo/*.jpg
```

**2.** As above but the archive is created in the same directory

```bash
$ tar cvzf photo/photo.tar.gz photo/*.jpg
```

**3.** Compress the current directory (1)

```bash
$ tar cvjf current.tar.bz2 .
```

**4.** Compress the current directory (2) 

```bash
$ tar cvjf current.tar.bz2 * 
```

''Note'': the difference between the two is quite simple. In the first case extracting the archive you'll find exactly the same folder you have archived. In the second you are archiving only the files inside the folder, not the folder itself. So extracting you'll find only the files and the subdirectory will not be created.

**5.** Copy a directory

```bash
$ tar cf - /some/directory | (cd /another/directory && tar  xf -)
```

**6.** Extract all the C header files (`.h`) from an archive

```bash
$ tar xvzf source.tar.gz *.h
```

**7.** Find all the `.jpg` files in `home/` and create an archive

```bash
$ find ~ -type f -name "*.jpg" | xargs tar cvzf photo.tar.gz
```

**8.** For huge amounts of files this is better

```bash
$ find ~ -type f -name "*.jpg" | xargs tar rvzf photo.tar.gz
```

The append mode is safer. tar pass arguments to tar in blocks so the archiver creates a new archive each time a new block of files is passed. But the name of the archive is always the same so it is overwritten. Using the append mode the new block of files is appended to the old.

**9.** Extract single files from an archive

```bash
$ tar xvjf archive.tar.bz2 file1.cpp file2.cpp file3.cpp
```

**10.** Extract single files from archived subdirectories

```bash
$ tar xvjf archive.tar.bz2 subdir1/file1.cpp subdir2/file2.cpp
```

''Note'': to extract single files from subdirectories you need to know exactly their names, so you will need other tools like `find`, `grep`, `awk`, to make a list.

**11.** Extract the entire subdirectory

```bash
$ tar xvjf archive.tar.bz2 subdir1/ 
```

**12.** Use a text file as a source of names to extract

```bash
$ tar xvf archive.tar -T names_list.txt
```

**13.** Print the content of an archive on a text file

```bash
$ tar tzf archive.tar.gz > names_list.txt
```

**14.** Delete from the current directory all the files already present inside an archive 

```bash
$ tar tzf archive.tar.gz | xargs rm -r
```

**15.** Estimate the size of the archive before creating it

```bash
$ tar czf - directory/ | wc -c
```

**16.** Send compress data through a SSH tunnel

```bash
$ tar czf - directory/ | ssh user@remote_host "cat > /remote/folder/archive.tar.gz"
```

**17.** The same as 16 but using `dd` instead of `cat`

```bash
$ tar czf - directory/ | ssh user@remote_host "dd of=/remote/folder/archive.tar.gz"
```

**18.** Restore a remote archive on the local machine

```bash
$ ssh user@remote_host "cat /remote/folder/archive.tar.gz" | tar xvzf -
```
