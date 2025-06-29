
#### SQL Injection Types
- In-band: printed directly on the front end
	- Union-based: specify column
	- Error-based: PHP or SQL errors
- Blind: retrieve output character by character
	- Boolean-based: return any output if true
	- Time-based: delay page response with Sleep() if true
- Out-of-band: retrieve output from remote location like DNS record
#### SQL Injection Start

PHP web application querying database:
```php
$conn = new mysqli("localhost", "root", "password", "users");
$query = "select * from logins";
$result = $conn->query($query);
```
Returned result printed on new lines:
```php
while($row = $result->fetch_assoc() ){
	echo $row["name"]."<br>";
}
```

Query based on user input without sanitization:
```php
$searchInput =  $_POST['findUser'];
$query = "select * from logins where username like '%$searchInput'";
$result = $conn->query($query);
```

As a result, we can escape the query and execute SQL:
```sql
select * from logins where username like '%1'; DROP TABLE users;'
```
(possible with MSSQL and PostgreSQL, but not MySQL)

Error due to single quote and no trailing quote.