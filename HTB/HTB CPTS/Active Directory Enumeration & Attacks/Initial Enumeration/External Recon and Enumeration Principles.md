
What to look for:
- IP Space
	- Valid ASN, cloud presence, hosting providers, DNS record entries
- Domain Information
	- Administer, subdomains, publicly accessible domain services (mailservers, DNS, Websites, VPN portals), defenses (SIEM, AV, IPS/IDS)
- Schema Format
	- email accounts, AD usernames, password policies (used for password spraying, credential stuffing, brute forcing)
- Data Disclosures
	- pdf, ppt, docx, xlsx, intranet site listings, user metadata, shares, GitHub repo, internal AD username format in the metadata of a PDF
- Breach Data
	- Publicly release usernames, passwords, etc.


Where to look:
- ASN / IP registrars
	- [IANA](https://www.iana.org/), [arin](https://www.arin.net/) (Americas), [RIPE](https://www.ripe.net/) (Europe), [BGP Toolkit](https://bgp.he.net/)
- Domain Registrars & DNS
	- [Domaintools](https://www.domaintools.com/), viewdns.info, [PTRArchive](http://ptrarchive.com/), [ICANN](https://lookup.icann.org/lookup), manual DNS record requests against domain in question or well known DNS servers, such as 8.8.8.8
- Social Media
	- LinkedIn, Twitter, Facebook, news articles, Indeed, Glassdoor
- Public-Facing Company Websites
	- News Articles, embedded documents, "About Us," "Contact Us"
- Cloud & Dev Storage Spaces
	- [GitHub](https://github.com/), AWS S3 buckets & Azure Blog storage containers, [Google searches using "Dorks](https://www.exploit-db.com/google-hacking-database)," [Trufflehog](https://github.com/trufflesecurity/truffleHog), [Greyhat Warfare](https://buckets.grayhatwarfare.com/)
- Breach Data Sources
	- [HaveIBeenPwned](https://haveibeenpwned.com/), [Dehashed](https://www.dehashed.com/)


Example Process:
- Use [BGP toolkit](http://he.net/)
	- It found an IP address, a mail server, and two nameservers
- Validate IP address with [viewdns.info](https://viewdns.info/)
```shell-session
nslookup ns1.inlanefreight.com
```
- Google search reveals one document and email addresses
	- filetype:pdf inurl:inlanefreight.com
	- intext:"@inlanefreight.com" inurl:inlanefreight.com
- Browse contact page for emails
	- first.last@inlanefreight.com is the format
- Username harvesting with [linkedin2username](https://github.com/initstring/linkedin2username)
	- create mashup of usernames to use for spraying
- [Dehashed](http://dehashed.com/) hunting for cleartext credentials and password hashes
```shell-session
sudo python3 dehashed.py -q inlanefreight.local -p
```
- Script: https://github.com/mrb3n813/Pentest-stuff/blob/master/dehashed.py
- Script #2: https://github.com/sm00v/Dehashed
- 
