---
title: "Compression Software Comparison"
date: 2021-10-09T14:53:44-05:00
draft: false
---

Critical: support of zlib, gzip and lzma.

I'd like to take the code and make it more robust with compressions.  If it runs into random data concatenated on the end of a segment, the ideal compressor would simply stop and return what it decompressed so far.  If it runs into another compressed segment concatenated on the end, it decompresses that segment too.  Being robust in the header checking would be nice too.

## OS X Tar
Supported formats: "tar, pax, cpio, zip, jar, ar, and ISO 9660", bzip2, compress, gzip, xz.

Details: It automatically detects the appropriate format.  '''How does it do this?'''

## XZ Utils
Supported formats: xz and lzma

## zlib
Supported formats: Presumably gzip and zlib, since deezee uses zlib to handle those.

## DeeZee
Overview: From the blackbag toolkit created by Matasano.  You give it a binary blob, it searches through it for compression file magic numbers.  It feeds any hits to the zlib library, which simply tries to decompress them.

Supported formats: Gzip and zlib formats. 

Details: It looks for magic numbers 0x789c and 0x1f8b by default, and I added numbers 0x7801 and 0x78da based on some info in a presentation.  The default magic numbers are more effective for the test set I'm using - my additions only yield chaff.

## Bzip2
Supported formats: bzip2

## GZip
Supported formats: gzip

