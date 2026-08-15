**After completing all steps in the assessment, you will be presented with a page that contains a flag in the format of HTB{...}. What is that flag?
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://94.237.52.208:35044/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://94.237.52.208:35044/admin/FUZZ -e .php
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://94.237.52.208:35044/admin/panel.php?accessID=FUZZ -fs 58
sudo nano /etc/hosts
94.237.52.208 fuzzing_fun.htb
gobuster vhost -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://fuzzing_fun.htb:35044/ --append-domain >> vhost.txt
cat vhost.txt | grep 200
sudo nano /etc/hosts
94.237.52.208 hidden.fuzzing_fun.htb
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://hidden.fuzzing_fun.htb:35044/godeep/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://hidden.fuzzing_fun.htb:35044/godeep/stoneedge/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://hidden.fuzzing_fun.htb:35044/godeep/stoneedge/bbclone/FUZZ
curl http://hidden.fuzzing_fun.htb:35044/godeep/stoneedge/bbclone/typo3/
```


Check different parameters (GET, PUT, POST) and filter by size.
```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://admin.academy.htb:30525/admin/admin.php -X POST -d "FUZZ=key" -H 'Content-Type: application/x-www-form-urlencoded' -fs 798
ffuf -w numbers.txt -u http://admin.academy.htb:30525/admin/admin.php -X POST -d "id=FUZZ" -H 'Content-Type: application/x-www-form-urlencoded' -fs 768
```


Check for subdomains first potentially
```
gobuster vhost -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://fuzzing_fun.htb:35044/ --append-domain >> vhost.txt
cat vhost.txt | grep 200
```

Check for extensions before pages
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt -u http://faculty.academy.htb:31833/indexFUZZ -c -ic -fs 985
```
