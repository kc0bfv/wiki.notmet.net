---
title: "Ripping from the Laptop CD Drive"
date: 2022-10-16T12:00:00-05:00
draft: false
---

# My DVDs

When I need to rip something from the laptop's CD drive onto the desktop or elsewhere, try the `gddrescue` package (with command `ddrescue`):

`ddrescue -n /dev/cdrom whatever.iso`

For a DVD you might need to prepend `lsdvd /dev/cdrom && `.

## Network Block Device Server and Client

You might also try nbd-server.  This is significantly more complicated.

`apt install nbd-server nbd-client`

On the server side setup a config.

/etc/nbd-server/config:

```
[generic]
    #user = nbd  # comment out to use root for cd drive
    #group = nbd # comment out to use root for cd drive
    includedir = /etc/nbd-server/conf.d
    listenaddr = 127.0.0.1
    port = 10809 # not needed, but makes the default clear
```

/etc/nbd-server/conf.d/cddrive.conf

```
[cddrive]
    exportname=/dev/cdrom
```

Now `service nbd-server restart` and you'll see it at TCP port 10809.

On the client side use SSH to forward it to your destination host.  Run `sudo modprobe nbd` then `sudo nbd-client 127.0.0.1 -N cddrive /dev/nbd0`.  Now this will only allow root to connect to /dev/nbd0, but that is a link to the remote cd drive.

# My CDs

These rip well using `CDDA_DEVICE=/dev/cdrom cdda2mp3`.  Make a directory to rip into, cd there, then run that.

Combine them together using something like ```ffmpeg -i "concat:file1.mp3|file2.mp3" -acodec copy output.mp3```.  You might generate that list of files with `for i in */*.mp3; do echo -n "$i|"; done > file` then use `cat file` instead of the filenames.
