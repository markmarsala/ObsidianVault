
**Inveigh
- Located in C:\Tools
- Powershell version is no longer updated
```powershell-session
Import-Module .\Inveigh.ps1
```
```powershell-session
(Get-Command Invoke-Inveigh).Parameters
```
```powershell-session
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

**C# Inveigh
```powershell-session
.\Inveigh.exe
```
- ESC to enter/exit interactive consol
- HELP
- GET NTLMV2UNIQUE
- GET NTLMV2USERNAMES
- STOP


#### Remediation
- Disable LLMNR
	- Make sure to tell clients to test changes slowly before rolling it out fully
	- Computer Configuration -> Administrative Templates -> Network -> DNS Client and enabling "Turn OFF Multicast Name Resolution"
- Disable NBT-NS
	- Must be disabled locally on each host
	- Control Panel -> Network and Sharing Center -> Change adapter settings -> Right click on adapter -> (TCP/IPv4) -> Properties -> Advanced -> WINS -> Disable NetBIOS over TCP/IP
- Disable NBT-NS using PowerShell Script
	- Create a PowerShell script under Computer Configuration -> Windows Settings -> Script (Startup/Shutdown) -> Startup 
```powershell
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
	- Local Group Policy Editor -> Startup -> Powershell Scripts tab -> "For this GPO, run scripts in the following order" to Run Windows PowerShell scripts first -> Add and choose script -> reboot system
- To push to all hosts in domain, Group Policy Management on DC -> host script on the SYSVOL share in the scripts folder and then call it via its UNC path such as: \\\inlanefreight.local\SYSVOL\INLANEFREIGHT.LOCAL\scripts
- Block LLMNR/NetBIOS traffic
- Enabling SMB Signing to prevent NTLM relay attacks.
- Network segmentation if LLMNR or NetBIOS must be enabled
- IDS/IPS


#### Detection
https://www.praetorian.com/blog/a-simple-and-effective-way-to-detect-broadcast-name-resolution-poisoning-bnrp/
- Hosts can be monitored for traffic on ports UDP 5355 and 137, and event IDs 4697 and 7045
- Monitor registry key HKLM\Software\Policies\Microsoft\Windows NT\DNSClient for changes to the EnableMulticast DWORD value. A value of 0 would mean that LLMNR is disabled.

