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

#### `CommonJs`、`CMD`和`AMD`规范的区别(2019/10/28)

1. `CommonJs`用的是同步的方式加载模块。`NodeJs`是该规范的主要实践者，`module exports require global`这四个变量为模块化的实现提供了支持。最基础的模块的导入导出这里就不再赘述。

   我们需要知道，在服务端，模块文件都存在本地磁盘，读取速度是很快的，所以使用同步加载问题不大。但在浏览器端，由于各方面原因，更多的还是推荐异步加载，即浏览器一般采用`AMD`规范。

2. `AMD`即`Asynchronous Module Definition`，`RequireJs`是其对应实践。它推崇依赖前置、提前执行。

3. `CMD`即`Common Module Definition`，`SeaJs`是其对应实践。它推崇依赖就近、延迟执行。

4. 除此之外还有ES6模块值得一提，它在语言标准的层面上，很简单的使用`import和export`就实现了浏览器端和服务端通用的模块解决方案。与`CommonJs`的区别在于，前者输出的是一个值的拷贝，而`ES6`模块输出的是值的引用。前者是运行时加载，后者是编译时才输出接口。

   参考文章：

   - [掘金--前端模块化：CommonJS,AMD,CMD,ES6](<https://juejin.im/post/5aaa37c8f265da23945f365c>)
   - [简书--CommonJS规范与AMD/CMD规范总结](<https://www.jianshu.com/p/7c7fc199b7aa>)
   - [个人博客--AMD规范](<https://zhaoda.net/webpack-handbook/amd.html>)

#### NodeJs -- V8 -- ECMAScript(2019/10/28)

1. `V8`是采用`C++`编写的一个高性能的`Javascript`引擎，可单独运行，也可以嵌入在`C++`应用中，同时也会编译、执行`Javascript`代码，管理内存，垃圾回收等。

   再来说说为什么它会这么高效。`V8`开发的核心工程师`Lars Bak`之前是`Sun`公司做`Java`虚拟机加速技术研究的，而且产出了`HotSpot`，曾经还开发了`Strongtalk`，这以上两者的精髓都被吸收在了`V8`的代码里面。

   它的高效主要体现在以下4个特性上：

   - JIT，即时编译。它编译出来的直接是机器语言，而不是字节码。由此极大提高了`Js`运行时的效率。

   - 垃圾回收，`V8`的垃圾回收借鉴了`Java VM`的精确垃圾回收管理，相较于其他语言的保守垃圾回收管理，前者这种机制的实现难度更大，但受益明显。

   - 内联缓存，`V8`使用这个特性提高了访问效率。如`this.test`的访问，没有使用内联缓存访问`test`的话则每次都要对哈希表进行一次寻址，而加入了该特性后，`V8`马上就能知道这个属性的偏移量，而不用再次计算。

   - 隐藏类，动态语言里我们可以方便的添加或移除对象属性，而隐藏类就是这样一套对象体系中的一种黑科技包装，所有如属性一样的对象都会被归为一个隐藏类。

     同一个隐藏类的对象可以用同一套内联缓存来寻址，因此，当两者结合在一起使用，更极大的提高了`V8`的效率。

2. `V8`在开发的过程中，一直跟随着`ECMAScirpt`发布的脚步，如最新版（`NodeJs 7.6`)也默认支持了`async/await`函数，也正是因为`V8`对`ECMAScirpt`的紧追不舍，才能让`Node Js`基本上完成了对`ES6`的支持。`Node Js`一方面一直使用`V8`作为自己的底层支持，一方面也为`Node Js`做了一些事情，如：

   - 在谷歌开发者工具中可以调试`Node Js`
   - 加速`ES6`
   - `Async / awit`
   - 对`Node Js`部分模块的一些修复

#### libuv

1. 专注于异步I/O的跨平台类库，主要为`Node Js`开发，但在`Python Julia`等语言中也有使用
2. `Node Js`中的一个重要概念就是事件循环，而事件循环就是使用`libuv`（C实现）进行驱动的。



