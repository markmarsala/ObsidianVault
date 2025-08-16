We can utilize PHP Wrappers to extend LFI to remote code execution (php://)

## Input Filters
php://filter/
- resource
- read

4 different types of filters:
- String Filters
- Conversion Filters
- Compression Filters
- Encryption Filters

Useful filters:
- convert.base64-encode


## Fuzzing for PHP Files
```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```
- Looking for HTTP response codes 200, 301, 302, and 403
- Spider to find all referenced PHP files


## Standard PHP Inclusion

