
AD provides authentication, accounting, and authorization functions within a Windows enterprise environment

Tools:
- Sysinternals
- WMI
- DNS
- Responder
- Kerbrute
- Bloodhound

Attacks:
- Password spraying
- Kerberoasting


Goals:
- Escalate privileges
- Domain compromise
- Access specific host, email inbox, database, etc.

```shell-session
xfreerdp /v:<MS01 target IP> /u:htb-student /p:Academy_student_AD!
```

```shell-session
ssh htb-student@<ATTACK01 target IP>
```

```shell-session
xfreerdp /v:<ATTACK01 target IP> /u:htb-student /p:HTB_@cademy_stdnt!
```


Tools:

MS01 - Windows machine
- C:\Tools

ATTACK01 - Linux (Parrot) machine
- /opt

