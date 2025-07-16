## Installation

```shell-session
sudo apt-get -y update
sudo apt-get -y install medusa
```
```shell-session
medusa [target_options] [credential_options] -M module [module_options]
```

## Flags
-h HOST or -H FILE
-u USERNAME or -U FILE
-p PASSWORD or -P FILE
-M MODULE
  - ssh
  - http
-m "MODULE_OPTION"
  - "POST /login.php HTTP/1.1\r\nContent-Length: 30\r\nContent-Type: application/x-www-form-urlencoded\r\n\r\nusername=^USER^&password=^PASS^"
-t TASKS (threads)
-f or -F (fast mode)
-n PORT
-v LEVEL (verbose)

## Modules
- FTP
  - medusa -M ftp -h 192.168.1.100 -u admin -P passwords.txt
- HTTP
  - medusa -M http -h www.example.com -U users.txt -P passwords.txt -m DIR:/login.php -m FORM:username=^USER^&password=^PASS^
- IMAP
  - medusa -M imap -h mail.example.com -U users.txt -P passwords.txt
- MySQL
  - medusa -M mysql -h 192.168.1.100 -u root -P passwords.txt
- POP3
  - medusa -M pop3 -h mail.example.com -U users.txt -P passwords.txt
- RDP
  - medusa -M rdp -h 192.168.1.100 -u admin -P passwords.txt
- SSHv2
  - medusa -M ssh -h 192.168.1.100 -u root -P passwords.txt  
- Subversion (SVN)
  - medusa -M svn -h 192.168.1.100 -u admin -P password.txt
- Telnet
  - medusa -M telnet -h 192.168.1.100 -u admin -P passwords.txt
- VNC
  - medusa -M vnc -h 192.168.1.100 -P passwords.txt
- Web Form
  - medusa -M web-form -h www.example.com -U users.txt -P passwords.txt -m FORM:"username=^USER^&password=^PASS^:F=Invalid"

## Targeting an SSH Server
```shell-session
medusa -h 192.168.0.100 -U usernames.txt -P passwords.txt -M ssh
```

## Targeting Multiple Web Servers with Basic HTTP Authentication
```shell-session
medusa -H web_servers.txt -U usernames.txt -P passwords.txt -M http -m GET
```

## Testing for Empty or Default Passwords
```shell-session
medusa -h 10.0.0.5 -U suernames.txt -e ns -M service_name
```
- Perform additional checks for empty passwords (-e n) and passwords matching the username (-e s)
