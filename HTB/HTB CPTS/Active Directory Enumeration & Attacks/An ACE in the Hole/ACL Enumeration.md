## Enumerating ACLs with PowerView

Given a user wley, which we have control over
```
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid wley
```

**Using Get-DomainObjectACL
```
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```

Run in new powershell session:
```
$guid= "00299570-246d-11d0-a768-00aa006e0529"
Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```

Using the -ResolveGUIDs Flag
```
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} 
```

**Creating a List of Domain Users
```
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```

**Useful foreach loop
```
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```
- We can now take over damundsen using wley's User-Force-Change-Password extended right.

**Further Enumeration of RIghts Using damundsen
```
$sid2 = Convert-NameToSid damundsen
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```
- damundsen has GenericWrite privileges over the Help Desk Level 1 group, therefore we can add a member.

**Investigating the Help Desk Level 1 Group with Get-DomainGroup
```
Get-DomainGroup -Identity "Help Desk Level 1" | select memberof
```
- Help Desk Level 1 group is a part of Information Technology group, so they inherit the privileges that the Information Technology group has.


To recap:
- We have control over the user 'wley' whose hash we retrieved earlier using Responder and cracked offline using Hashcat to reveal the cleartext password value.
- We enumerated objects that the user 'wley' has control over and found that we could force change the password of the user 'damundsen.'
- From here, we found that the 'damundsen' user can add a member to the Help Desk Level 1 group using GenericWrite privileges
- The Help Desk Level 1 group is nested into the Information Technology group, which grants members of that group any rights provisioned to the Information Technology group

**Investigating the Information Technology Group
```
$itgroupsid = Convert-NameToSid "Information Technology"
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose
```
- Members of IT group have GenericAll rights over the user 'adunn,' so we can modify group membership, force change a password, and perform a targeted Kerberoasting attack and attempt to crack the user's password if it is weak

**Looking for Interesting Access for adunn
```
$adunnsid = Convert-NameToSid adunn
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $adunnsid} -Verbose
```
- DS-Replication-Get-Changes is set, so we can perform a DCSync attack


## Enumerating ACLs with BloodHound
1. Select WLEY@INLANEFREIGHT.LOCAL as the starting node
2. Select Node Info tab
3. Select Outbound Control Rights
4. Select First Degree Object Control
5. Right-click on line between and press Help to get info around abusing this ACE
6. Transitive Object Control show path wley -> damundsen -> help desk level 1 -> IT -> adunn
7. Select adunn as node > analysis tab > DSync
