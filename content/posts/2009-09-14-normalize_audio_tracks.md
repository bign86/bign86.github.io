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

Collections of mp3 files may be built using several softwares with different settings resulting in files with different volume that forces us to adjust the speakers all the time. Volume normalization levels track's volume to a standard by adjusting the decibel volume and the gain in the file. The software increases or lowers the gain preventing sound distortions when the track is played.\
In this how to I show how to normalize the volume using 3 different softwares and an example on how to automate the process using Bash.

### Contents

- [Mp3Gain](#mp3gain)
- [Vorbisgain](#vorbisgain)
  - [Other useful options are](#other-useful-options-are)
- [Normalize](#normalize)
- [Automate with the Shell](#automate-with-the-shell)

## Mp3Gain

Mp3Gain is a famous software present in every distributions and also available on Windows and Mac. On Debian, install it with

```bash
apt-get install mp3gain
```

on RedHat and derivates

```bash
yum install mp3gain
```

With the option `-x` Mp3Gain checks the files in a directory to give us hints on what is best to do with them. The wildcard `*` is accepted to analyze more than one file. For example

```bash
mp3gain -x song.mp3
Recommended "Track" dB change: 0.380000
Recommended "Track" mp3 gain change: 2.5
Max PCM sample at current gain: 17485.364916
Max mp3 global gain field: 176
Min mp3 global gain field: 88
```

Mp3Gain is suggesting to raise both decibels and the gain.\
To apply the suggestion

```bash
mp3gain -g 2.5 file.mp3
```

To normalize a whole directory and have all the files at the same level of volume use the option `-r`

```bash
mp3gain -r file1.mp3 file2.mp3 ...
```

Inside an album there can be tracks intentionally kept to a lower volume to get some artistic effect that would be lost normalizing the tracks. To preserve relative differences in volume between tracks, Mp3Gain can work only on the gain but not on the volume base line using the `-a` option

```bash
mp3gain -a file1.mp3 file2.mp3 ...
```

mp3 files are usually dual channel, so they have a right (1) and left (0) channel. Mp3Gain can modify the gain on only one channel wih option `-l`. The syntax to use is `mp3gain -l <channel> <gain_difference> file.mp3`. For example

```bash
mp3gain -l 0 2 file.mp3
```

raises the gain of the left channel by 2 points.\
Mp3Gain also has an "undo" option `-u` to cancel the last modification

```bash
mp3gain -u file.mp3
```

For more useful options the manual page is the resource to read

```bash
man mp3gain
```

## Vorbisgain

Vorbisgain is specific for the ogg file format. It works using the ReplayGain algorithm to set a gain to 89 for each track, the same level used in CDs. Vorbisgain does not modify the real track but appends a specific tag to the file with the output of its calculation on the optimal gain.\
The syntax is

```bash
vorbisgain file.ogg
```

To work on an entire album, the `-g` option tells Vorbisgain to calculate a medium gain optimal for every track in the album and applies it to each track.

```bash
vorbisgain -a folder/*.ogg
```

Vorbisgain can undo a modification simply removing the tags from the files using the `-c` option

```bash
vorbisgain -c folder/*.ogg
```

### Other useful options are

```bash
vorbisgain -r -f folder/*.ogg
```

| Option | Meaning                                                            |
|:------:| ------------------------------------------------------------------ |
| `-r`   | enter recursively in subfolders                                    |
| `-f`   | process only file that do not have ReplayGain tags. Skip the other |

As always, check the man page

```bash
man vorbisgain
```

## Normalize

Normalize allows to modify the gain on wav, mp3 and ogg files decoding the file, modifying, and recoding. This is a very long process and fortunately since version 0.7 of the software, gain in wav and mp3 files can be modified without decoding. For ogg is still mandatory. Normalize should be present in every distribution.\
The package to install in Debian is

```bash
apt-get install normalize-audio
```

To normalize wav files to the same volume the syntax is

```bash
normalize-audio file1.wav file2.wav ...
```

To keep relative volume difference between tracks intact, use the batch mode with the `-b` option

```bash
normalize-audio -b folder/*.wav
```

In batch mode Normalize calculates the median and applies the same gain to all tracks. Normalize also calculates the volume standard deviation to eliminate from the median calculation those tracks that are too high or too low in volume. Outliers are then normalized separately.\
There is also a mix mode `-m` option where all files are leveled to the same volume without the median calculation like in the batch mode.

```bash
normalize-audio -m folder/*.wav
```

For mp3 files Normalize works in the same way as for wav files. Since version 0.7 mp3s do not have to be decoded to modify the gain. Note that this still does not work with ogg files. The commands are the same as for the wav case, however the time and memory requested are much higher.\
Packaged in the software there are scripts to decompress mp3 and ogg files taht can be used as conversion tools between formats. To decompress a mp3 or ogg file enter

```bash
normalize-mp3 file.mp3
normalize-ogg file.ogg
```

To convert a mp3 in ogg

```bash
normalize-mp3 --ogg file.mp3
```

while to convert a ogg in mp3

```bash
normalize-ogg --mp3 file.ogg
```

The default output bitrate of Normalize is 128 bit/s, but it can be changed with the `--bitrate` option

```bash
normalize-mp3 --ogg --bitrate 192 file.mp3
```

Note that at the time of this post, other conversion tools like lame or ffmpeg do a better job in converting between formats and should be preferred.

## Automate with the Shell

To process many files sequencially by hand is annoying and time consuming. The shell can easily automate the job using the `find` command. Here there are some examples

1. The following 2 lines are equivalent. They search for any file with `.mp3` extension and pipe it to Mp3Gain.

  ```bash
  find . -type f -iname *.mp3 -exec mp3gain -a {} \;
  find . -type f -iname *.mp3 -print0 | xargs -0 mp3gain -a
  ```

2. Search recursively for mp3 files and use `normalize-audio`

  ```bash
  find . -type d -exec sh -c "normalize-audio -b \"{}\"/*.mp3" \;
  ```

3. The same as in point 1 but using `normalize-audio`

  ```bash
  find . -type f -iname \*.mp3 -exec normalize-audio {} \;
  find . -type f -iname \*.mp3 -print0 | xargs -0 normalize-audio -b
  ```
