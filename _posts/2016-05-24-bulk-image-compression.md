---
layout: post
title:  Bulk Image Compression
date:   2016-05-24 00:07:19
categories: Random
---

Need to resize or compress images in bulk often? I would generally use https://tinypng.com/ but with a max of 20 images at a time, it can still be a painful task manipulating >500 images.

As usual, I got bored one night and looked for a bulk compressor for primarily png images. I found https://pngquant.org/

After installing, one simple command allowed me to compress an entire folder of png images and overwrite its original file.

```language=bash
  $ pngquant *.png -f --ext .png
```

pngquant is very flexible and there are many more options to resize, compress, specify output folder etc.
I highly suggest reading up on the docs to find out more.
