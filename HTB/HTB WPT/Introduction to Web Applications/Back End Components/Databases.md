Relational (SQL)
- MySQL
- MSSQL
- Oracle
- PostgreSQL
- SQLite
- MariaDB
- Amazon Aurora
- Azure SQL


Non-relational (NoSQL)
- MongoDB
- ElasticSearch
- Apache Cassandra
- FireBase

Create database
```
$sql = "CREATE DATABASE database1";
$conn->query($sql)
```

Connect
```
$conn = new mysqli("localhost", "user", "pass", "database1");
$query = "select * from table_1";
$result = $conn->query($query);
```

Example post
```
$searchInput =  $_POST['findUser'];
$query = "select * from users where name like '%$searchInput%'";
$result = $conn->query($query);
```

Return results
```
while($row = $result->fetch_assoc() ){
	echo $row["name"]."<br>";
}
```
