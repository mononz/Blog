---
layout: post
title:  Basic Gradle Awesomeness
date:   2016-07-20 15:08:11
categories: Android
---

Want to hide hardcoded constants like URLs, API keys and other credentials? Gradle has a great place for these.

Below is a slightly reduced build.gradle (app) file that I use.

```language=gradle
apply plugin: 'com.android.application'

android {
   signingConfigs {
       debug {
           keyAlias 'debugAlias'
           keyPassword 'password'
           storeFile file('../.debug.jks')
           storePassword 'password'
       }
       release {
           keyAlias 'releaseAlias'
           keyPassword 'releasePassword'
           storeFile file('../release.jks')
           storePassword 'releasePassword'
       }
   }
   compileSdkVer ...
   defaultConfig { .... }
   buildTypes {
       debug {
           debuggable true
           signingConfig signingConfigs.debug
           buildConfigField 'String', 'SERVER_URL', '"DEV_API.EXAMPLE.COM"'
       }
       release {
           debuggable false
           signingConfig signingConfigs.release
           buildConfigField 'String', 'SERVER_URL', '"API.EXAMPLE.COM"'
       }
   }
   buildTypes.each {
       it.buildConfigField 'int', 'DB_VERSION', '3'
   }
}
dependencies { ... }
```

### Why put constants in build.gradle rather than in a resources folder?

During the build process, the constants listed here will actually be built into the resources files, so why do it?

The /res folder to me stands for constants like strings, dimensions, assets and layouts that don't change. I like to put important constants that 'could' change per app release in the build.gradle file.

During an app release/update process, we come to the build.gradle file in order to update the versionCode and versionName. Automatically I can see `DB_VERSION` constant and decide whether it also needs to change too.

### Access build.gradle values at runtime

Values listed in the build.gradle folder are accessed in the app using the BuildConfig class.

```language=java
private Database(Context context) {
    super(context, DATABASE_NAME, null, BuildConfig.DB_VERSION);
}
```

### Build Types

You can also change the gradle values depending on build target. As shown in the sample file above, there are different `SERVER_URL`s for release and debug build targets. This can be great for having separate dev, staging and production urls - just build the app in the corresponding build type and its all gravy!

### Build Flavours

*Not listed, will touch on it soon*
