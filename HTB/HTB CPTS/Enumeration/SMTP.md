### SMTP
------------
TCP port 25 and 587 (TLS)
Sometimes TCP port 465 for encrypted connection

Mail User Agent (MUA) - stores header and body into SMTP server
Mail Transfer Agent (MTA) - software basis for sending and receiving emails
Mail Submission Agent (MSA) / Relay server - checks validity of the origin of the email
Mail Delivery Agent
Mailbox (POP3/IMAP)

Open Relay Attack - spoofing. Some misconfigured SMTP servers can send fake emails.

MUA -> MSA -> MTA -> MDA -> Mailbox

Commands:
- AUTH PLAIN
- HELO
- MAIL FROM
- RCPT TO
- DATA
- RSET
- VRFY
- EXPN
- NOOP
- QUIT

Connecting:
*telnet 10.129.14.128 25*

nmap scripts:
- smtp-commands
- smtp-open-relay