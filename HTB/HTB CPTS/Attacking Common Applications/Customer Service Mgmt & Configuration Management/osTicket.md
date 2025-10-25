## Footprinting/Discovery/Enumeration

Cookie name = OSTSESSID

"powered by osTicket"

Footer may contain "Support Ticket System"

Social Engineer by causing a problem and calling the Help Desk to gain usernames or emails.


## Attacking osTicket

https://nvd.nist.gov/vuln/detail/CVE-2020-24881

We can submit a ticket, and it may give us a valid email to use in the environment.

In this example, we sent a ticket and it gave us the email: 940288@inlanefreight.local. we can use this email to register an account and receive a confirmation email through osTicket.
- Wiki
- Chat service (Slack, Mattermost, Rocket.chat)
- Git (GitLab, Bitbucket)


## osTicket - Sensitive Data Exposure

Dehashed: https://github.com/sm00v/Dehashed
linkedin2username: https://github.com/initstring/linkedin2username
```
sudo python3 dehashed.py -q inlanefreight.local -p
```
