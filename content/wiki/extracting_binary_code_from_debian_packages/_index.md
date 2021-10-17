---
title: "Extracting Binary Code From Debian Packages"
date: 2021-10-09T14:51:44-05:00
draft: false
---

For my thesis I needed a corpus of binary data from several architectures.  Fortunately these architectures all have Debian ports (though some are old and incomplete, they'll probably do), so I decided to grab the code from these Debian ports.  As Linux distros, the Debian ports use ELF binary file format, so after finding repositories and downloading the deb files within, I needed to extract the code.  This is how I did it.

## Extracting Deb Packages
Wikipedia has a great page about the [deb file format](http://en.wikipedia.org/wiki/Deb_(file_format)).

Basically, deb packages are ar archives, so use `ar` to unpackage them.  Then extract the `data.tar.*` file (several compression formats, or none, are possible in there).  I used `tar -C directoryname` to extract the data archive into a specific directory.

## Extracting Binary Code
Since it's extracted into a specific directory, I unleash a recursive function on it which uses the `objcopy` method on [StackOverflow](http://stackoverflow.com/questions/3925075/how-do-you-extract-only-the-contents-of-an-elf-section).  I extracted sections .init, .fini, .text and .plt because at least on ARM they are supposed to have executable code.  I concatenated all the sections to just one binary file, then successfully disassembled it in IdaPro (as a proof of method).

