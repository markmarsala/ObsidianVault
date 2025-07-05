
For Windows environment where SSH is not usable

DVC - Dynamic Virtual Channels are responsible for tunneling packets over the RDP connection

https://github.com/nccgroup/SocksOverRDP/releases

https://www.proxifier.com/download/#win-tab
- We can look for ProxifierPE.zip

Connect to target using xfreerdp and copy the SocksOverRDPx64.zip file to the target.

**Loading SocksOverRDP.dll using regsvr32.exe
```cmd-session
regsvr32.exe SocksOverRDP-Plugin.dll
```

Now we can connect to 172.16.5.19 over RDP using mstsc.exe with credentials victor:pass@123

We will need to transfer SocksOverRDPx64.zip or just SocksOverRDP-Server.exe to 172.16.5.19. We can then start SocksOverRDP-Server.exe with Admin privileges.

**Confirming the SOCKS Listener is Started (pivot host)
```cmd-session
netstat -antb | findstr 1080
```


Next, transfer proxifier to the pivot host.

Proxifier -> Profile -> Proxy Server -> Address: 127.0.0.1, port 1080, SOCKS5

Then, we can run remote desktop connection and connect to 172.16.6.155 with credentials jason:WellConnected123!
#### RDP Performance Considerations

If the performance is slow, go to Experience in Remote Desktop Connection (mstsc.exe) and set performance to modem.

