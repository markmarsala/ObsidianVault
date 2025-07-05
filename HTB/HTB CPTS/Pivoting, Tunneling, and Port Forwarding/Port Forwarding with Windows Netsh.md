
What if our pivot host is a Windows machine? Use netsh

![[Pasted image 20250628125517.png]]

Netsh is used for
- Finding routes
- Viewing the firewall configuration
- Adding proxies
- Creating port forwarding rules

**Using Netsh.exe to Port Forward
```cmd-session
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.69.130 connectport=3389 connectaddress=172.16.5.19
```

**Verifying Port Forward
```cmd-session
netsh.exe interface portproxy show v4tov4
```

After configuring the `portproxy` on our Windows-based pivot host, we will try to connect to the 8080 port of this host from our attack host using xfreerdp. Once a request is sent from our attack host, the Windows host will route our traffic according to the proxy settings configured by netsh.exe.

```cmd-session
xfreerdp /v:10.129.69.130:8080 /u:victor /p:'pass@123'
```

