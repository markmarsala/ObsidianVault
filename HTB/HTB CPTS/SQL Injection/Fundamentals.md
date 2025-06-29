### SQL Injection Fundamentals
---------
DBMS features:
- Concurrency
- Consistency
- Security
- Reliability
- Structured Query Language

Non-relational Database storage models:
- key-value (JSON/XML)
- document-based
- wide-column
- graph


#### MySQL

SQL can:
- Retrieve data
- Update data
- Delete data
- Create new tables and databases
- Add / remove users
- Assign permission to these users

Connect locally:
```shell-session
mysql -u root -p
```
(do not input password here as it will be stored in bash_history)

Connect remotely:
```shell-session
mysql -u root -h docker.hackthebox.eu -P 3306 -p 
```

SQL commands:

```shell-session
CREATE DATABASE users;
```
```shell-session
SHOW DATABASES;
```
```shell-session
USE users;
```
```sql
CREATE TABLE logins (
    id INT,
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );
```
```shell-session
SHOW TABLES;
```
```shell-session
DESCRIBE logins;           #fields and types
```
```sql
id INT NOT NULL AUTO_INCREMENT,     
```
 AUTO_INCREMENT: increment id by one every new item added to table
 NOT NULL: a particular column is never left empty
 ```sql
username VARCHAR(100) UNIQUE NOT NULL,
```
UNIQUE: inserted item is not repeated
```sql
date_of_joining DATETIME DEFAULT NOW(),
```
```sql
PRIMARY KEY (id)
```
```sql
CREATE TABLE logins (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    date_of_joining DATETIME DEFAULT NOW(),
    PRIMARY KEY (id)
    );
```


##### INSERT
```sql
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
```
```shell-session
INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02');
```
If values have DEFAULT set, we don't need to add them:
```sql
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```
```shell-session
INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss');
```
```shell-session
INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');
```

##### SELECT
```sql
SELECT * FROM table_name;
```
```sql
SELECT column1, column2 FROM table_name;
```

##### DROP (delete)
```shell-session
DROP TABLE logins;
```

##### ALTER (change properties of tables)
```shell-session
ALTER TABLE logins ADD newColumn INT;
```
```shell-session
ALTER TABLE logins RENAME COLUMN newColumn TO newerColumn;
```
MODIFY changes a column's datatype
```shell-session
ALTER TABLE logins MODIFY newerColumn DATE;
```
```shell-session
ALTER TABLE logins DROP newerColumn;
```

##### UPDATE (change records)
```sql
UPDATE table_name SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;
```
```shell-session
UPDATE logins SET password = 'change_password' WHERE id > 1;
```

##### ORDER BY (sort)
```shell-session
SELECT * FROM logins ORDER BY password;
```
ASC/DESC
```shell-session
SELECT * FROM logins ORDER BY password DESC;
```
```shell-session
SELECT * FROM logins ORDER BY password DESC, id ASC;
```

##### LIMIT
```shell-session
SELECT * FROM logins LIMIT 2;
```
Limit with offset (only returns 2 and 3):
```shell-session
SELECT * FROM logins LIMIT 1, 2;
```

##### WHERE
```sql
SELECT * FROM table_name WHERE <condition>;
```
```shell-session
SELECT * FROM logins WHERE id > 1;
```
```shell-session
SELECT * FROM logins where username = 'admin';
```

##### LIKE
```shell-session
SELECT * FROM logins WHERE username LIKE 'admin%';
```
```shell-session
SELECT * FROM logins WHERE username like '___';
```
(select usernames with 3 chars)

##### AND (&&) (0 = false, 1 = true) 
```shell-session
SELECT 1 = 1 AND 'test' = 'test';
```

##### OR ( || )
```shell-session
SELECT 1 = 1 OR 'test' = 'abc';
```

##### NOT  (!)
```shell-session
SELECT NOT 1 = 1;
```

Operator precedence:
1. /, \*, %
2. +, -
3. =, <, >, <=, >=, !=, LIKE
4. !
5. &&
6. ||


