---
layout: post
title:  Android Reverse Engineering
date:   2016-07-20 14:32:27
categories: Android
subclass: 'post'
navigation: True
logo: 'assets/images/ghost.png'
---

As an Android developer, I feel it is important to know how an Android package is constructed and how you can protect your codebase from being copied by others.

#### APKs

An Android APK is a essentially just a zip file. You can rename it as a .zip and extract the main dex file and all other assets, drawables and other binary data.

<img src="/assets/content/decomplile/inside_apk.png" alt="inside an apk" style="width:80%">

The classes.dex file is where all the java source is compiled. To access the java classes inside, we need to convert this dex to a jar. For this, I used the [Jadx](https://github.com/mononz/jadx) decompiler.

With Jadx, you can open an APK in the GUI and look at the java source code. For any android developer, this code is very readable.
I have decompiled quite a few apps to see how they work and to see if implementing similar strategies in my own apps would be beneficial.

Be aware, the code is not totally identical to that of the original developer and ids and references can be replaced using the R.class file.
Also, if an Android app has been compiled with proguard obfuscation methods, the code will be near impossible to understand.

#### Final Thoughts

There are other decompilers available but I have found jadx very useful. One online decompiler option can be found at http://www.javadecompilers.com/apk
[dex2jar](https://github.com/pxb1988/dex2jar) is a also a helpful toolkit for messing with Android files.

I strongly suggest decompiling your own apps and checking what potential attackers can easily access. Hiding API keys and other credentials may be required!

**References**<br>
[Unbundling Pokemon Go](https://applidium.com/en/news/unbundling_pokemon_go/)
