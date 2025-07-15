
/usr/share/wordlists/rockyou.txt

#### Linux

Shadow File - only read by root
```shell-session
cat /etc/shadow
```

Format: \$id\$salt\$hash

IDs
- \$1$             MD5
- \$2a$          Blowfish
- \$5$           SHA-256
- \$6$          SHA-512
- \$sha1$    SHA1crypt
- \$y$            Yescrypt
- \$gy$          Gost-yescrypt
- \$7$         Scrypt

Passwd File - read by any user
```shell-session
cat /etc/passwd
```


#### Windows

LSA - Local Security Authority
- Authenticates users and logs them into the local computer
- Translates names and security IDs

![[Pasted image 20250305085506.png]]

Credential providers are COM objects located in DLLs

LSASS - Local Security Authority Subsystem Service
- Local system security policy
- User authentication
- Sending security audit logs to Event log

![[Pasted image 20250305085919.png]]

SAM - Security Account Manager
- Stores password in LM or NTLM hash
- C:\system32\config\SAM
- SYSTEM level permissions are required to view it

A computer can be assigned to either a workgroup or a domain.
- If workgroup -> SAM
- If domain -> DC must validate with ntds.dit in c:\ntds.dit

syskey.exe
- Partially encrypts SAM file hard disk copy to prevent offline password cracking


Credential Locker:
```powershell-session
C:\Users\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\
```


#### NTDS
- Computer authenticates with DC

Each DC has a file NTDS.dit that is kept synchronized across all DC. Database that stores:
- User accounts
- Group accounts
- Computer accounts
- Group policy objects