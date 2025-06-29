### Enumeration
-----------------
1.	There is more than meets the eye. Consider all points of view.
2.	Distinguish between what we see and what we do not see.
3.	There are always ways to gain more information. Understand the target.


1. Infrastructure Based Enumeration
	1. Internet Presence: identification of internet presence and externally accessible infrastructure
		1. Security Measures
		2. Cloud Instances
		3. IP Addresses
		4. Netblocks
		5. ASN
		6. vHosts
		7. Subdomains
		8. Domains
	2. Gateway: identify the possible security measures to protect the company's external and internal infrastructure
		1. Cloudflare
		2. VPN
		3. Network Segmentation
		4. NAC
		5. Proxies
		6. EDR
		7. IDS/IPS
		8. DMZ
		9. Firewalls
2. Host Based Enumeration
	1. Accessible Services: identify accessible interfaces and services that are hosted externally and internally
		1. Interface
		2. Version
		3. Port
		4. Configuration
		5. Functionality
		6. Service Type
	2. Processes: identify the internal processes, sources, and destinations associated with the services
		1. Destination
		2. Source
		3. Tasks
		4. Proceed Data
		5. PID
3. OS Based Enumeration
	1. Privileges: identification of the internal permissions and privileges to the accessible services
		1. Environment
		2. Restrictions
		3. Permissions
		4. Users
		5. Groups
	2. OS Setup: identification of the internal components and systems setup
		6. Sensitive Private Files
		7. Configuration Files
		8. OS Environment
		9. Network Config
		10. Patch Level
		11. OS Type


#### Infrastructure Based Enumeration
-----------------------------------
SSL certificate
- May include more than just a subdomain
- Often used for several domains

crt.sh
- Certificate Transparency logs
- curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .
- Unique subdomains: curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u

Company hosted servers: for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done

Shodan - IP List
- for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
- for i in $(cat ip-addresses.txt);do shodan host $i;done

dig
- dig any inlanefreight.com

**A** records: IP addresses that point to a specific (sub)domain
**MX** records: mail server
**NS** records: show which name servers are used to resolve the FQDN to IP addresses
**TXT** records:  SPF, DMARC, and DKIM


#### Cloud Resources
--------------------
Amazon (AWS) : S3 buckets
Google (GCP) : cloud storage
Microsoft (Azure) : blobs

Google Dorks search
- S3 buckets: intext:[company]inurl:amazonaws.com
- Azure blob: intext:[company]inurl:blob.core.windows.net

*domain.glass*

*GrayHatWarfare*

A company may use abbreviations of the company name.

Check employee profiles for information on cloud services, programming languages, GitHub, etc.