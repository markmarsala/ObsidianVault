Escalate to:
- NT AUTHORITY\SYSTEM
- administrator account (local)
- Local account part of Administrators group (same privs as administrator account)
- Standard domain user who is part of the local Administrators group
- A domain admin that is part of the local Administrators group

Windows Commands:
https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands
- OS name
- Version
- Running Services

## System Information

```
tasklist /svc
```

Standard Windows Processes:
- smss.exe
- csrss.exe
- winlogon.exe
- lsass.exe
- svchost.exe
- MsMpEng.exe (Windows Defender)

## Display All Environment Variables
```
set
```
- PATH (leftmost takes priority), can be used for dll injection
- If a file is placed in USEPROFILE\AppData\Microsoft\Windows\Start Menu\Programs\Startup, when the user logs into a different machine, this file will execute.

## View Detailed Configuration Information
```
systeminfo
```
- Shows if box has been patched recently and if it is a VM

## Patches and Updates
```
wmic qfe
Get-HotFix | ft -AutoSize
```

## Installed Programs
```
wmic product get name
Get-WmiObject -Class Win32_Product | select Name, Version
```

## Display Running Processes
```
netstat -ano
```

## User & Group Information

**Logged-In Users
```
query user
```

**Current User
```
echo %USERNAME%
```

**Current User Privileges
```
whoami /priv
```

**Current User Group Information
```
whoami /groups
```

**Get All Users
```
net user
```

**Get All Groups
```
net localgroup
```

**Details About a Group
```
net localgroup administrators
```

**Get Password Policy & Other Account Information
```
net accounts
```

Cheat sheet: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md
