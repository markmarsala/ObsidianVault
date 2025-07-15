
Tasks
- Enumerate the internal network (172.16.5.0/23), identifying hosts, critical services, and potential avenues for a foothold
- This can include active and passive measures to identify users, hosts, and vulnerabilities we may be able to take advantage of to further our access.
- Document any findings we come across for later use. Extremely important!

Key Data Points
- AD users
	- Find valid users for password spraying
- AD Joined Computers
	- DC, file servers, SQL servers, web servers, Exchange mail servers, database servers, etc.
- Key Services
	- Kerberos, NetBIOS, LDAP, DNS
- Vulnerable Hosts and Services
	- Quick win to gain foothold


#### Identifying Hosts (passive)
- Wireshark (gui)
```shell-session
sudo -E wireshark
```
	- Look for ARP and MDNS packets (layer 2)
- [TCPDump](https://linux.die.net/man/8/tcpdump) (no gui)
- [net-creds](https://github.com/DanMcInerney/net-creds)
- [NetMiner](https://www.netminer.com/en/product/netminer.php)
- pktmon.exe
```shell-session
sudo tcpdump -i ens224 
```
- [Responder](https://github.com/lgandx/Responder-Windows) - listen, analyze, and poison LLMNR, NBT-NS, and MDNS requests and responses
```bash
sudo responder -I ens224 -A 
```
	- A means Analyze mode, no poison



#### Identifying Hosts (active)
- [fping](https://fping.org/)
```shell-session
fping -asgq 172.16.5.0/23
```
	- 'a' shows targets that are alive, 's' print stats at the end of the scan, 'g' to generate a target list from the CIDR network, and 'q' to not show per-target results
- nmap
```bash
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```
	- 'A' means aggressive scan which quickly enumerates well-known ports to include web services, domain services, etc.


#### Identifying Users
- [Kerbrute](https://github.com/ropnop/kerbrute) - internal AD username enumeration
	- Precompiled binaries: https://github.com/ropnop/kerbrute/releases/latest
- jsmith.txt and jsmith2.txt from [Insidetrust](https://github.com/insidetrust/statistically-likely-usernames)
	- user lists

**Compile Kerbrute binaries
```shell-session
sudo git clone https://github.com/ropnop/kerbrute.git
```
```shell-session
make help
```
- Lists compiling options
```shell-session
sudo make all
```
```shell-session
ls dist/
```
```shell-session
./kerbrute_linux_amd64 
```

**Adding the Tool to our Path (can type kerbrute from any location on the system and able to access it)
```shell-session
echo $PATH
```
```shell-session
sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```

**Enumerating Users with Kerbrute
```shell-session
kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```


#### Identifying Potential Vulnerabilities
To gain SYSTEM-level access (nearly the same as domain user):
- Remote Windows exploits such as MS08-067, EternalBlue, or BlueKeep
- Abusing a service running in the context of the SYSTEM account, or abusing the service account SeImpersonate privileges using [Juicy Potato](https://github.com/ohpe/juicy-potato). Not possible with Windows Server 2019.
- Local privilege escalation flaws (Windows 10 Task Scheduler 0-day)
- Gaining admin access on a domain-joined host with a lcoal account and using Psexec to launch a SYSTEM cmd window

**SYSTEM-level access on a domain-joined host allows:
- Enumerate the domain using built-in tools or offensive tools such as Bloodhound and PowerView.
- Perform Kerberoasting / ASREPRoasting attacks within the same domain.
- Run tools such as Inveigh to gather Net-NTLMv2 hashes or perform SMB relay attacks.
- Perform token impersonation to hijack a privileged domain user account.
- Carry out ACL attacks.
