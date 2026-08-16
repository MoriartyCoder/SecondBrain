## Overall
To access a SMB share you need *smbclient*.
```
smbclient //[IP]/[SHARE] [OPTIONS]
```
### Options:
* -U \<USER\>:  specify the user
* -p \<PORT\>: specify the port

## Enumeration
To find out whether a Samba share exists or not you can perform a simple nmap scan on the IP.
If you find one then prepare enum4linux.