##### DNS
1. Your computer asks for directions (DNS query)
2. The DNS resolver checks its map (recursive lookup)
3. Root name server points the way
4. TLD name server narrows it down
5. Authoritative name server delivers the address
6. The DNS resolver returns the information
7. Your computer connects

Hosts file:
- Redirecting a domain to a local server for development
	- 127.0.0.1        myapp.local
- Testing Connectivity by specifying an IP address
	- 129.168.1.20     testserver.local
- Blocking unwanted websites by redirecting their domains to a non-existent IP address
	- 0.0.0.0      unwanted-site.com

DNS tools:
- dig
	- dig domain.com
	- dig domain.com A
	- dig domain.com AAAA
	- dig domain.com MX
	- dig domain.com NS
	- dig domain.com TXT
	- dig domain.com CNAME
	- dig domain.com SOA
	- dig @1.1.1.1 domain.com (specific nameserver to query)
	- dig +trace domain.com (full path of DNS resolution)
	- dig -x 192.168.1.1 (reverse lookup)
	- dig +short domain.com (concise)
	- dig +noall +answer domain.com
	- dig domain.com ANY
- nslookup
- host
- dnsenum
- fierce
- dnsrecon
- theHarvester

Subdomains:
- Development and staging environments
- Hidden login portals
- Legacy Applications
- Sensitive Information

8. Active Subdomain Enumeration
	1. DNS zone transfer
	2. Brute-force enumeration
		1. dnsenum
		2. ffuf
		3. gobuster
9. Passive Subdomain Enumeration
	1. Certificate Transparency (CT) logs
	2. Search engines (site:)

##### Subdomain Bruteforcing
10. Wordlist Selection
11. Iteration and Querying
12. DNS Lookup
13. Filtering and Validation

Subdomain brute-force tools:
- dnsenum
	- DNS record enumeration
	- Zone transfer attempts
	- Subdomain Brute-Forcing
	- Google Scraping
	- Reverse Lookup
	- WHOIS Lookups
```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
```
- fierce
- dnsrecon
- amass
- assetfinder
- puredns


##### DNS Zone Transfers

Vulnerability: access controls governing who can initiate a zone transfer

Fix: allow zone transfers only to trusted secondary servers, ensuring that sensitive zone data remains confidential.

```shell-session
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

