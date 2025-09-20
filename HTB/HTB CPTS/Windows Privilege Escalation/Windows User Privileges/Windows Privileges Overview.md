Subject = User's Access Token (User SID, Group SIDs, Privileges, Extra Access Information) -> Object = Object's Security Descriptor (Object Owner SID, Group SID, SACL, DACL)
1. System performs access check
2. Examines each ACE until a match is found
3. Access decision is made

**Right and Privileges in Windows
Groups
- Default Administrators
- Server Operators
- Backup Operators
- Print Operators
- Hyper-V Administrators
- Account Operators
- Remote Desktop Users
- Remote Management Users
- Group Policy Creator Owners
- Schema Admins
- DNS Admins

**User Rights Assignment
- SeNetworkLogonRight = access this computer from the network
- SeRemoteInteractiveLogonRight = allow log on through remote desktop services
- SeBackupPrivilege = back up files and directories
- SeSecurityPrivilege = manage auditing and security log
- SeTakeOwnershipPrivilege = take ownership of files or other objects
- SeDebugPrivilege = debug programs
- SeImpersonatePrivilege = impersonate a client after authentication
- SeLoadDriverPrivilege = load and unload device drivers
- SeRestorePrivilege = restore files and directories
- SeTcbPrivilege = act as part of the operating system

**Local Admin User Rights - Elevated
```
whoami /priv
```
- Run as elevated to see all the privileges. Standard users will have far less. Disabled means the privilege is applied and needs to be enabled.
- Backup operators have SeShutdownPrivilege, which allows them to shut down a domain controller locally (massive disruption of service).

Script to enable certain privileges: https://www.powershellgallery.com/packages/PoshPrivilege/0.3.0.0/Content/Scripts%5CEnable-Privilege.ps1
Script to adjust token privileges: https://www.leeholmes.com/adjusting-token-privileges-in-powershell/

## Detection
https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4672

