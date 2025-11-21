## Identifying the GraphQL Engine

1. Request to /graphql endpoint

Tool to identify engine: https://github.com/dolevf/graphw00f
```
python3 main.py -d -f -t http://172.17.0.2
```

More info on the engine: https://github.com/nicholasaleks/graphql-threat-matrix
- Visit the /graphql endpoint to query directly

## Introspection

Identify all GraphQL types supported by the backend:
```
{
  __schema {
    types {
      name
    }
  }
}
```
- Found UserObject

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

Obtain all the queries supported by the backend:
```
{
  __schema {
    queryType {
      fields {
        name
        description
      }
    }
  }
}
```

Query everything
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

Use this tool to visualize: https://github.com/graphql-kit/graphql-voyager
- Online version: https://graphql-kit.com/graphql-voyager/
- Make a local one for real world engagements


