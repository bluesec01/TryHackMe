 # Glitch

 ## Data

 ### Description 

 This is a simple challenge in which you need to exploit a vulnerable web application and root the machine. It is beginner oriented, some basic JavaScript knowledge would be helpful, but not mandatory. Feedback is always appreciated.

--> There is no real info to be found in the description (except the javascript)

### IP

ip = `10.10.133.11`

## NMAP

```
# Nmap 7.92 scan initiated Sat Jun 11 16:53:01 2022 as: nmap -sV -oN nmap.txt 10.10.133.11
Nmap scan report for 10.10.133.11
Host is up (0.072s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jun 11 16:53:15 2022 -- 1 IP address (1 host up) scanned in 14.73 seconds
```

==> nginx server on port 80

## Probing

### JAvascript

It fetches from /api/access and logs the response

Running the function getAccess() in the console gave us this token `"dGhpc19pc19ub3RfcmVhbA=="`

This is a base64 string

Converting with

```

echo 'dGhpc19pc19ub3RfcmVhbA==' | base64 -d
this_is_not_real  
```

### Enumerating http

/secret/

### Cookies

The cookie `token` can be set to `this_is_not_real`

 [unfinished!!]
