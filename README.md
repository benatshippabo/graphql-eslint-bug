# graphql-eslint-bug

https://github.com/B2o5T/graphql-eslint/issues/1117

Given this schema:

```gql
type Query {
  user(id: String!): UserResult
}

union UserResult = Account | Error

type Account {
  id: ID!
  name: String!
  phone: String
}

type Error {
  message: String!
}
```

we expect `@graphql-eslint/eslint-plugin` eslint configured with:

```yml
rules:
  "@graphql-eslint/require-id-when-available": error
```

to error out for the inline `Account` fragment and `Account` fragment:

```gql
query UserById($id: String!) {
  user(id: $id) {
    ... on Account {
      # inline fragment: we expect an error here about a missing `id` field
      ...UserFields
    }

    ... on Error {
      message
    }
  }
}

fragment UserFields on Account {
  # fragment: we expect an error here about a missing `id` field
  name
  phone
}
```
