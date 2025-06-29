#### SQL Prevention

1. Input Sanitization
2. Least privilege
3. WAFs
4. Parameterized queries

Injection can be avoided by sanitizing any user input, rendering injected queries useless. Libraries provide multiple functions to achieve this, one such example is the [mysqli_real_escape_string()](https://www.php.net/manual/en/mysqli.real-escape-string.php) function. This function escapes characters such as `'` and `"`, so they don't hold any special meaning.

```php
<SNIP>
$username = mysqli_real_escape_string($conn, $_POST['username']);
$password = mysqli_real_escape_string($conn, $_POST['password']);

$query = "SELECT * FROM logins WHERE username='". $username. "' AND password = '" . $password . "';" ;
echo "Executing query: " . $query . "<br /><br />";
<SNIP>
```

You can also restrict to only letters and spaces:
```php
<SNIP>
$pattern = "/^[A-Za-z\s]+$/";
$code = $_GET["port_code"];

if(!preg_match($pattern, $code)) {
  die("</table></div><p style='font-size: 15px;'>Invalid input! Please try again.</p>");
}

$q = "Select * from ports where port_code ilike '%" . $code . "%'";
<SNIP>
```

We should ensure that the user querying the database only has minimum permissions.

```shell-session
CREATE USER 'reader'@'localhost';
```
```shell-session
GRANT SELECT ON ilfreight.ports TO 'reader'@'localhost' IDENTIFIED BY 'p@ssw0Rd!!';
```

Use Web Application Firewall (WAF) to reject HTTP requests containing malicious input like 'INFORMATION_SCHEMA'

Parameterized queries contain placeholders for the input data, which is then escaped and passed on by the drivers. Instead of directly passing the data into the SQL query, we use placeholders and then fill them with PHP functions.

```php
<SNIP>
  $username = $_POST['username'];
  $password = $_POST['password'];

  $query = "SELECT * FROM logins WHERE username=? AND password = ?" ;
  $stmt = mysqli_prepare($conn, $query);
  mysqli_stmt_bind_param($stmt, 'ss', $username, $password);
  mysqli_stmt_execute($stmt);
  $result = mysqli_stmt_get_result($stmt);

  $row = mysqli_fetch_array($result);
  mysqli_stmt_close($stmt);
<SNIP>
```

The query is modified to contain two placeholders, marked with `?` where the username and password will be placed. We then bind the username and password to the query using the [mysqli_stmt_bind_param()](https://www.php.net/manual/en/mysqli-stmt.bind-param.php) function. This will safely escape any quotes and place the values in the query.