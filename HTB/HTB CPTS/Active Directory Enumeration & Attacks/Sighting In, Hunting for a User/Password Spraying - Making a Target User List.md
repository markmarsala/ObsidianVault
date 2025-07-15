
- SMB NULL session
- LDAP anonymous bind
- Kerbrute + wordlist ([statically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames), [linkedin2username](https://github.com/initstring/linkedin2username))
- Credentials provided by client, LLMNR/NBT-NS response poisoning, password spray

Log:
- The accounts targeted
- Domain Controller used in the attack
- Time of the spray
- Date of the spray
- Password(s) attempted

**SMB NULL Session to Pull User List (domain user account or SYSTEM level access)
- enum4linux
```shell-session
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```
- rpcclient
```shell-session
rpcclient -U "" -N 172.16.5.5
```
- CrackMapExec
```shell-session
crackmapexec smb 172.16.5.5 --users
```


**LDAP Anonymous
- ldapsearch
```shell-session
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```
- windapsearch
```shell-session
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```


**Kerbrute
```shell-session
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```
	- This generates event ID 4768: A Kerberos authentication ticket (TGT) was requested. This will only be triggered if Kerberos event logging is enabled via group policy. This is a great recommendation to client.


**Credentialed enumeration
```shell-session
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```


