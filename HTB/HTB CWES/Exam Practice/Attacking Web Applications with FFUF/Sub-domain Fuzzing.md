```
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.inlanefreight.com/

ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u https://FUZZ.inlanefreight.com/ -ic -c
```
