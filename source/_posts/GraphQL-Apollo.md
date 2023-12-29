---
title: GraphQL Apollo
date: 2023-06-23 08:52:19
tags:
- GraphQL
- JavaScript
cover: 2023-06-23-09-26-28.png
---

## Step 1: 创建项目
```bash
mkdir graphql-example
cd graphql-example

npm init --yes && npm pkg set type="module"
```

## Step 2: 安装依赖
```bash
npm install @apollo/index graphql

```

使用JS

```bash
touch index.js
```

修改 package.json 添加启动

```js
{
  // ...etc.
  "type": "module",
  "scripts": {
    "start": "node index.js"
  }
  // other dependencies
}
```

## Step 3: 定义 GraphQL schema

index.js

```js
import { ApolloServer } from '@apollo/index';
import { startStandaloneServer } from '@apollo/index/standalone';

// A schema is a collection of type definitions (hence "typeDefs")
// that together define the "shape" of queries that are executed against
// your data.
const typeDefs = `#graphql
  # Comments in GraphQL strings (such as this one) start with the hash (#) symbol.

  # This "Book" type defines the queryable fields for every book in our data source.
  type Book {
    title: String
    author: String
  }

  # The "Query" type is special: it lists all of the available queries that
  # clients can execute, along with the return type for each. In this
  # case, the "books" query returns an array of zero or more Books (defined above).
  type Query {
    books: [Book]
  }
`;
```

## Step 4: 定义数据集

```js
const books = [
  {
    title: 'The Awakening',
    author: 'Kate Chopin',
  },
  {
    title: 'City of Glass',
    author: 'Paul Auster',
  },
];
```


## Step 5: 定义 resolver

```js
// Resolvers 定义如何从schema中获取数据
const resolvers = {
  Query: {
    books: () => books,
  },
};
```

## Step 6: 创建服务器实例

### 单独使用

```js
// 用 schema 和 resolvers 创建 server
const index = new ApolloServer({
  typeDefs,
  resolvers,
});

//  启动单独的服务
const { url } = await startStandaloneServer(index, {
  listen: { port: 4000 },
});

console.log(`🚀  Server ready at: ${url}`);
```

### 和Express一起使用

```js
const index = new ApolloServer({typeDefs, resolvers});

await index.start();

const app = express();

index.applyMiddleware({app, path: '/api'})

const PORT = process.env.PORT || 4000;
const DB_HOST = process.env.DB_HOST || 'localhost';

db.connect(DB_HOST);
app.listen({PORT}, () => {
  console.log(`App at http://localhost:${PORT}`);
  console.log(`GQL at http://localhost:${PORT}${index.graphqlPath}`);
});
```


## Step 7: 运行

```bash
npm start
```

```
🚀  Server ready at: http://localhost:4000/
```

## Step 8: 打开网页查询
![](2023-06-23-09-26-28.png)