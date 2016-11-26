---
layout: post
title: "How to Download Youtube Media Files"
mathjax: false
related: false
comments: true
published: true
---


_Created: Nov, 2016_

There are many musicians playing amazing pieces on Youtube, and here is my way to grab the video and audio for offline access with a handy Python tool called [youtube-dl](https://rg3.github.io/youtube-dl/). 

First, install youtube-dl with pip: 

```
virtualenv pyenv
source pyenv/Scripts/activate  # this may be different on different platforms
pip install --upgrade pip
pip install youtube-dl
```

To extract the audio, one also needs to install ffmpeg utilities. 

* On Windows, download it from [here](https://ffmpeg.zeranoe.com/builds/). The statically linked binaries will be convenient. Add the "bin" path to system path. 

* On Mac, install [Homebrew](http://brew.sh/), and install ffmpeg from there

```
brew install ffmpeg
```

At last, use youtube-dl as follows

```
youtube-dl -x -k https://www.youtube.com/watch?v=MZuSaudKc68
```

This will grab all available versions of videos, and extract the audio by the end. 

