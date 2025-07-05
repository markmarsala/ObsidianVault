
Port forwarding: technique that allows us to redirect a communication request from one port to another

**Executing a local port forward
```shell-session
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
- the -L command tells the SSH client to request the SSH server to forward all the data we send via the port 1234 to localhost:3306 on the ubuntu server.

**Confirming Port Forward with Netstat
```shell-session
netstat -antp | grep 1234
```

**Confirming Port Forward with Nmap
```shell-session
nmap -v -sV -p1234 localhost
```

**Forwarding Multiple Ports
```shell-session
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```

**Looking for Opportunities to Pivot using ifconfig
```shell-session
ifconfig
```
- One connected to our attack host (ens192)
- One communicating to other hosts within a different network (ens224)
- The loopback interface (lo)

SSH tunneling over SOCKS proxy:
In order to scan IPs on a network that we do not have access to, we must start a SOCKS listener on our local host and then configure SSH to forward that traffic via SSH to the network (172.16.5.0/23) after connecting to the target host.

This can bypass firewall restrictions.
It can also be used for pivoting via creating a route to an external server from NAT networks.

**Enabling Dynamic Port Forwarding with SSH
```shell-session
ssh -D 9050 ubuntu@10.129.202.64
```
- -D means dynamic port forwarding
- We must use proxychains to route packets over port 9050

**Proxychains** forces TCP traffic to go through SOCKS4/SOCKS5, TOR, or HTTP/HTTPS proxies.

To inform proxychains that we must use port 9050, we must modify the proxychains configuration file located at **/etc/proxychains.conf.** We can add socks4 127.0.0.1 9050 to the last line if it is not already there.

**Using Nmap with Proxychains - SOCKS tunneling
```shell-session
proxychains nmap -v -sn 172.16.5.1-200
```
- Can only perform a full TCP connect scan over proxychains
- Host-alive checks may not work against Windows target due to Windows Defender blocking ICMP requests

**Enumerating the Windows Target through Proxychains
```shell-session
proxychains nmap -v -Pn -sT 172.16.5.19
```

**Using Metasploit with Proxychains
```shell-session
proxychains msfconsole
```
```shell-session
search rdp_scanner
```
```shell-session
use 0
```
```shell-session
set rhosts 172.16.5.19
```
```shell-session
run
```

**Using xfreerdp with Proxychains
```shell-session
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
- We just pivoted to a Windows machine via the Ubuntu server in the network!






