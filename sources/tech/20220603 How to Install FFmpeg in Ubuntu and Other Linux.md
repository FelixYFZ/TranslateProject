[#]: subject: "How to Install FFmpeg in Ubuntu and Other Linux"
[#]: via: "https://www.debugpoint.com/2022/06/install-ffmpeg-ubuntu/"
[#]: author: "Arindam https://www.debugpoint.com/author/admin1/"
[#]: collector: "lkxed"
[#]: translator: " "
[#]: reviewer: " "
[#]: publisher: " "
[#]: url: " "

How to Install FFmpeg in Ubuntu and Other Linux
======
This tutorial outlines the steps required to install FFmpeg in Ubuntu and Other Linux systems.

The ffmpeg is a collection library and software program to manipulate multimedia files. The entire ffmpeg is a robust set of libraries that allows you to convert, stream, and manipulate audio and video files. Many frontend Linux applications use it as backend hence depends on it. For example, a screen recording application may need ffmpeg to convert recorded streams to gif images.

Popular applications and services that use FFmpeg are VLC Media Player, YouTube, Blender, Kodi, Shotcut, and Handbrake – to name a few.

Fun fact: NASA’s Mars 2020 mission rover Perseverance used FFmpeg to complete and process images and video before beaming back to Earth!

### About ffmpeg package

The [ffmpeg][1] itself is a powerful program as a command-line utility. It is available for Linux, Windows, and macOS and supports many architectures. It is written in C and Assembly, providing extensive performance and a cross-platform utility.

#### The Core

The core of ffmpeg is the command-line utility or programs. They can be used on the command line or called from any programming language. For example, you can use these from your shell program, python script, etc.

* ffmpeg: Used to convert audio and video streams, including sources from LIVE streams such as TV cards
* ffplay: Media player bundled in this package to play media
* ffprobe: Command line tool to show media information – can output as txtm csv, xml, json formats

### FFmpeg Installation

Installing FFmpeg is easy in Ubuntu and other Linux distributions. Open a terminal prompt and run the following commands to install.

#### Ubuntu and similar distro

```
sudo apt install ffmpeg
```

#### Fedora

For Fedora Linux, you need to add the [RPM Fusion repo][2] for FFmpeg. The official Fedora repo doesn’t have the FFmpeg package.

```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```

```
sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-
```

```
sudo dnf install ffmpeg
```

#### Arch Linux

```
pacman -S ffmpeg
```

After the successful installation, you can verify the installation using the below command.

```
ffmpeg --version
```

![FFmpeg installed in Ubuntu Linux][3]

### Example: How to do basic tasks using ffmpeg

First, let me give you a simple example of the basic syntax. Consider the following example. It simply converts an mp4 file to mkv file.

1. Convert a basic video file

```
ffmpeg -i big_buck_bunny.mp4 big_buck_bunny.mkv
```

Of course, this is the easiest method, but it’s not complete because it doesn’t have the bit rate, resolution and other attributes of the video file required for the conversion.

1. Convert an audio file

Secondly, you can convert an audio file using a similar command.

```
ffmpeg -i sunny_day.ogg sunny_day.mp3
```

1. Convert with an audio and video codec

Finally, the following example can convert a video file using specified codecs. The parameter `-c` with `a` or `v` defines audio and video, respectively. The below command uses `libvpx` video and `libvorbis` audio codec for conversion.

```
ffmpeg -i big_buck_bunny.mp4 -c:v libvpx -c:a libvorbis big_buck_bunny.webm
```

### How to find out about the available codecs, encoders and decoders in your system?

#### List all codecs

To list all the codecs available, run the below command.

```
ffmpeg -codecs
```

This command lists all the codecs available with their capability, whether they support decoding or encoding, etc. Moreover, they are identified with the position as per the below table.

```
D..... = Decoding supported.E.... = Encoding supported..V... = Video codec..A... = Audio codec..S... = Subtitle codec...I.. = Intra frame-only codec....L. = Lossy compression.....S = Lossless compression
```

![FFmpeg Codec list][4]

#### List all encoders

Listing all the encoders is accessible via the below command.

```
ffmpeg -encoders
```

#### List all decoders

Similarly, the decoders list you can get via the below command.

```
ffmpeg -decoders
```

#### Details

You can also get more details about the encoders or decoders using the parameter -h.

```
ffmpeg -h decoder=mp3
```

### Summary

I hope you learned the basics of FFmpeg and its commands. You can learn more about the program via the official [documentation][5].

--------------------------------------------------------------------------------

via: https://www.debugpoint.com/2022/06/install-ffmpeg-ubuntu/

作者：[Arindam][a]
选题：[lkxed][b]
译者：[译者ID](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://www.debugpoint.com/author/admin1/
[b]: https://github.com/lkxed
[1]: https://ffmpeg.org/
[2]: https://www.debugpoint.com/2020/07/enable-rpm-fusion-fedora-rhel-centos/
[3]: https://www.debugpoint.com/wp-content/uploads/2022/06/FFmpeg-installed-in-Ubuntu-Linux.jpg
[4]: https://www.debugpoint.com/wp-content/uploads/2022/06/FFmpeg-Codec-list.jpg
[5]: https://ffmpeg.org/documentation.html