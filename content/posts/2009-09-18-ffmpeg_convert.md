---
title: "Convert audio/video with ffmpeg"
date: 2009-09-18
modified: 2011-09-14
tags:
  - linux
  - shell
  - audio
  - video
categories:
  - multimedia
author_profile: true
draft: false
comments: false
---

How to convert movies between different formats using the ffmpeg library. First we see convertion from an AVI into a DVD or VCD and then from a AVI file to a Flash movie.

## Convert a DivX into a DVD or VCD

The syntax to use is the following

```bash
$ ffmpeg -i myDivx.avi -target pal-dvd -aspect 16:9 -sameq myDVD.mpg
```

| Option    | Meaning                                           |
| --------- | ------------------------------------------------- |
| `-i`      | input file                                        |
| `-target` | kind of device that will read the output file     |
| `-sameq`  | tells to keep the same quality of the source file |
| `-aspect` | output format. Common formats are 16:9 and 4:3    |

To convert to the VCD or NTSC-VCS format the syntax is the same. Only the target change

```bash
$ ffmpeg -i myDivx.avi -target vcd -sameq myVCD.mpg
or
$ ffmpeg -i myDivx.avi -target ntfc-vcd -sameq myVCD.mpg
```

### Some other useful options

| Option  | Meaning                                     |
| ------- | ------------------------------------------- |
| `-hq`   | to have an high quality output              |
| `-bf 2` | use both a and b frames to convert to MPEG2 |

For example

```bash
$ ffmpeg -i myDivx.avi -bf 2 -target ntfc-vcd myVCD.mpg
```

## Convert to a Flash video

Converting in Flash format is very simple

```bash
$ ffmpeg -i myDivx.avi myFlash.fvl
```

### Some useful options

| Option       | Meaning                                                 |
| ------------ | ------------------------------------------------------- |
| `-ab [num]`  | output sound bitrate. Default is 64                     |
| `-ar [freq]` | output frequency. Default is the same of the input file |
| `-b [num]`   | output video bitrate. Default is 200                    |

Notice that the Flash format doesn't support the 44800 Hz audio frequency but only 44100, 22050 e 11025 Hz. If converting a 44800 Hz file we don't specify a different frequency for the output, an error will be displayed.

For example

```bash
$ ffmpeg -i myDivx.avi -ab 192 -ar 44100 myFlash.fvl
```

_Warning_: to toggle the quality is available the -sameq option. But pay attention to the dimension of the output file. ith this option enabled can be very large.
