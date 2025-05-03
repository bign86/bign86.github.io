---
title: "Case conversions in the shell"
date: 2009-03-07
modified: 2015-07-09
tags:
  - shell
  - linux
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

This post collects ways in several languages to convert between string cases in Bash.

## Using `tr`

To convert a whole text file from upper to lowercase and viceversa

```bash
tr "[:upper:]" "[:lower:]" < input.txt > output.txt
tr "[:lower:]" "[:upper:]" < input.txt > output.txt
```

To convert the content of a variable

```bash
echo $VAR_NAME | tr "[:upper:]" "[:lower:]"
echo $VAR_NAME | tr "[:lower:]" "[:upper:]"
```

You can also use another syntax for the same command

```bash
tr '[A-Z]' '[a-z]' < input.txt
tr '[a-z]' '[A-Z]' < input.txt
```

This method is easy to use within a loop in a shell script

```bash
find . -type f | while read f; do mv $f `echo $f | tr "[:lower:]" "[:upper:]"`; done
find . -type f | while read f; do mv $f `echo $f | tr "[:upper:]" "[:lower:]"`; done
```

## Using `dd`

The command `dd` has an option for case conversion

```bash
dd if=input.txt of=output.txt conv=ucase
dd if=input.txt of=output.txt conv=lcase
```

## Using `awk`

The `print` command and the powerful program `awk` can be combined to change the case

```bash
awk '{ print toupper($0) }' input.txt > output.txt
awk '{ print tolower($0) }' input.txt > output.txt
```

To convert a variable

```bash
echo $VAR_NAME | awk '{ print toupper($0) }'
echo $VAR_NAME | awk '{ print tolower($0) }'
```

## Using perl

Using the Perl language you can take advantage of the functions `uc()` and `lc()`

```bash
perl -pe '$_= uc($_)' input.txt > output.txt
perl -pe '$_= lc($_)' input.txt > output.txt
```

The same technique can be used inside a Perl script.

## Using `sed`

The `sed` command has a way to search for single characters and convert the case

```bash
sed -e 's/\(.*\)/\U\1/' input.txt > output.txt
sed -e 's/\(.*\)/\L\1/' input.txt > output.txt
```

## Using Bash 4.x

Since Bash 4.0 a new parameter expansion for case conversion has been introduced

```bash
${VAR_NAME,,}    # to lowercase
${VAR_NAME^^}    # to uppercase
```
