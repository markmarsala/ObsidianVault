**Add Vhost to hosts file
```
echo "10.20.20.34 inlanefreight.htb" | sudo tee -a /etc/hosts
```

## Gobuster Vhost fuzzing
```
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

## Gobuster Subdomain Fuzzing
```
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
