
UDP/53
TCP/53

```shell-session
nmap -p53 -Pn -sV -sC 10.10.110.213
```

#### DNS Zone Transfer

```shell-session
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```
```shell-session
fierce --domain inlanefreight.htb
```

Domain takeover: registering a non-existent domain name to gain control over another domain. If attackers find an expired domain, they can claim that domain to perform further attacks such as hosting malicious content on a website or sending a phishing email leveraging the claimed domain.

Subdomain takeover:
CNAME is used to point a subdomain to parent domain
```shell-session
sub.target.com.   60   IN   CNAME   anotherdomain.com
```
If anotherdomain.com has expired and is claimed by attacker, they have total control over sub.target.com until updated

#### Subdomain Enumeration
```shell-session
./subfinder -d inlanefreight.com -v  
```

Subbrute
```shell-session
git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
```
```shell-session
cd subbrute
```
```shell-session
echo "ns1.inlanefreight.com" > ./resolvers.txt
```
```shell-session
./subbrute inlanefreight.com -s ./names.txt -r ./resolvers.txt
```

Enumerate CNAME records:
```shell-session
 host support.inlanefreight.com
```

Check if subdomain takeover is possible:
https://github.com/EdOverflow/can-i-take-over-xyz



#### DNS Spoofing / DNS Cache Poisoning

first, edit /etc/ettercap/etter.dns
```shell-session
cat /etc/ettercap/etter.dns

inlanefreight.com      A   192.168.225.110
*.inlanefreight.com    A   192.168.225.110
```
target domain name                  attacker's IP address

1. Start ettercap
2. Navigate to Hosts > Scan for Hosts
3. Add the target IP address to Target1 (192.168.152.129)
4. Add a default gateway IP to Target 2 (192.168.152.2)
5. Navigate to Plugins > Manage Plugins
6. Activate dns_spoof
