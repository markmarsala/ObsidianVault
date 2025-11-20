```
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://94.237.53.219:45888/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
```
