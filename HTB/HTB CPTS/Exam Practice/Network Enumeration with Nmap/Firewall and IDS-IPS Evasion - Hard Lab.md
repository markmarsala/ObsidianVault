**Now our client wants to know if it is possible to find out the version of the running services. Identify the version of service our client was talking about and submit the flag as the answer.
```
sudo nmap -Pn --min-rate=500 --max-rtt-timeout 300ms --source-port 53 -p- 10.129.2.47
sudo nmap -Pn --min-rate=500 --max-rtt-timeout 300ms --source-port 53 -sV -sC -p 50000 10.129.2.47
sudo nc -nv -p53 10.129.2.47 50000
```
- n disable dns resolution
- v verbose command
- p port number from which to connect (IDS allows 53)
