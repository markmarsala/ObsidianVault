If auditing of process creation events is enable, then event ID 4688: A new process has been creted will be saved.

The tools can then flag potentially malicious activity, such as whoami, netstat, and tasklist.

Initial access commands: tasklist, ver, ipconfig, systeminfo
Reconnaissance: dir, net view, ping, net use, type
Spreading malware: at, reg, wmic, wusa

**Confirming Group Membership
```
net localgroup "Event Log Readers"
```

**Searching Security Logs Using wevtutil
```
wevtutil qe Security /rd:true /f:text | Select-String "/user"
```

**Passing Credentials to wevtutil
```
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
```

**Searching Security Logs Using Get-WinEvent
```
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*'} | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```
- Note: Searching the Security event log with Get-WInEvent requires administrator access or permissions adjusted on the registry key HKLM\System\CurrentControlSet\Services\Eventlog\Security. Membership in just the Event Log Readers group is not sufficient.
- cmdlet can also be run as another user with the -Credential parameter
