### Linux Remote Management Protocols
----------
#### SSH - TCP port 22

SSH-1 is vulnerable to MITM attacks
- Banner: SSH-1.??-OpenSSH_3.9p1 (can use both SSH-1 and SSH-2)
SSH-2 is not

Authentication:
- Password
- Public-key
- Host-based
- Keyboard
- Challenge-response
- GSSAPI

Config:
- /etc/ssh/sshd_config (server)

Version 7.2p1 of OpenSSH is vulnerable to command injection


Footprinting:
```shell-session
git clone https://github.com/jtesta/ssh-audit.git
```


We can specify the type of authentication. For instance, password enables us to brute-force.
```shell-session
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```




#### Rsync - TCP port 873

Often used for backups and mirroring since it only sends the differences between the source files and the older version of the files that reside on the destination server.

Probing for accessible shares:
```shell-session
nc -nv 127.0.0.1 873
```

Enumerating an open share:
```shell-session
rsync -av --list-only rsync://127.0.0.1/dev
```

Rsync over ssh: https://phoenixnap.com/kb/how-to-rsync-over-ssh

#### R-Services

TCP ports 512, 512, and 514
Unencrypted

R-commands suite:
- rcp (remote copy)
- rexec (remote execution)
- rlogin (remote login)
- rsh (remote shell)
- rstat
- ruptime
- rwho (remote who)
- rusers

![[Pasted image 20250126114330.png]]

/etc/hosts.equiv contain trusted hosts, meaning, if one of these hosts attempt to access the system, they are granted access without authentication

```shell-session
[!bash!]$ cat /etc/hosts.equiv

# <hostname> <local username>
pwnbox cry0l1t3
```

**Note:** The `hosts.equiv` file is recognized as the global configuration regarding all users on a system, whereas `.rhosts` provides a per-user configuration.

```shell-session
[!bash!]$ cat .rhosts

htb-student     10.0.17.5
+               10.0.17.10
+               +
```

'+' specifies a wildcard, can be anything

```shell-session
rlogin 10.0.17.2 -l htb-student
```
```shell-session
rwho
```
```shell-session
rusers -al 10.0.17.5
```
User rlogin, then rwho, then rusers


