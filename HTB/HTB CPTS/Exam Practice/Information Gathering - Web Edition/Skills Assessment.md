**What is the IANA ID of the registrar of the inlanefreight.com domain?
```
whois.com
```
- IANA ID: 468

**What http server software is powering the inlanefreight.htb site on the target system? Respond with the name of the software, not the version, e.g., Apache.
```
sudo nano /etc/hosts
curl -vvv "http://inlanefreight.htb:45094"
```
- add IP to hosts

**What is the API key in the hidden admin directory that you have discovered on the target system?
```
gobuster vhost -u http://inlanefreight.htb:45094 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 300
gobuster dir -u http://web1337.inlanefreight.htb:45094/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 300
curl http://web1337.inlanefreight.htb:45094/robots.txt
curl http://web1337.inlanefreight.htb:45094/admin_h1dd3n/
```
- disallow: /admin_h1dd3n

**After crawling the inlanefreight.htb domain on the target system, what is the email address you have found? Respond with the full email, e.g., mail@inlanefreight.htb.
```
gobuster vhost -u http://web1337.inlanefreight.htb:45094 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt --append-domain -t 300
pip3 install scrapy
wget -O RecondSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip RecondSpider.zip
python3 ReconSpider.py http://dev.web1337.inlanefreight.htb:45094/
cat results.json
```

**What is the API key the inlanefreight.htb developers will be changing too?
```
cat results.json
```
