**Basic listing
```shell-session
sqlmap -h
```

**Advanced Listing
```shell-session
sqlmap -hh
```

Example SQLi vulnerable PHP code:
```shell-session
$link = mysqli_connect($host, $username, $password, $database, 3306);
$sql = "SELECT * FROM users WHERE id = " . $_GET["id"] . " LIMIT 0, 1";
$result = mysqli_query($link, $sql);
if (!$result)
    die("<b>SQL error:</b> ". mysqli_error($link) . "<br>\n");
```

```shell-session
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch
```
- '--batch' is used for skipping any required user-input
