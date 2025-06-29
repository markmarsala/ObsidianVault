##### Crawling

Discovers links from a page, queues them up, then continues in a cycle.

- Breadth-first crawling
- Depth-first crawling

Extracting valuable information:
- Internal and external links
- Comments
- Metadata
- Sensitive files (backup, config, log, API keys, creds, keys, code)

##### robots.txt

This file tells bots not to scrape the following directories.

1. User-agent
2. Directive
	1. Disallow
	2. Allow
	3. Crawl-delay
	4. Sitemap

Respecting robots.txt:
- Avoiding overburdening servers
- Protecting sensitive information
- Legal and ethical compliance

robots.txt in web recon
- Uncovering hidden directories
- Mapping website structure
- Detecting crawler traps (intentionally include honeypot directories)

##### Well-Known URIs

the '.well-known' directory in the main directory stores important files for users.

To access a website's security policy, a client would request https://example.com/.well-known/security.txt.

URIs:
- security.txt
- /.well-known/change-password
- openid-configuration
- assetlinks.json
- mta-sts.txt (SMTP security enhancement)
- https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml

openid-configuration
1. Endpoint discovery
	1. Authorization endpoint
	2. Token endpoint
	3. Userinfo endpoint
2. JWKS URI
3. Supported scopes and response types
4. Algorithm details


##### Popular Web Crawlers
1. Burp Suite Spider
2. OWASP ZAP (Zed Attack Proxy)
3. Scrapy (Python Framework)
4. Apache Nutch (Scalable Crawler)

Scrapy:
```shell-session
pip3 install scrapy
```
```shell-session
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
```
```shell-session
unzip ReconSpider.zip
```
```shell-session
python3 ReconSpider.py http://inlanefreight.com
```

Extracted contents:
- emails
- links
- external_files
- js_files
- form_fields
- images
- videos
- audio
- comments