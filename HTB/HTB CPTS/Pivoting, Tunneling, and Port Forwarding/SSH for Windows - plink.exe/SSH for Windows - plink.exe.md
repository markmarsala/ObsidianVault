
Plink, short for PuTTY Link, is a Windows command-line SSH that comes as part of the PuTTY package when installed

![[Pasted image 20250626171255.png]]

**Using Plink.exe
```cmd-session
plink -ssh -D 9050 ubuntu@10.129.15.50
```

- use Proxifier to start a SOCKS tunnel
- IP is 127.0.0.1 and port 9050