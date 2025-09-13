## Network Information

**Interace(s), IP Address(es), DNS Information
```
ipconfig /all
```

**ARP Table
```
arp -a
```

**Routing Table
```
route print
```

## Enumerating Protections

GetAppLockerPolicy cmdlet to enumerate the local, effective (enforced), and domain AppLocker policies.
- Help see what binaries or file types may be blocked and whether we will have to perform some sort of AppLocker bypass.

**Check Windows Defender Status
```
Get-MpComputerStatus
```

**List AppLocker Rules
```
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

**Test AppLocker Policy
```
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```
