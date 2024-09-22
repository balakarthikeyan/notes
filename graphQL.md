# 𝗚𝗿𝗮𝗽𝗵𝗤𝗟
GraphQL excels in efficient data loading. 𝗚𝗿𝗮𝗽𝗵𝗤𝗟 is best for applications with complex, interconnected data and those needing real-time updates. Its flexible data fetching is great for dynamic user interfaces and fast front-end development.

## 𝗞𝗲𝘆 𝗕𝗲𝗻𝗲𝗳𝗶𝘁𝘀: 
🔷 𝐄𝐟𝐟𝐢𝐜𝐢𝐞𝐧𝐭 𝐃𝐚𝐭𝐚 𝐋𝐨𝐚𝐝𝐢𝐧𝐠:Ideal for apps with complex data structures and relationships.
🔷 𝐒𝐢𝐧𝐠𝐥𝐞 𝐍𝐞𝐭𝐰𝐨𝐫𝐤 𝐑𝐞𝐪𝐮𝐞𝐬𝐭: Combines multiple data needs into one network request.
🔷 𝐑𝐞𝐚𝐥-𝐓𝐢𝐦𝐞 𝐃𝐚𝐭𝐚 𝐰𝐢𝐭𝐡 𝐒𝐮𝐛𝐬𝐜𝐫𝐢𝐩𝐭𝐢𝐨𝐧𝐬: Supports real-time updates.

## 𝗖𝗵𝗮𝗹𝗹𝗲𝗻𝗴𝗲𝘀: 
🔶 𝐏𝐞𝐫𝐟𝐨𝐫𝐦𝐚𝐧𝐜𝐞 𝐈𝐬𝐬𝐮𝐞𝐬 𝐰𝐢𝐭𝐡 𝐂𝐨𝐦𝐩𝐥𝐞𝐱 𝐐𝐮𝐞𝐫𝐢𝐞𝐬:  Complex queries can sometimes slow down performance.
🔶 𝐂𝐚𝐜𝐡𝐢𝐧𝐠 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞𝐬: Unlike REST, GraphQL needs more advanced caching strategies.
🔶 𝐒𝐞𝐜𝐮𝐫𝐢𝐭𝐲 𝐂𝐨𝐧𝐜𝐞𝐫𝐧𝐬: Requires careful management of query complexity and depth to avoid malicious queries.

## GraphQL’s basic terminology:

- `Query:` is a read-only operation requested to a GraphQL server
- `Mutation:` is a read-write operation requested to a GraphQL server
- `Resolver:` In GraphQL, the Resolver is responsible for mapping the operation and the code running on the backend which is responsible for handle the request. It is analogous to MVC backend in a RESTFul application
- `Type:` A Type defines the shape of response data that can be returned from the GraphQL server, including fields that are edges to other Types
- `Input:` like a Type, but defines the shape of input data that is sent to a GraphQL server
- `Scalar:` is a primitive Type, such as a String, Int, Boolean, Float, etc
- `Interface:` An Interface will store the names of the fields and their arguments, so GraphQL objects can inherit from it, assuring the use of specific fields
- `Schema:` In GraphQL, the Schema manages queries and mutations, defining what is allowed to be executed in the GraphQL server.

## GraphQL’s Interface Definition Language (IDL)

Interface Definition Language (IDL) or Schema Definition Language (SDL) is the most concise way to specify a GraphQL Schema. The syntax is well-defined and will be adopted in the official GraphQL Specification.

```
schema {
    query: QueryType
}

enum Gender {
    MALE
    FEMALE
}

type User {
    id: String!
    firstName: String!
    lastName: String!
    createdAt: DateTime!
    age: Int! @default(value: 0)
    gender: [Gender]!
    emails: [Email!]! @relation(name: "Emails")
}

type Email {
    id: String!
    email: String!
    default: Int! @default(value: 0)
    user: User @relation(name: "Emails")
}
```