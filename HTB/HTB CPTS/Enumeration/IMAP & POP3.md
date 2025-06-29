### IMAP / POP3
---------
Internet Message Access Protocol - access to emails from a mail server is possible
- TCP port 143 or 993 (TLS)
- allows online management of emails directly on the server 
- supports folder structures

Commands:
- LOGIN username password
- LIST "" *
- CREATE "INBOX"
- DELETE "INBOX"
- RENAME "ToRead" "Important"
- LSUB "" *
- SELECT INBOX
- UNSELECT INBOX
- FETCH [ID] all RFC822
- CLOSE
- LOGOUT
(may need to put 'a' in front of all commands)

Connect:
```shell-session
curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v
```
```shell-session
openssl s_client -connect 10.129.14.128:imaps
```

Post Office Protocol
- TCP port 110 and 995 (TLS)
- only provides listing, retrieving, and deleting emails as functions at the email server

Commands:
- USER username
- PASS password
- STAT
- LIST
- RETR id
- DELE id
- CAPA
- RSET
- QUIT

Connect:
```shell-session
openssl s_client -connect 10.129.14.128:pop3s
```

