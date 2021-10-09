---
title: "Booting Coreboot"
date: 2021-10-09T15:09:06-05:00
draft: false
---

I was interested in building Coreboot, which provides an open source BIOS.  It's pretty popular for embedded stuff - I just wanted to build an x86 version so I could play with it in QEMU.

## Building Coreboot

Install prerequisites.  I'm using Debian because, duh.

    apt-get install git build-essential gnat flex bison libncurses5-dev wget zlib1g-dev

Clone the coreboot repo

    git clone https://review.coreboot.org/coreboot
    cd coreboot
    git submodule update --init --checkout

Startup the menu configuration

    make nconfig

Change the settings you want to change, but here are the ones I made sure were set:

    Mainboard -> Mainboard Vendor -> Emulation
    Mainboard -> Mainboard Model -> QEMU x86 i440fx/piix4...

Those are the defaults though :-).  Now I'm trying again with PS/2 Keyboard Init in Generic Drivers

You've got to have a payload - use SeaBIOS because it's the default and it's fine.  Still, go in this configuration item to see all the cool stuff you might use as a payload.  But, leave it as the default.  Don't screw this up.

    Payload -> Add a Payload

Ok, that's enough config

Make the toolchain - coreboot uses its own gcc toolchain...

    make crossgcc-i386

At one point I had some problems in this part, building BINUTILS.  Specifically, it wanted -fPIC for some of the tools.  I think I solved this by cleaning parts the build and building components with:

    make crossgcc-i386 CFLAGS=-fPIC CXXFLAGS=-fPIC

However, I just tried the build-from-scratch again and didn't have to use this form at all.

Now you can build coreboot

    make

The result that you need is for coreboot.rom to be in build/

## Building a Linux Disk Image

Setup a 10 GB sparse disk file

    dd if=/dev/zero of=disk.img bs=1024 count=1 seek=10239k

Give it a partition that's bootable

    parted -s disk.img -- mklabel msdos mkpart primary 1m 10g toggle 1 boot

Give it a loopback device

    sudo losetup -f disk.img

Tell linux to look for the partition on the loopback device

    sudo partprobe /dev/loop0

Create a filesystem on the loopback device partition

    sudo mkfs.ext2 /dev/loop0p1

Create a place to mount the partition

    mkdir tmp

Mount the partition

    sudo mount /dev/loop0p1 tmp

Install a useful base of the Debian system.  Oh yeah - you are using Debian, right?  This will be for i386, obviously.

    sudo debootstrap --include=linux-image-686,grub-pc --arch i386 stable ./tmp
    sudo debootstrap --arch i386 stable ./tmp # This version doesn't include a kernel or grub... and you need those.  Go figure.

Use grub-install to install the boot loader

    sudo grub-install --boot-directory=./tmp/boot --modules=part_msdos /dev/loop0

Mount some things within tmp so we can chroot in

    sudo mount --bind /proc ./tmp/proc && sudo mount --bind /sys ./tmp/sys && sudo mount --bind /dev ./tmp/dev

Setup the grub config

    sudo chroot ./tmp grub-mkconfig -o /boot/grub/grub.cfg

Set a root password

    sudo chroot ./tmp passwd

I had some trouble using the system within QEMU as it is - all the keys on the keyboard seemed like they were mapped incorrectly.  I decided to redirect a console to a serial port.  One way to do this is to get grub in on the action.  Edit tmp/etc/default/grub to change existing lines, or add, so that the following config is present:

    GRUB_CMDLINE_LINUX='console=tty0 console=ttyS0,19200n8'
    GRUB_TERMINAL=serial
    GRUB_SERIAL_COMMAND="serial --speed=19200 --unit=0 --word=8 --parity=no --stop=1"

However, now that I figured out to add "-k en-us" to the qemu command-line, as shown below, I no longer have keyboard problems.

Now:

    sudo chroot ./tmp update-grub

Prepare to unmount things - skipping this step screwed up my host system by, I believe, unmounting these things within the parent

    sudo mount --make-unbindable tmp/proc && sudo mount --make-unbindable tmp/sys && sudo mount --make-unbindable tmp/dev

Unmount things

    sudo umount tmp/proc && sudo umount tmp/sys && sudo umount tmp/dev
    sudo umount tmp
    sudo losetup -d /dev/loop0

## Starting it Up

Now startup qemu with:

    qemu-system-i386 -drive file=disk.img,format=raw -bios coreboot/build/coreboot.rom -serial telnet::4444,server -k en-us

That's pretty self explanatory based on what we've done so far, but the -serial part says to emulate a serial port over telnet on TCP port 4444.  It'll wait for you to telnet to it before it continues running the server.  The -k en-us part specifies keyboard mapping and fixes the keyboard issue I had been having, thus making the serial part less necessary.

    telnet localhost 4444

You'll get GRUB and everything over that socket.

## References

https://www.coreboot.org/QEMU_Build_Tutorial The official Coreboot QEMU build tutorial - This covers the bulk of what I've done above, and defers to the general build tutorial linked below, but I had to make several tweaks to their procedures when it came to making and booting an image.

https://www.coreboot.org/Build_HOWTO The official Coreboot build tutorial -1 The above link defers to this one when it comes to building Coreboot.

I had to use each of these together to figure out how to build that bootable image.  http://www.olafdietsche.de/2016/03/24/debootstrap-bootable-image https://myles.sh/debootstrap-a-standalone-debian-system-across-multiple-partitions/ https://wiki.debian.org/Debootstrap

https://www.cyberciti.biz/faq/howto-setup-serial-console-on-debian-linux/ provided the info on how to get GRUB over serial easily

