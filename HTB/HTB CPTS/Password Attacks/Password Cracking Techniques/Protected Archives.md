
Types of archived files: https://fileinfo.com/filetypes/compressed
- tar
- vmdb/vmx
- kdbx
- pkg
- gz
- cpt
- luks
- rpm
- rar
- truecrypt
- deb
- war
- zip
- bitlocker
- 7z
- gzip


Download all file extensions
```shell-session
curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```

Cracking ZIP
```shell-session
zip2john ZIP.zip > zip.hash
```
```shell-session
cat zip.hash
```

Cracking the Hash with John
```shell-session
john --wordlist=rockyou.txt zip.hash
```

Viewing the cracked hash
```shell-session
john zip.hash --show
```

##### Cracking OpenSSL Encrypted Archives
```shell-session
file GZIP.gzip 
```
```shell-session
for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```

##### Cracking BitLocker Encrypted Drives
Using bitlocker2john
```shell-session
bitlocker2john -i Backup.vhd > backup.hashes
```
```shell-session
grep "bitlocker\$0" backup.hashes > backup.hash
```
```shell-session
cat backup.hash
```
```shell-session
hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked
```
```shell-session
cat backup.cracked 
```

Windows - Mounting BitLocker VHD

Mount backup on a windows machine and double click to prompt password



