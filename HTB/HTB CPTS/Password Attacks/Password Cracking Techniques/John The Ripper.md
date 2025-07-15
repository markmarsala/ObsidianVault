
Single Crack Mode
```shell-session
john --format=<hash_type> <hash or hash_file>
```
```shell-session
john --format=sha256 hashes_to_crack.txt
```

![[Pasted image 20250305091706.png]]
![[Pasted image 20250305091728.png]]
![[Pasted image 20250305091803.png]]
![[Pasted image 20250305091824.png]]
![[Pasted image 20250305091843.png]]
![[Pasted image 20250305091912.png]]

Wordlist Mode
```shell-session
john --wordlist=<wordlist_file> --rules <hash_file>
```

Incremental Mode
```shell-session
john --incremental <hash_file>
```


#### Cracking Files

```shell-session
pdf2john server_doc.pdf > server_doc.hash
```
```shell-session
john --wordlist=<wordlist.txt> server_doc.hash 
```

Tools
- pdf2john
- ssh2john
- mscash2john
- keychain2john
- rar2john
- pfx2john
- truecrypt_volume2john
- keepass2john
- vncpcap2john
- putty2john
- zip2john
- hccap2john
- office2john
- wpa2john

```shell-session
locate *2john*
```

