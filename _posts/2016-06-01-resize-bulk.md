---
layout: post
title:  Resize pngs by bulk
date:   2016-06-01 02:47:21
categories: Random
---

http://forum.linuxcareer.com/threads/1644-Batch-image-resize-using-linux-command-line

http://davidmalin.blogspot.com.au/2007/08/batch-resize-and-compress-your-images.html

mogrify -resize 50% -quality 75 *.png

mogrify -resize 30% -quality 75 *.png

mogrify -resize 144x192 *.png
Width x Height
