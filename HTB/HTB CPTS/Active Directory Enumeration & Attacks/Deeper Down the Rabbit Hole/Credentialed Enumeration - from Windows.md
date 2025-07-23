## ActiveDirectory PowerShell Module

```shell-session
Get-Module
```
- If the module is not loaded, run
```shell-session
Import-Module ActiveDirectory
```

**Get Domain Info
```shell-session
Get-ADDomain
```

**Get-ADUser
```shell-session
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
- accounts with the ServicePrincipalName property populated may be susceptible to a kerberoasting attack

**Checking For Trust Relationships
```shell-session
Get-ADTrust -Filter *
```

**Group Enumeration
```shell-session
Get-ADGroup -Filter * | select name
```

**Detailed Group Info
```shell-session
Get-ADGroup -Identity "Backup Operators"
```

**Group Membership
```shell-session
Get-ADGroupMember -Identity "Backup Operators"
```

## PowerView
Commands
- Export-PowerViewCSV
- ConvertTo-SID
- Get-DomainSPNTicket
- Get-Domain
- Get-DomainController
- Get-DomainUser
- Get-DomainComputer
- Get-DomainGroup
- Get-DomainOU
- Find-InterestingDomainAcl
- Get-DomainGroupMember
- Get-DomainFileServer
- Get-DomainDFSShare
- Get-DomainGPO
- Get-DomainPolicy
- Get-NetLocalGroup
- Get-NetLocalGroupMember
- Get-NetShare
- Get-Netsession
- Test-AdminAccess
- Find-DomainUserLocation
- Find-DomainShare
- Find-InterestingDomainShareFile
- Find-LocalAdminAccess
- Get-DomainTrust
- Get-ForestTrust
- Get-DomainForeignUser
- Get-DomainForeignGroupMember
- Get-DomainTrustMapping

**Domain User Information
```shell-session
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```

**Recursive Group Membership
```shell-session
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

**Trust Enumeration
```shell-session
Get-DomainTrustMapping
```

**Testing for Local Admin Access
```shell-session
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```

**Finding Users With SPN (ServicePrincipalName) Set (subject to Kerberoasting)
```shell-session
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

## SharpView
```shell-session
.\SharpView.exe Get-DomainUser -Help
```
```shell-session
.\SharpView.exe Get-DomainUser -Identity forend
```

## Snaffler (acquire credentials and sensitive data in AD)
```shell-session
Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
.\Snaffler.exe  -d INLANEFREIGHT.LOCAL -s -v data
```
- Helps find passwords, ssh keys, configuration files, etc.

## SharpHound
```shell-session
.\SharpHound.exe --help
```
```shell-session
.\SharpHound.exe -c All --zipfilename ILFREIGHT
```
Custom cipher queries: https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/

**Note: always record transferring files and where. You want to delete all files after engagement is over.
