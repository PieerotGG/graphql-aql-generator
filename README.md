# sgq
schema graphql query – query GraphQL with only a GraphQL IDL schema

# Example


```es6
'use strict';

const graphql = require('graphql-sync').graphql;
const sgq = require('sgq');


let typeDefs = [`
  type BlogEntry {
    _key: String!
    authorKey: String!

    author: Author @aql(exec: "FOR author in Author filter author._key == @current.authorKey return author")
  }

  type Author {
    _key: String!
    name: String
  }

  type Query {
    blogEntry(_key: String!): BlogEntry
  }
`]

const schema = sgq(typeDefs);

const query = `
{
  blogEntry(_key: "1") {
    _key
    authorKey
    author {
      name
     }
  }
}`;

const result = graphql(schema, query);
print(result);
/*
{
  "data" : {
    "blogEntry" : {
      "_key" : "1",
      "authorKey" : "2",
      "author" : {
        "name" : "Plumbum"
      }
    }
  }
}
*/`
```
