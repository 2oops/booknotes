# GraphQL

1. `GraphQL` 是一个用于 `API` 的查询语言，是一个使用基于类型系统来执行查询的服务端运行时（类型系统由你的数据定义）。`GraphQL`并没有和任何特定数据库或者存储引擎绑定，而是依靠你现有的代码和数据支撑。

2. JS中使用

   ```js
   //node.js
   let { graphql, buildSchema } = require('graphql');
   
   let schema = buildSchema(`
     type Query {
       name: String
     }
   `);
   
   let root = { name: () => '2oops world!' };
   
   graphql(schema, '{ name }', root).then((response) => {
     console.log(response);//{ data: [Object: null prototype] { name: '2oops world!' } }
   });
   //执行node hello.js
   ```
