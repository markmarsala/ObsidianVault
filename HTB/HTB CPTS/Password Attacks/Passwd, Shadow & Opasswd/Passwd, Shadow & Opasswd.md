
PAM - Pluggable Authentication Modules
Located in /usr/lib/x86_x64-linux-gnu/security/

Passwd Format
login name : password info : UID : GUID : full name/comments : home directory : shell

```shell-session
head -n 1 /etc/passwd
```
Root without password:
```shell-session
root::0:0:root:/root:/bin/bash
```


Shadow File Format
username : encrypted password : last pw change : min pw age : max pw age : warning period : inactivity period : expiration date : unused

```shell-session
sudo cat /etc/shadow
```
If encrypted password field has ! or \* then Unix password is not allowed, but kerberos or key based is.

- `$1$` – MD5
- `$2a$` – Blowfish
- `$2y$` – Eksblowfish
- `$5$` – SHA-256
- `$6$` – SHA-512

```shell-session
sudo cat /etc/security/opasswd
```
Old passwords are stored in ^

#### Unshadow
```shell-session
sudo cp /etc/passwd /tmp/passwd.bak
```
```shell-session
sudo cp /etc/shadow /tmp/shadow.bak
```
```shell-session
unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```
```shell-session
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```


#### MD5 cracking
```shell-session
cat md5-hashes.list
```
```shell-session
hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```

