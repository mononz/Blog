---
layout: post
title:  Android Lambdas using Java7
date:   2017-02-07 10:13:04
categories: Android
---

Tired of writing out `new View.OnClickListener() {...} ` even though it is an autocomplete...? screw it! There is a much nicer way using Java 8 lambdas!!

Thankfully we can use backported Java8 lambdas in our Java 7 Android projects with the help of RetroLambda!

#### Why use Retrolambda over Jack?

> Retrolambda lets you run Java 8 code with lambda expressions, method references and try-with-resources statements on Java 7, 6 or 5...

Officially we would need to use the new Jack compiler in order to use Java8 features.

I have used the jack compiler before but I was plagued with extremely slow compile times and many failed builds. I ditched it and returned to developing with Java 7.

Quite simply, Jack is not ready yet. But I do look forward to the day when I can comfortably run Jack with more complete Java 8 features.

#### What can we do with Retrolambda?

we can turn these 6 lines
```language=java
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        doStuff();
    }
});
```

into one simple line!
```language=java
button.setOnClickListener(view -> doStuff());
```
<br>
#### Add Retrolambda to Android project

In your main build.gradle file, add the `classpath 'me.tatarka:gradle-retrolambda:3.5.0'` dependency
```language=groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.0-beta3'
        classpath 'com.google.gms:google-services:3.0.0'
        classpath 'me.tatarka:gradle-retrolambda:3.5.0'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
...
```

And in your app/build.gradle file at the top add the `apply plugin: 'me.tatarka.retrolambda'` plugin
```language=groovy
apply plugin: 'com.android.application'
apply plugin: 'me.tatarka.retrolambda'

android {
...
```

At the time of writing this post, the latest version of gradle-retrofit is 3.5.0, get the latest version number from [here](https://github.com/evant/gradle-retrolambda/releases)

#### Pro Tip 1 - Minimal effort update of old to new

Using Android Studio? click on the line suggesting the change. Then press `alt`+`return` (on a mac). Simply select `Replace with lambda` and it will automagically (with minimal effort) turn this long operation

<img src="https://static.mononz.net/ghost/content/retrolambda/alt-return-before.png" alt="alt return tip before" style="width:80%">

into this short and sweet operation!

<img src="https://static.mononz.net/ghost/content/retrolambda/alt-return-after.png" alt="alt return tip after" style="width:80%">

####Pro Tip 2 - Even less effort!

In Android studio, click on the `Analyze` Tab then `Run inspection by name...`, type in something like *can be replace with lambda* and run the `Anonymous type can be replaced with lamba` inspection.

<img src="https://static.mononz.net/ghost/content/retrolambda/inspection.png" alt="run inspection by name" style="width:80%">

Check the Inspection results and quickly go through each result and `alt`+`enter` them all as specified above. Done and Dusted nice and quick!
