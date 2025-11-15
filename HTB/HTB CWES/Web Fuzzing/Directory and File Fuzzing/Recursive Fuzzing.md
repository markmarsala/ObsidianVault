```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion
```

Flags:
- recursion-depth
- rate
- timeout

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -u http://83.136.254.84:36629/recursive_fuzz/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```
