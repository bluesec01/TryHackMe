# Bounty Hacker

## data

### IP

IP address: `10.10.112.50`

## Probing

### NMAP

```
# Nmap 7.92 scan initiated Sun Jun 12 00:43:23 2022 as: nmap -sV -oN nmap.txt 10.10.112.50
Nmap scan report for 10.10.112.50
Host is up (0.049s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun 12 00:43:36 2022 -- 1 IP address (1 host up) scanned in 13.08 seconds
```

### Feroxbuster

nothing

### FTP

The ftp server has anonymous login enabled