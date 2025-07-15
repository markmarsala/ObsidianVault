
```shell-session
apt install ffuf -y
```

ffuf -h
- '-w' for wordlists
- '-u' for URL
- '-t' for number of threads (200)

## Directory Fuzzing
```shell-session
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
