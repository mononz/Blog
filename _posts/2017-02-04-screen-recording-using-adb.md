---
layout: post
title:  Screen recording using ADB
date:   2017-02-04 10:08:39
categories: Android
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

Can easily record your screen using the android debug bridge (adb). I personally find it is the easiest way to create short clips for sharing with others whilst developing my apps.

With your phone connected to your computer, use the
```
adb shell screenrecord
```

A simple use case for recording the screen and outputting to a video file is
```
adb shell screenrecord /sdcard/file.mp4
```

Use `Ctrl+C` to end your recording.

To download the file, we can use adb as well!
```
adb pull /sdcard/file.mp4 ls ~/Documents/file.mp4
```

Use the --help extension to find out what more you can do!
```
adb shell screenrecord –-help
```
