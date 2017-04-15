---
layout: post
title:  Building Ogre3D for Android (on OSX)
date:   2016-07-22 00:06:29
categories: Ogre3D
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

#### Prerequisites

- Install Android Studio
- Install the Android SDK
- Install the Android NDK
- Download and install [CMake](http://www.cmake.org/cmake/resources/software.html)
- Download the latest [Ogre Android dependencies](https://sourceforge.net/projects/ogre/files/ogre-dependencies-android/). I used [this version]( 'https://sourceforge.net/projects/ogre/files/ogre-dependencies-android/1.9/AndroidDependencies_27_08_2013.zip/download)
- Clone the Ogre v1-9 branch using [Mercurial](https://www.mercurial-scm.org/) with the following command: `hg clone https://bitbucket.org/sinbad/ogre -r v1-9`

#### Build Ogre3D

Add the following to either `~/.bash_profile` or `~/.profile` and sub out your appropriate paths
```language=shell
export ANDROID_SDK="/path/to/androidsdk"
export ANDROID_NDK="/path/to/androidndk"
export PATH="$PATH:$ANDROID_SDK/tools:$ANDROID_SDK/platform-tools:$ANDROID_NDK"
```

ogredeps/build
```
$ cmake -G"Unix Makefiles" -DCMAKE_TOOLCHAIN_FILE="../cmake/android.toolchain.cmake" -DANDROID_ABI=armeabi-v7a -DANDROID_NATIVE_API_LEVEL=24 -DANDROID_TOOLCHAIN_NAME=arm-linux-androideabi-4.9 ..
```

ogre-1-9
```
cmake -DCMAKE_TOOLCHAIN_FILE="../Cmake/toolchain/android.toolchain.cmake" -DCMAKE_C_COMPILER="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang" -DCMAKE_CXX_COMPILER="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++" -DOGRE_DEPENDENCIES_DIR="/Users/mononz/Work/ogredeps/build" -DANDROID_ABI=armeabi-v7a  -DANDROID_NATIVE_API_LEVEL=24 -DANDROID_TOOLCHAIN_NAME=arm-linux-androideabi-4.9 ..
```
