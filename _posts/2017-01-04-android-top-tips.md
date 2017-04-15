---
layout: post
title:  Android Top Tips
date:   2013-11-10 10:18:00
categories: Android
---

values/strings.xml
``` language=xml
<string name="title">Test Title #%d</string>
```

layout/main.xml
``` language=xml
android:id="@+id/titleTextView"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:text="Sample Activity #%d" />
```

Java code
``` language=java
title.setText(String.format(getString(R.string.title), 1)));
```
