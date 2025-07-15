Hybrid Attacks work by predicting human behavior and using a targeted wordlist.

For instance, if a user has the password Summer2023, and must change it, then they will most likely change it to Summer2024 or Summer2023!

What to put into wordlists
- Common Passwords
- Industry-specific terms
- Password Change Policy
- Personal info
- Initial reconnaissance

Given this policy:
- Minimum of 8 characters
- At least on uppercase letter
- At least one number

```shell-session
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/darkweb2017_top-10000.txt
```
```shell-session
grep -E '^.{8,}$' darkweb2017-top10000.txt > darkweb2017-minlength.txt
```
```shell-session
grep -E '[A-Z]' darkweb2017-minlength.txt > darkweb2017-uppercase.txt
```
```shell-session
grep -E '[a-z]' darkweb2017-uppercase.txt > darkweb2017-lowercase.txt
```
```shell-session
grep -E '[0-9]' darkweb2017-lowercase.txt > darkweb2017-number.txt
```

## Credential Stuffing
Sources of credentials:
- Data Breaches
- Phishing Scams
- Malware
- Public Wordlists
Attacker:
- Acquiring credentials
- Identifying targets
- Automated testing
- Unauthorized access
Consequences:
- Data theft
- Identity fraud
- Financial crimes
- Further attacks

Password reuse is bad :(
