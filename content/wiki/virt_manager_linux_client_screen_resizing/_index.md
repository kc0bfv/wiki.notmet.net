---
title: "Virt Manager Linux Client Screen Resizing"
date: 2021-10-09T15:29:35-05:00
draft: false
---

I always have trouble getting screen resizing working after installing a Linux client VM.  Right now I'm using virt manager exclusively, on Debian.  Here's what worked with a Debian unstable client:

* Install spice-vdagent and qemu-guest-agent (probably only one is necessary)
* Make sure the VM has channel spice and channel qemu-ga - here's the XML

```
<channel type="spicevmc">
  <target type="virtio" name="com.redhat.spice.0" state="disconnected"/>
  <alias name="channel0"/>
  <address type="virtio-serial" controller="0" bus="0" port="2"/>
</channel>
```

```
<channel type="unix">
  <source mode="bind" path="/var/lib/libvirt/qemu/channel/target/domain-4-DebianVM/org.qemu.guest_agent.0"/>
  <target type="virtio" name="org.qemu.guest_agent.0" state="connected"/>
  <alias name="channel1"/>
  <address type="virtio-serial" controller="0" bus="0" port="1"/>
</channel>
```

* Make sure both agents can startup on reboot
* Resize screen with `xrandr --output Virtual-0 --auto`

