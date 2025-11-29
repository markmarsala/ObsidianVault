**What is the IANA ID of the registrar of the inlanefreight.com domain?
whois.com search for inlanefreight.com

**What http server software is powering the inlanefreight.htb site on the target system? Respond with the name of the software, not the version, e.g., Apache.
Add vhost to /etc/hosts
```
curl -v http://inlanefreight.htb:35480
```

**What is the API key in the hidden admin directory that you have discovered on the target system?
```
gobuster vhost -u http://inlanefreight.htb:35480 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
ffuf -w /usr/share/dirb/wordlists/common.txt -u http://web1337.inlanefreight.htb:43113/FUZZ
curl http://web1337.inlanefreight.htb:43113/robots.txt
curl http://web1337.inlanefreight.htb:43113/admin_h1dd3n/
```

**After crawling the inlanefreight.htb domain on the target system, what is the email address you have found? Respond with the full email, e.g., mail@inlanefreight.htb.
```
pip3 install scrapy
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip ReconSpider.zip
python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:43113/
```

**What is the API key the inlanefreight.htb developers will be changing too?
```
python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:43113/
```
