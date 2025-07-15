
Hunting for Files
```shell-session
for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

Hunting for SSH Keys
```shell-session
grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```


AES-128-CBC can be cracked

Crack with John
```shell-session
locate *2john*
```
```shell-session
ssh2john.py SSH.private > ssh.hash
```
```shell-session
cat ssh.hash
```
```shell-session
john --wordlist=rockyou.txt ssh.hash
```
```shell-session
john ssh.hash --show
```

Cracking Microsoft Office Documents
```shell-session
office2john.py Protected.docx > protected-docx.hash
```
```shell-session
cat protected-docx.hash
```
```shell-session
john --wordlist=rockyou.txt protected-docx.hash
```
```shell-session
john protected-docx.hash --show
```

Cracking PDFs
```shell-session
pdf2john.py PDF.pdf > pdf.hash
```
```shell-session
cat pdf.hash
```
```shell-session
john --wordlist=rockyou.txt pdf.hash
```
```shell-session
john pdf.hash --show
```



