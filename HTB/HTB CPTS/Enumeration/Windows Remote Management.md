### Windows Remote Management Protocols
---------
- Remote Desktop Protocol (RDP)
- Windows Remote Management (WinRM)
- Windows Management Instrumentation (WMI)


#### RDP
TCP/UDP port 3389

```shell-session
sudo cpan
```
```shell-session
cpan install Encoding::BER
```
```shell-session
git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check
```

Connect:
- xfreerdp
```shell-session
xfreerdp /u:cry0l1t3 /p:"P455w0rd!" /v:10.129.201.248
```
- rdesktop
- Remmina


#### WinRM
TCP ports 5985 (HTTP) and 5986 (HTTPS)

Use in conjunction with WinRS to execute commands remotely

Tools:
- Test-WsMan
- evil-winrm
```shell-session
evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!
```



#### WMI - Windows Management Instrumentation
TCP port 135, moves to random port after establishment

Accessed via
- PowerShell
- VBScript
- Windows Management Instrumentation Console (WMIC)

```shell-session
/usr/share/doc/python3-impacket/examples/wmiexec.py Cry0l1t3:"P455w0rD!"@10.129.201.248 "hostname"
```

