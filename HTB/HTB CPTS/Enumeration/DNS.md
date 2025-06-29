### DNS - Domain Name System
-------------
TCP or UDP port 53
Unencrypted except
- DNS over TLS (DoT)
- DNS over HTTPS (DoH)
- DNSCrypt

- DNS Root Server
- Authoritative Nameserver - hold authority for a particular zone, binding
- Non-authoritative Nameserver
- Caching DNS Server
- Forwarding Server
- Resolver


DNS Records

A                          Returns an IPv4 address of the requested domain as a result
AAAA                   Returns an IPv6 address of the requested domain
MX                       Returns the responsible mail servers as a result
NS                        Returns the DNS servers (nameservers) of the domain
TXT                      SPF, DMARC, SSL validation, etc.
CNAME                Serves as an alias for another domain name
PTR                      Converts IP addresses into valid domain names
SOA                     Provides information about the corresponding DNS zone and email                                       address of the administrative contact

Bind9 - Linux DNS server
Configs (/etc/bind/): named.conf.local, named.conf.options, named.conf.log

FQDN - Fully Qualified Domain Name
- has reverse lookup file (IPs to domain names using PTR record)

DIG - NS Query
```shell-session
dig ns inlanefreight.htb @10.129.14.128
```

DIG - Version Query
```shell-session
dig CH TXT version.bind 10.129.120.85
```

DIG - ANY Query
```shell-session
dig any inlanefreight.htb @10.129.14.128
```

DIG - AXFR Zone Transfer
AXFR - Asynchronous Full Transfer Zone
```shell-session
dig axfr inlanefreight.htb @10.129.14.128
```

DIG - AXFR Zone Transfer - Internal
```shell-session
dig axfr internal.inlanefreight.htb @10.129.14.128
```

Subdomain Brute Forcing - bash for loop
```shell-session
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```

Subdomain Brute Forcing - DNSenum
```shell-session
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```

