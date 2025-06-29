#### SQLi Discovery

- '                 %27
- "                 %22
- \#                %23
- ;                  %3B
- )                  %29

On the right hand side are URL encoded. An example of this is when we put our payload directly in the URL 'i.e. HTTP GET request'.

```sql
SELECT * FROM logins WHERE username=''' AND password = 'something';
```

This outputs SQL error due to odd number of quotes. Put even number of quotes or comment out parts.
