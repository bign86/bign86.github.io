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

How to convert movies between different formats using the `ffmpeg` library? First, we see the convertion from an AVI into a DVD or VCD, and then from an AVI into a Flash movie.

## Convert a DivX into a DVD or VCD

The base `ffmpeg` syntax is the following

```bash
ffmpeg -i myDivx.avi -target pal-dvd -aspect 16:9 -sameq myDVD.mpg
```

| Option    | Meaning                                           |
| --------- | ------------------------------------------------- |
| `-i`      | input file                                        |
| `-target` | type of device that will read the output file     |
| `-sameq`  | keep the same quality of the source file          |
| `-aspect` | output format. Common formats are 16:9 and 4:3    |

To convert to the VCD or NTSC-VCS format the general syntax is the same

```bash
ffmpeg -i myDivx.avi -target vcd -sameq myVCD.mpg
# or
ffmpeg -i myDivx.avi -target ntfc-vcd -sameq myVCD.mpg
```

### Some other useful options

| Option  | Meaning                                     |
| ------- | ------------------------------------------- |
| `-hq`   | high quality output                         |
| `-bf 2` | use both a and b frames to convert to MPEG2 |

For example

```bash
ffmpeg -i myDivx.avi -bf 2 -target ntfc-vcd myVCD.mpg
```

## Convert to a Flash video

Converting into the Flash format is very simple

```bash
ffmpeg -i myDivx.avi myFlash.fvl
```

### Some useful options

| Option       | Meaning                                                 |
| ------------ | ------------------------------------------------------- |
| `-ab [num]`  | output sound bitrate. Default is 64                     |
| `-ar [freq]` | output frequency. Default is the same of the input file |
| `-b [num]`   | output video bitrate. Default is 200                    |

Notice that the Flash format does not support the 44800 Hz audio frequency but only 44100, 22050 e 11025 Hz. An error is displayed when converting from a 44800 Hz file without specifyng a supported frequency for the output.

For example

```bash
ffmpeg -i myDivx.avi -ab 192 -ar 44100 myFlash.fvl
```

_Warning_: the `-sameq` option retaing the quality of the source. Pay attention to the dimension of the output file as with this option enabled could become really large.
