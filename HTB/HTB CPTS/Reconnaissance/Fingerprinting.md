##### Fingerprinting

- Banner grabbing
```shell-session
curl -I inlanefreight.com
```
- Analyzing HTTP headers
- X-Powered-By
- Probing for specific responses
- Analyzing page content

Tools:
- wappalyzer
- BuiltWith
- WhatWeb
- Nmap
- Netcraft
- wafw00f
```shell-session
pip3 install git+https://github.com/EnableSecurity/wafw00f
```
```shell-session
wafw00f inlanefreight.com
```
- Nikto
```shell-session
sudo apt update && sudo apt install -y perl
```
```shell-session
git clone https://github.com/sullo/nikto
```
```shell-session
cd nikto/program
```
```shell-session
chmod +x ./nikto.pl
```
Only run software identification modules:
```shell-session
nikto -h inlanefreight.com -Tuning b
```

