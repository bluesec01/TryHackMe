# Pickle Rick

## Data

IP address: `10.10.246.178`

Web site: https://10-10-246-178.p.thmlabs.com/

## NMAP

```
# Nmap 7.92 scan initiated Fri Jul  1 15:05:23 2022 as: nmap -sV -oN nmap.txt 10.10.246.178
Nmap scan report for 10.10.246.178
Host is up (0.032s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Jul  1 15:05:32 2022 -- 1 IP address (1 host up) scanned in 8.66 seconds
```

- ssh is open
- http is open

## probing

### Inspecting html

Shortcut: `ctrl + U`

```
  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
```

Username is found!

### Feroxbuster

/assets is found

### Cookies

No cookies are used

### Javascript

No javascript is used

### Hydra

[First time used]

Hydra is a tool that can be used to bruteforce an ssh connection
--> We NEED to have the ssh key

### Nikto

Another enumerator

robots.txt is found
-> `Wubbalubbadubdub`

login.php is found

username & password is known
Username: `R1ckRul3s`
Password: `Wubbalubbadubdub`



