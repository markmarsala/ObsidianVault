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
- SSHv2
- Subversion (SVN)
- Telnet
- VNC
- Web Form
