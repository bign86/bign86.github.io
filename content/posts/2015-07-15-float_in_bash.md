---
title: "Floating point calculations in Bash"
date: 2015-07-15
tags:
  - shell
  - linux
  - math
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

A very well known problem of Bash is the absence of floating point number capabilities. Shells in general are not designed to perform calculations, with the Korn shell 93 (Ksh93) being the only that as far as I know can do these operations. However, solutions are widely available altough not suitable for long and complex calculations. The main problem is that in Bash everything is a string, even numbers, and is impossible to operate on the a that represents a decimal number. Therefore, we need to rely on external tools. `bc` has been designed for this purpose, but also `awk` could be used.

`bc` is not only an executable, is the interpreter of a real programming language able to understand operations passed as strings and give the result. That means that we can use it also for more complex thing than simple additions or multiplications. A detailed guide to `bc` is not the purpouse of this article, so I will simply scratch the top and show some code to use the simplest features of this interpreter.

## Floating point operations

To perform a simple floating point operation pass a string describing the math to `bc`

```bash
echo "1.452 + 9.236" | bc
10.688
echo "171/98" | bc
1
```

In the second example above, 1 is the correct integer part of the result, but we want decimals! `bc` by default gives us in the output the same number of decimal digits used in the input. The scale parameter manage the number of decimals we want to see

```bash
echo "scale=5; 171/98" | bc
1.74489
```

Operations can be concatenated

```bash
echo "1.2+3.4*5.6/7.8" | bc
3.2
```

Last result can be reused using `last` or a dot `.`

```bash
# Using last
echo "1.2+3.4;last*5.6" | bc
4.6
25.7

# Using a .
echo "1.2+3.4;.*5.6" | bc
4.6
25.7
```

## Comparisons

We can do comparisons with `bc`

```bash
echo "1.5 < 4.5" | bc -l
1   # True
echo "1.5 > 4.5" | bc -l
0   # False
echo "2.4/4.8 == 0.5" | bc -l
1   # True
```

## Use in scripts

Using it in scripts is very easy. Few pieces of codes:

1. **Define a variable**: A variable can be defined as usual

   ```bash
   FLOAT_NUM=$(echo "scale=5; 171/98" | bc)
   ```

2. **Function**: simple floating point functions

   ```bash
   # First example
   floatCalc() {
       echo "scale=5; $@" | bc -l
   }
   
   # Second example
   floatCalc() {
       bc -l <<< "scale=5; $@"
   }
   ```

3. **True/False comparisons**: they can be used inside conditional statements in scripts

   ```bash
   if [ $(echo "$1 > $2" | bc -l) -ne 0 ]; then
       echo "True"
   else
       echo "False"
   fi
   ```

4. **Numbers converters**: `bc` can be used to convert numbers from one base to another. The two special variables `ibase` and `obase` define the base of the input and of the output. For example to convert the number 42 from base 10 to binary

   ```bash
   echo "ibase=10; obase=2; 42" | bc
   101010
   ```

## More operators

Many operators are available using the standard math library enabled using the `-l` flag. Refer to the `bc` documentation for a full list. Here a few examples

1. **Exponentiation**

   ```bash
   echo "1.5123^9" | bc -l
   41.37533941184533627968
   ```

2. **Natural logaritm**

   ```bash
   echo "l(1.5123)" | bc -l
   .41363167077455779798
   ```

3. **Sine, cosine**

   ```bash
   echo "s(1.5123)" | bc -l
   .99828957768901794850
   echo "c(1.5123)" | bc -l
   .05846297184955788583
   ```

4. **Square root**

   ```bash
   echo "sqrt(2)" | bc -l
   1.41421356237309504880
   ```

## Final note

There are several flavours of shell for Unix. None of them support floating point numbers with one exception. The Korn shell 93 supports floating-point numbers, but as far as I know is the only version of Ksh with this feature.

## Useful websites

[Phoxis](http://phoxis.org/2009/12/23/floatmathbash/)\
[LJ](http://www.linuxjournal.com/content/floating-point-math-bash)\
[Bc manual](http://www.gnu.org/software/bc/manual/html_mono/bc.html)
