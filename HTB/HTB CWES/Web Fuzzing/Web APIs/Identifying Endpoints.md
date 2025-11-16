## REST

- Query Parameters (appended to end of URL with ?, i.e. /users?limit=10&sort=name)
- Path Parameters (embedded directly within URL, i.e. /products/{id}pen_spark)
- Request Body Parameters (sent in the body of POST, PUT or PATCH, i.e. { "name": "New Product", "price": 99.99 }

**Discover endpoint and parameters
1. API Documentation
2. Network Traffic Analysis
3. Parameter Name Fuzzing


## SOAP

- keywords
- author
- genre

**Discover endpoint and parameters
1. WSDL Analysis
2. Network Traffic Analysis
3. Fuzzing for Parameter Names and Values

## GraphQL

1. Field (name,email)
2. Relationship (posts)
3. Nested Object (posts { title, body })
4. Argument (posts(limit:5))

```
query {
  user(id: 123) {
    name
    email
    posts(limit: 5) {
      title
      body
    }
  }
}
```

**GraphQL Mutations

1. Operation (createPost)
2. Argument (title: "New Post", body: "this is the content of the new post")
3. Selection (id,title)

```
mutation {
  createPost(title: "New Post", body: "This is the content of the new post") {
    id
    title
  }
}
```

**Discover Queries and Mutations

1, Introspection
2. API Documentation
3. Network Traffic Analysis
