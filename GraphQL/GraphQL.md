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

## Advanced Tutorial

### Clients

-  GraphQL provides the ability to abstract away a lot of the manual work you'd usually have to do during that process and lets you focus on the real important parts of your app!
-  A major benefit of GraphQL is that it allows you to fetch and update data in a declarative manner.
-  Previously we used plain HTTP like fetch in JavaScript to load data from an API, now with GraphQL you can write a query where you declare your data requirements and let the system take care of sending the request and handling the response for you
   -  This is what a GraphQL client like Apollo or Relay will do.

### Server

-  GraphQL is often explained as a frontend-focused API technology because it enables clients to get data in a much nice way than before
   -  but the API itself is implemented on the server side
-  GraphQL enables the server developer to focus on describing the data available rather than implementing and optimizing specific endpoints
-  A query is traversed field by field, executing "resolvers" for each field
-  Find the resolvers in our server to run for every field, the execution starts at the query type and goes breadth-first
   -  at the end the execution algorithm puts everything together into the correct shape for the result and returns that
-  Most GraphQL server implementations will provide "default resolvers" so you don't have to specify a resolver function for every single field

-  Fragments are a handy feature to help imporove the structure and reusability of your GraphQL code. A fragment is a collection of fields on a specific type

```
type User {
  name: String!
  age: Int!
  email: String!
  street: String!
  zipcode: String!
  city: String!
}

This can be written with a fragment as

fragment addressDetails on User {
  name
  street
  zipcode
  city
}

Now when writing a query to access the address information of a user we can use the following syntax to refer to the fragment and save the work to actually spell out the four fields

{
  allUsers {
    ... addressDetails
  }
}

This query is equivalent to writing:

{
  allUsers {
    name
    street
    zipcode
    city
  }
}
```

-  In GraphQL type definitions, each field can take zero or more arguments. Each argument needs to have a name and a type, in GraphQL it's also possible to specify default values for arguments
-  One of GraphQl's major strengths is that it lets you send multiple queries in a single request, however since the response data is shaped after the structure of the fields being requested you might run into naming issues
-  In GraphQL there are two different kinds of types
   -  Scalar types represent concerete units of data, five predefined scalars: String, Int, Float, Boolean, and ID
   -  Object types have fields that express the properties of that type and are composable

## React + Apollo Tutorial

-  GraphQL is a query language for APIs
-  Apollo abstracts away all lower-level networking logic and provides a nice interface to the GraphQL server
-  In contrast to working with REST APIs you don't hav eto deal with constructing your own HTTP requests any more instead you can simply write queries and mutations and send them using an ApolloClient instance
-  The first thing you have to do when using Apollo is configure your ApolloClient instance, it needs to know the endpoint of your GraphQL API so it can deal with the network connections
-  When using Apollo you've got two ways of sending queries to the server
-  The first one is to directly use the query method on the ApolloClient directly, this is a very direct way of fetching data and will allow you to process the response as a Promise

```
client.query({
  query: gql`
    {
      feed {
        links {
          id
        }
      }
    }
  `
}).then(response => console.log(response.data.allLinks))
```

-  A more declarative way when using React is to use Apollo's render prop API to manage your GraphQL data just using components
   -  Now Apollo supports React Hooks so instead of render prop the useQuery hook can be used to fetch GraphQL data
   -  All the logic for retrieving your data, tracking loading and error states, and updating your UI is encapsulated by the useQuery hook
   -  useQuery leverages React's Hooks API to bind a query to our component and render it based on the results of the query
   -  once the data comes back the component will update reactively with the data it needs
-  Simply write a GraphQL query and Apollo Client will take care of requesting and caching your data, as well as updating your UI
-  Apollo Client is easy to get started with, but extensible for when you need to build out more advanced features.
-  it's generally accepted that GraphQL improves performance by helping avoid round trips to the server and reducing payload size.
-  Every data graph uses a schema to define the types of data it includes
   -  your graphql schema defines what types of data a client can read and write to your data graph
   -  schemas are strongly typed which unlocks powerful developer tooling
-  Most of the definitions in a GraphQL schema are object types. Each object type you define should represent an object that an application client might need to interact with
-  If a declared field's type is in [Square Brackets], it's an array of the specified type, if an array has an exclamation point after it, the array cannot be null but it can be empty
-  We've defined the objects that exist in our data graph, but clients don't yet have a way to fetch those objects. To resolve that our schema needs to define queries that clients can execute against the data graph
-  Queries enable clients to fetch data, but not to modify data. To enable clients to modify data, our schema needs to define some mutations
-  A mutation's return type is entirely up to you, but we recommend defining special object types specifically for mutation responses
   -  It's good practice for a mutation to return wahtever objects it modifies so the requesting client can update its cache and UI without needing to make a followup query
-  GraphQL APIs are extremely flexible because you can layer them on top of any service, including any business logic, REST APIs, databases, or gRPC services
-  An Apollo data source is a class that encapsulates all of the data fetching logic, as well as caching and deduplication, for a particular service
-  Resolvers provide the instructions for turning a GraphQL operation (a query, mutation, or subscription) into data
   -  they can either return the same type of data we specify in our schema or a promise for that data
-  Resolver functions accept four arguments

```
fieldName: (parent, args, context, info) => data;
```

-  parent: An object that contains the result returned from the resolver on the parent type
-  args: An object that contains the arguments passed to the field
-  context: An object shared by all resolvers in a GraphQL operation. We use the context to contain per-request state such as authentication information and access our data sources
-  info: Information about the execution state of the operation which should only be used in advanced cases
-  We recommend keeping your resolvers thin as a best practice which allows you to safely refactor without worrying about breaking your API
-  When you write a GraphQL query you always want to start with the operation keyword(either query or mutation) and its name(like GetLaunches)
-  What's awesome about GraphQL is that the shape of your query will match the shape of your response
-  Pagination is a solution to this problem that ensures that the server only sends data in small chunks
-  You can write resolvers for any types in your schema not just queries and mutations, this is what makes GraphQL so flexible

## Authentication

-  The context function of your ApolloServer instance is called with the request object each time a GraphQL operation hits your API, use this request object to read the authorization headers
-  Authenticate the user within the context function
-  Once the user is authenticated, attach the user to the object returned from the context function, this allows us to read the user's information from within our data sources and resolvers, so we can authorize whether they can access the data

## Graph Manager

-  Publishing your schema to Apollo Graph Manager unlocks many features necessary for running a graph API in production
   -  Schema explorer: You can quickly explore all the types and fields in your schema with usage statistics on each field, this metric makes you understand the cost of a field
   -  Schema history: Allows you to confidently iterate a graph's schema by validating the new schema against field-level usage data from a previous schema
   -  Performance analytics: Insights into every field, resolver, and operation of your graph's execution
   -  Client awareness: Report client identity(name and version) to your server for insights on client activity

## Fetch Data with Queries

-  The useQuery hook is one of the most important building blocks of an Apollo app, it's a react hook that fetches a GraphQL query and exposes the result so you can render your UI based on the data it returns
-  The userQuery hook fetches and loads data from queries to our UI, it exposes error, loading, and data properties through a result object, that help us populate and rende rour component.

# ByteConf GraphQL 2020

-  Arguments reduce multiple API calls
   -  one of the nice features of GraphQL compared to REST
-  the json response from GraphQL is going to return exactly what the query asked for (unique feature of GraphQL you can query what you want with no extra data or filtering)
-  multiple clients can ask for different sets of data from the same GraphQL server
-  what some fields you might have a ton of data and we don't always want all of that back, for this we can use pagination
-  use aliases to distinguish between queries with the same field

```
query MyFirstQuery {
   firstThree: following (first: 3) {
      edges {
         node {
            <!-- id
            name
            bio -->
            ...userInfo
         }
      }
   }
   lastThree: following (last: 3) {
      edges {
         node {
            <!-- id
            name
            bio -->
            ...userInfo
         }
      }
   }
}

fragment userInfo on User {
   id
   name
   bio
}

```

-  Fragment is GraphQL's reusable unit
   -  they let you build sets of fields and include them in multiple queries
-  the ... indicates the use of a fragment
-  Mutations are used to make changes to the data (create, update, delete data)
- Serverless computing is a cloud-computing execution model in which the cloud provider runs the server, and dynamically manages the allocation of machine resources 
   - the idea that we pay for things only as we use them
- Define the data types and how they are related 
- Specify entry points for the API
   - Query and Mutation types 
- GraphQL Schema Definition Language or construct schema programmatically 
- Resolver functions are functions that define how to resolve data for GraphQL requests 
- 