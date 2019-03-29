# NodeJS

1. NodeJS调试

   1. inspector原理
      **websockets服务（监听）**
      **Inspector协议**
      **http服务（单向）**

   ```
   code node-debug
   code --inspect app.js
   node app.js
   ```

   2. vscode F5断点调试，优点：不侵入代码，可以查看当前上下文的变量，可以查看堆栈(call stack)
   3. Chrome devTools

2. NodeJS
   1. 特点：无阻塞高并发,异步操作