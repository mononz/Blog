---
layout: post
title:  Stealing databases!
date:   2016-05-16 04:25:53
categories: Android
---

In my Paragon application 'Paradex', I have implemented a fairly background sync procedure to keep various sqlite database tables up to date.
It is somewhat important to have the latest data set available to users who download your application and may not have an internet connection for an update at that time.

#### What do I do?

I load up my application on a rooted Android device or on a Genymotion emulator (which is pre-rooted yay!).
Once the sync procedures have finished and the app database is up to date, I steal it!

#### Thieving process

Fire up your command line, making sure you have adb installed with

```language-bash
$ adb version
Android Debug Bridge version 1.0.32
Revision 09a0d98bebce-android
```

Now lets hack!

```language-bash
$ adb shell
# su
# cp /data/data/com.example.app/databases/app.db /sdcard/app.db
# exit
# exit
$ adb pull /sdcard/app.db ./Desktop/
1030 KB/s (67584 bytes in 0.064s)
```

Remember to substitute your package name and database name accordingly.
You should now have your app.db file sitting on your desktop ready to overwrite the database currently in your projects assets directory.

Remember to update the database version number in your code, especially if the schema has changed!!

I use this process frequently so I hope this is helpful for others too :)
