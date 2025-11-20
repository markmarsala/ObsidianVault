Alternative to REST

Endpoints:
- /graphql
- /api/graphql

Example request:
```
{
  users {
    id
    username
    role
  }
}
```

Example response:
```
{
  "data": {
    "users": [
      {
        "id": 1,
        "username": "htb-stdnt",
        "role": "user"
      },
      {
        "id": 2,
        "username": "admin",
        "role": "admin"
      }
    ]
  }
}
```

Get username:
```
{
  users(username: "admin") {
    id
    username
    role
  }
}
```

Not interested in the roles category, but interested in password:
```
{
  users(username: "admin") {
    id
    username
    password
  }
}
```

Supports sub-querying
```
{
  posts {
    title
    author {
      username
      role
    }
  }
}
```

Response:
```
{
  "data": {
    "posts": [
      {
        "title": "Hello World!",
        "author": {
          "username": "htb-stdnt",
          "role": "user"
        }
      },
      {
        "title": "Test",
        "author": {
          "username": "test",
          "role": "user"
        }
      }
    ]
  }
}
```
