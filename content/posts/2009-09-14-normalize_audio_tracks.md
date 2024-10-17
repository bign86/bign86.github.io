---
title: "Normalize audio tracks volume from the command line"
date: 2009-09-14
modified: 2011-09-25
tags:
  - linux
  - shell
  - audio
categories:
  - multimedia
author_profile: true
draft: false
comments: false
---

Collections of mp3 files are usually made using different programs with different settings, on Linux, Windows etc. In each playlist there are files with different volume and we have to change the output volume of the speakers all the time. Volume normalization level the volume of the tracks to a standard. To achieve that only a small portion of the file is modified called the gain. The software can increase or lower the gain saving the track from distortions.\\
In this how to I present how to normalize the volume using 3 different programs and an example of how to automate the process using the shell.

### Contents

- [Mp3Gain](#mp3gain)
- [Vorbisgain](#vorbisgain)
  - [Other useful options are](#other-useful-options-are)
- [Normalize](#normalize)
- [Automate with the Shell](#automate-with-the-shell)

## Mp3Gain

Mp3Gain is a very famous program present in every distributions and also for Windows and Mac. On Debian install it with

```bash
# apt-get install mp3gain
```

on RedHat and derivates 

```bash
# yum install mp3gain
```

Mp3Gain can check the files in a directory to give us hints on what is best to do with those files with the option `-x`. The wildcard `*` is accepted to analyze more than one file. For example

```bash
$ mp3gain -x song.mp3
Recommended "Track" dB change: 0.380000 
Recommended "Track" mp3 gain change: 2.5
Max PCM sample at current gain: 17485.364916
Max mp3 global gain field: 176
Min mp3 global gain field: 88
```

The program suggest to raise both volume (in dB) and the gain.\\
The files has not been modified yet. To apply the suggestion enter

```bash
$ mp3gain -g 2.5 file.mp3
```

To normalize a whole directory and have all the files at the same level of volume use the option `-r`

```bash
$ mp3gain -r file1.mp3 file2.mp3 ...
```

Inside an album there can be tracks intentionally kept to a lower volume to get some artistic effect that is lost normalizing tracks. To preserve relative differences in volume between tracks Mp3Gain can work only on the gain but not on the volume base line using the `-a` option

```bash
$ mp3gain -a file1.mp3 file2.mp3 ...
```

mp3 files are usually dual channel, so they have a right (1) and left (0) channel. Mp3Gain can modify the gain on only one channel wih option `-l`. The syntax to use is `mp3gain -l <channel> <gain_difference> file.mp3`. For example

```bash
$ mp3gain -l 0 2 file.mp3
```

this command raise the gain of the left channel of two points.\\
Mp3Gain has also a "undo" option `-u` to cancel the last modification

```bash
$ mp3gain -u file.mp3
```
For more useful options the manual page is always the resource to read

```bash
$ man mp3gain
```

## Vorbisgain

Vorbisgain is specific for the ogg file format. It works using the ReplayGain algorithm to obtain a gain of 89 dB for each track, the same level used for a normal CD. The program doesn't modify the real track but append a specific tag to the file with the output of its calculation of the optimal gain.\\
The syntax is

```bash
$ vorbisgain file.ogg
```

To work on an entire album there is the `-g` option that tells Vorbisgain to calculate a medium gain optimal for every track in the album and apply that gain to each track in the same way.

```bash
$ vorbisgain -a folder/*.ogg
```

Vorbisgain can undo a modification simply removing the tags from the files using the `-c` option

```bash
$ vorbisgain -c folder/*.ogg
```

### Other useful options are

```bash
$ vorbisgain -r -f folder/*.ogg
```

| Option | Meaning                                                           |
|:------:| ----------------------------------------------------------------- |
| `-r`   | enter recursively in subfolders                                   |
| `-f`   | process only file that don't have ReplayGain tags. Skip the other |

As always check the man page

```bash
$ man vorbisgain
```

## Normalize

Normalize allows to modify the gain on wav, mp3 and ogg files decoding the file, modifying, and recoding. This is a very long process and fortunately from the version 0.7 of the program, gain in wav and mp3 files can be modified without decoding. For ogg is still mandatory. Normalize should be present in every distribution.\\
The package to install in Debian is

```bash
# apt-get install normalize-audio
```

To normalize wav files the syntax is

```bash
$ normalize-audio file1.wav file2.wav ...
```

The files will be all at the same volume. But if you want to keep relative volume between tracks like for Mp3Gain, you can use the batch mode with the `-b` option

```bash
$ normalize-audio -b folder/*.wav
```

In batch mode the program calculate the median and apply the same gain to all tracks. The interesting thing it's that it calculate also the standard deviation to eliminate from the calculation of the median the tracks that are to high or to low in volume. For those very different files a different calculation is applied to level them to the others.\\
There is also the mix mode (`-m` option). In mix mode all files are leveled to the same volume without the median calculation like in the batch mode.

```bash
$ normalize-audio -m folder/*.wav
```

For mp3 file the program works in the same way as for wav files. From the version 0.7 mp3 don't have to be decoded to modify the gain. However this doesn't work with ogg files that need to be decoded. Commands are the same but the time and memory requested are much higher.\\
The old scripts to uncompress mp3 and ogg files are still there in the package. They can be used as conversion tools from one format to the other. To uncompress an mp3 or ogg file enter

```bash
$ normalize-mp3 file.mp3
$ normalize-ogg file.ogg
```

To use these script to convert the syntax is very simple. To convert a mp3 in ogg

```bash
$ normalize-mp3 --ogg file.mp3
```

To convert a ogg in mp3

```bash
$ normalize-ogg --mp3 file.ogg
```

The default output bitrate is 128 bit/s, but you can change it with the `--bitrate` option

```bash
$ normalize-mp3 --ogg --bitrate 192 file.mp3
```

Has to be said that other conversion tools like lame or ffmpeg do a better job.

## Automate with the Shell

Process many files sequencially by hand is annoying. But the shell can easely automate the job using the find command. Here there are some examples of find used to normalize tracks.

**1.** These two lines are equivalent. They search for any file with `.mp3` extension and pipe it to Mp3Gain.

```bash
$ find . -type f -iname *.mp3 -exec mp3gain -a {} \;
$ find . -type f -iname *.mp3 -print0 | xargs -0 mp3gain -a
```

**2.** Search recursively for mp3 files and  use normalize-audio

```bash
$ find . -type d -exec sh -c "normalize-audio -b \"{}\"/*.mp3" \;
```

**3.** The same as the first but with `normalize-audio`

```bash
$ find . -type f -iname \*.mp3 -exec normalize-audio {} \;
$ find . -type f -iname \*.mp3 -print0 | xargs -0 normalize-audio -b
```
