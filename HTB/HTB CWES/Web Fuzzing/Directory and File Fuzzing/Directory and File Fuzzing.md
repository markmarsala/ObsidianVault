Wordlists: SecLists (https://github.com/danielmiessler/SecLists)

Fuzzing web directories and files:
- Discovery/Web-Content/common.txt
- Discovery/Web-Content/directory-list-2.3-medium.txt
- Discovery/Web-Content/raft-large-directories.txt
- Discovery/Web-Content/big.txt

## Directory Fuzzing
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```

## File Fuzzing
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://94.237.58.98:49358/webfuzzing_hidden_path/FUZZ -e .php,.html,.txt,.bak,.js -v 
```

