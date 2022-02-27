---
title: "Removing Image Metadata"
date: 2022-02-27T12:00:00-05:00
draft: false
---

To view image metadata from the Linux command line:

`exiftool *.jpg`

To remove metadata:

`mogrify -strip img.jpg`

To remove it in Android "ExifEraser" works well.