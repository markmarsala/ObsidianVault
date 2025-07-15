Once we fuzz for parameters anf ind some, we can fuzz for values to complete the puzzle

For instance, if the parameter is id, then we can fuzz numbers from 1 to 1000

```shell-session
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```

```shell-session
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
Once you find a correct id, then we can use curl to make a post request:
```shell-session
curl -X POST http://admin.academy.htb:46077/admin/admin.php -d "id=73"
```
