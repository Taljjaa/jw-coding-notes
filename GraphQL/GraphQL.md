# GraphQL

Starting with the [how to graphql tutorial](https://www.howtographql.com/basics/0-introduction/)

## What is GraphQL

-  new API standard that was invented and open-sourced by Facebook
-  enables declarative data fetching so you can grab exactly what you want
-  GraphQL server exposes a single endpoint
-  GraphQL is more efficient Alternative to REST
   -  Increased mobile usage creates need for efficient data loading
   -  Variety of different frontend frameworks and platforms on the client-side
   -  Fast development speed and expectation for rapid feature development
-  GraphQL can be used with any programming language and framework
-  rapidly changing requirements on client-side don't go well with the static nature of REST
-  GraphQL was developed to cope with the need for more flexibility and efficiency in client-server communication
-  In REST we might have three different endpoints /users/:id, /users/:id/posts, and /users/:id/followers
   -  We have to download additional information and make multiple different fetch requests from the backend
-  With GraphQL you only need a single request
-  No more over or under-fetching
-  REST: structure endpoints according to clients' data need but this is not flexible and easily iterated on
-  No need to adjust API when product requirements and design change
-  GraphQL uses strong type system to defined capabilites of an API

## Core Concepts

```
type Person {
    name: String!
    age: Int!
    posts: [Post!]!
}

//the exclamation mark means this field is required

type Post {
    title: String!
    author: Person!
}

```

-  By adding the author field and posts field we have created a relationship between Person and Post
-  The posts field syntax is used to denote that the value will be a list in the Schema Definition Language (SDL)
-  Now we have defined a one to many relationship between and Person type and a Post type
-  The structure of the returned data in GraphQL is completely flexible depending on what the client asks for
-  Write data with Mutations
   -  3 kinds of mutations
      -  creating new data
      -  updating existing data
      -  deleting existing data
-  All queries need a root and a payload
   -  the queries can also have parameters

```
mutation {
    createPerson(name: 'Bob', age: 36) {
        name
        age
    }
}
// the payload of a mutation is what the server responds with, this is nice because we can get data back from the server in one round trip

mutation {
    createPerson(name: 'Bob', age: 36) {
        id
    }
}

//this will return the id which we could then use to for login and user features
```

-  Subscriptions?

## GraphQL Schema

-  defines capabilites of the API by specifying how a client can fetch and update data
-  collection of GraphQL types with special root types
-  In order to use the root createPerson we would have to create a field of createPerson on the mutation, this applies to the Query type and the Subscription Type

```
mutation {
    createPerson(name: 'Bob', age: 36) {
        id
    }
}

type Mutation {
    createPerson(name: String!, age: String!): Person!
}
```

## Architecture

-  GraphQL server with a connected database
   -  often used for greenfield projcets
   -  uses single web server that implements GraphQL
   -  server resolves queries and constructs response with data that it fetches from the database
-  GraphQL doesn't care about the database or how the data is stored
-  GraphQL server integrating existing systems
   -  compelling use case for companies with legacy infrastructures and many different APIs
   -  can be used to unify existing systems and hide complexity of data fetching logic

### Resolver functions

-  GraphQL queries/mutations consist of a set of fields in the payload
-  GraphQL server has one resolver function per field
   -  the purpose of each resolver is to retrieve the data for its corresponding field
-  GraphQL is great for frontend develoeprs as data fetching complexity can be pushed to the server-side
-  client doesn't care where data is coming from
-  opportunity for new abstractions on the frontend
-  from imperative to declarative data fetching

## Advanced Tutorial- Clients

-  GraphQL provides the ability to abstract away a lot of the manual work you'd usually have to do during that process and lets you focus on the real important parts of your app!
