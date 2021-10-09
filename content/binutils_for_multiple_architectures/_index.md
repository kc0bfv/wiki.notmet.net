---
title: "Binutils For Multiple Architectures"
date: 2021-10-09T14:56:01-05:00
draft: false
---

To configure binutils to handle multiple architectures at the same time, check out the file ''bfd/config.bfd'' for a list of all supported architectures.  This file does pattern matching against what you put on the command line in ''--enable-targets''.  The patterns start on line 122 for binutils 2.22, with the line ''case "${targ}" in''.  I modified my binutils homebrew formula to look like this:

    system "./configure", "--disable-debug",
                          "--disable-dependency-tracking",
                          "--program-prefix=g",
                          "--prefix=#{prefix}",
                          "--infodir=#{info}",
                          "--mandir=#{man}",
                          "--disable-werror",
                          "--enable-interwork",
                          "--enable-multilib",
                          "--enable-targets=x86_64-elf,arm-none-eabi,m32r,powerpc-elf,arm-elf,avr-elf,m68k-elf"
