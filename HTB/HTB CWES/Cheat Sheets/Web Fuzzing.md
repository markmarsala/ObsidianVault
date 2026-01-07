- Hidden directories or files
- Insecure APIs and endpoints
- SQL injection points
- XSS vulnerabilities
- Command injection flaws

## Misc

**Add DNS entry to /etc/hosts
```
sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'
```

**Brute-forcing numerical IDs
```
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```

**Form submissions or API call
```
curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'
```

## Wordlists

**General
```
/usr/share/seclists/Discovery/Web-Content/common.txt
```

**Directory-Focused
```
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

**Large Directory
```
/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
```

**Comprehensive
```
/usr/share/seclists/Discovery/Web-Content/big.txt
```

## ffuf 
```
ffuf -u http://example.com/FUZZ
ffuf -u http://example.com/FUZZ -w wordlist.txt
ffuf -u http://example.com/FUZZ -w wordlist.txt -ic
ffuf -u http://example.com/FUZZ -w wordlist.txt -c
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200
ffuf -u http://example.com/FUZZ -w wordlist.txt -mr "Welcome"
ffuf -u http://example.com/FUZZ -w wordlist.txt -e .php,.html
ffuf -u http://example.com/FUZZ -w wordlist.txt -t 50
ffuf -u http://example.com/FUZZ -w wordlist.txt -x http://127.0.0.1:8080
```
- u (url)
- w (wordlist)
- ic (ignore comments)
- c (colorize)
- mc 200 (Match status code)
- mr "Welcome" (Match regex)
- e .php,.html (add extensions to each wordlist entry)
- t (threads)
- x http://127.0.0.1:8080 (use proxy)

## gobuster
```
gobuster dir -u http://example.com -w wordlist.txt
gobuster dir -u http://example.com -w wordlist.txt -x .php,.html
gobuster dir -u http://example.com -w wordlist.txt -s 200
gobuster dir -u http://example.com -w wordlist.txt -t 50
gobuster dir -u http://example.com -w wordlist.txt -o results.txt
gobuster dns -d example.com -w subdomains.txt
gobuster dns -d example.com -w subdomains.txt -i
gobuster dns -d example.com -w subdomains.txt -z
```
- dir (directory)
- s (match status code)
- x (specific extensions)
- o (output to file)
- d (fuzz DNS subdomains)
- i (show IP addresses)
- z (silent mode, suppress output except for results)

## wenum
```
wenum -c -w wordlist.txt --hc 404 -u http://example.com/FUZZ
wenum -c -w wordlist.txt -d 'username=FUZZ&password=secret' -u http://example.com/login
wenum -c -w wordlist.txt -b 'session=12345' -u http://example.com/FUZZ
wenum -c -w wordlist.txt -H 'User-Agent: Wenum' -u http://example.com/FUZZ
wenum -c -w wordlist.txt -t 50 -u http://example.com/FUZZ
wenum -c -w wordlist.txt -X PUT -u http://example.com/FUZZ
wenum -c -w wordlist.txt --hs 50 -u http://example.com/FUZZ
```
- hc (exclude status code)
- d (POST data)
- n (use cookie)
- H (custom header)
- t (threads)
- X (specific HTTP method)
- hs (filter my content length)

## Feroxbuster
```
feroxbuster -u http://example.com -w wordlist.txt
feroxbuster -u http://example.com -w wordlist.txt -e
feroxbuster -u http://example.com -w wordlist.txt -x 404
feroxbuster -u http://example.com -w wordlist.txt -t 50
feroxbuster -u http://example.com -w wordlist.txt --depth 3
feroxbuster -u http://example.com -w wordlist.txt -o results.txt
feroxbuster -u http://example.com -w wordlist.txt --no-recursion
feroxbuster -u http://example.com -w wordlist.txt --url-redirect
```
- e (include specified file extensions)
- x (exclude responses with status code)
- t (threads)
- depth (recursion depth)
- o (output)
- no-recursion
- url-redirect (follow redirects automatically)


