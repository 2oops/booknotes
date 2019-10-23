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

#### Basic(2019-10-23)

1. 定义：`Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.`即是说`NodeJs`是一个建立于谷歌V8引擎上的`JavaScript`运行时。`NodeJs`使用了**事件驱动、非阻塞式I/O**的模型，轻量高效。

2. `NodeJs`架构图

   ![img](https://www.nodejs.red/nodejs/base/img/nodejs_architecture.png)

   ***

   这里解释一下：

   标准库：其对外提供`Javascript`接口，包括`http fs stream`等模块；

   `Node Bindings`则是`JS`和`C++`连接的桥梁，封装底层模块，提供接口给上层模块；

   `V8`是使用`C++`开发的高性能的`JS`引擎，也应用于谷歌浏览器上；

   `Libuv`是一个跨平台的支持事件驱动的`I/O库`，`I/O`操作的核心部分，采用`C和C++`开发

   `DNS`中的`C-ares`则是一个异步`DNS`解析库；

   底层组件则提供了`http`解析、`OpenSSL`等功能

3. `NodeJs`适合做什么？

   `NodeJs`很擅长**I/O密集型**任务，因此在一些高并发场景下有一定的优势。所以它的定位在：**提供一种简单安全的方法在`JS`中构建高性能和可扩展的网络应用程序**。因为它的四个特点：**单线程、非阻塞I/O、事件驱动、跨平台**

4. `NPM`源设置

   `npm config set registry=https://registry.npm.taobao.org`

   切换为官方源

   `npm config set registry=https://registry.npmjs.org`

5. `NPM`注册登录

   ```
   $ npm adduser
   Username: your name
   Password: your password
   Email: (this IS public) your email
   // 查看当前用户
   npm whoami
   //npm登录
   npm login
   // 发布
   npm publish
   ```

6. `NPM`发布私有模块

   ```
   ...json
   {
       "name":"@facebook/logger"// @开头，/结束,facebook为组织名，logger为包名
   }
   ```

   

