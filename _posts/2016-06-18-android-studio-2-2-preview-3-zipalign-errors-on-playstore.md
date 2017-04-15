---
layout: post
title:  Android Studio 2.2 preview 3 zipalign errors on playstore?!
date:   2016-06-18 17:29:05
categories: Android
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

When things go well, life is good; When things don't work anymore, we get reminded how easy things were before.

I updated to the latest Canary Android studio today - version 2.2 preview 3. Unfortunately, I was due to push out a minor app update when i was presented the following error on the playstore!

<img src="/assets/content/zipalign_error/upload.png" alt="uploaderror" style="width:80%">

Googling around, I found a few other developers in the same boat and decided to sign it manually like we used to do a couple years ago.

Just to be clear, usually you should have a stable version of Android Studio installed on your computer and revert to that if you have any issues.
I don't believe I could do this due me using the JACK Compiler with Java8 lang features which I don't believe have made their way into Stable as of yet.

Let's break out the [docs](https://developer.android.com/studio/publish/app-signing.html) for reference.

Okay, let's go!

###### 1 - Compile an unsigned release apk

Firstly, just make sure you comment out the release signingConfig in your buildTypes section of the app level build.gradle file as below. If you don't use buildTypes, no need to worry.

<img src="/assets/content/zipalign_error/gradle.png" alt="build.gradle" style="width:80%">

Now, open up the side gradle tab and double click the 'assembleRelease' entry as follows.

<img src="/assets/content/zipalign_error/sidebar.png" alt="gradle sidebar" style="width:60%">


You should now find an unaligned apk in the directory `app/build/outputs/apk/app-release-unsigned.apk`

###### 2 - Signing Phase

I copy this file to my desktop along with my `release_key.jks`.
On mac/linux, open up a terminal and run the next command. Substitute your own `release_key.jks` and `paragon` alias  into the command.

```lang=bash
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore release_key.jks app-release-unsigned.apk paragon
```

Verify that the apk its signed using the command

```lang=bash
$ jarsigner -verify -verbose -certs app-release-unsigned.apk
```

If you get the `Verification successful` message, you are good to go forth and zip align!

###### 3 - Zipalign the signed apk

```lang=bash
$ zipalign -v 4 app-release-unsigned.apk app-release.apk
```

This should be all that is needed to prep your .apk for the PlayStore!

###### 4 - Release time!

If all goes well, it's time for release!

<img src="/assets/content/zipalign_error/current.png" alt="release" style="width:80%">

###### Final thoughts

All and all, Android Studio 2.2 preview 3 is currently broken for signing, but that's ok! It is a preview and we expect bugs. Five minutes of extra effort is fine but I look forward to the easy automatic was in the next release :)

Cheers!
