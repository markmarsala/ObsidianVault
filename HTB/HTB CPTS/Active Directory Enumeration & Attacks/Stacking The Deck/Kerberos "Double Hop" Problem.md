Kerberos: we get a signed ticket from the KDC that permits us to access the requested resource

Occurs when using WinRM, 'Enter-PSSession' for 2 different machines

**Problem
When we use Kerberos to establish a remote session, we are not using a password for authentication. When password authentication is used, with PSExec, for example, that NTLM hash is stored in the session, so when we go to access another resource, the machine can pull the hash from memory and authenticate us.

Attack host -> DEV01 -> DC01
- If we use win-rm to access DEV01 and use powerview (which needs to query the DC), it will not work since it is network authentication, therefore Kerberos will not give us ticket to access resources in the domain.
- The TGS ticket is sent to the remote service, allowing command execution, but the TGT is not sent.
- If unconstrainted delegation is enabled, then the TGT ticket will be sent and we can access resources.


## Workaround #1: PSCredential Object
```
$SecPassword = ConvertTo-SecureString '!qazXSW@' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\backupadm', $SecPassword)
import-module .\PowerView.ps1
get-domainuser -spn -credential $Cred | select samaccountname
```
- Must use '-credential' flag

RDP will not cause problem because the password is stored in memory, but Win-RM has the double hop problem.


## Workaround #2: Register PSSession Configuration
```
Enter-PSSession -ComputerName ACADEMY-AEN-DEV01.INLANEFREIGHT.LOCAL -Credential inlanefreight\backupadm
klist
```
- We're only allowed access to HTTP on the target since we connected over WinRM

**Register a new session configuration
```
Register-PSSessionConfiguration -Name backupadmsess -RunAsCredential inlanefreight\backupadm
Restart-Service WinRM
Enter-PSSession -ComputerName DEV01 -Credential INLANEFREIGHT\backupadm -ConfigurationName  backupadmsess
get-domainuser -spn | select samaccountname
```

