# graphql-eslint-bug

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

We expect `@graphql-eslint/eslint-plugin` configured with eslint:

```yml
rules:
  "@graphql-eslint/require-id-when-available": error
```

to error out for the inline `Account` fragment and `Account` fragment:

```gql
query UserById($id: String!) {
  user(id: $id) {
    ... on Account {
      # inline fragment: expect an error here about missing `id` field
      ...UserFields
    }

    ... on Error {
      message
    }
  }
}

fragment UserFields on Account {
  # fragment: expect an error here about missing `id` field
  name
  phone
}
```
