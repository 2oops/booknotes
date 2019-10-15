# Advanced

1. `Webpack`性能优化，实现`webpack`打包优化，有两个优化点：

   - 如何减少打包时间
   - 如何减少打包大小

   ***

   #### 减少打包时间

   1. **优化`Loader`**

      对于`Loader`来说，首先优化的当是`babel`了，`babel`会将代码转成字符串并生成`AST`，然后继续转化成新的代码，转换的代码越多，效率就越低。

      首先可以**优化`Loader`的搜索范围**

      ```javascript
      module.exports = {
          module: {
              rules: [
                  test: /\.js$/, // 对js文件使用babel
                  loader: 'babel-loader',
                  include: [resolve('src')],// 只在src文件夹下查找
          		// 不去查找的文件夹路径，node_modules下的代码是编译过得，没必要再去处理一遍
                  exclude: /node_modules/
              ]
          }
      }
      ```

      另外可以将`babel`编译过文件**缓存**起来，以此加快打包时间，主要在于设置`cacheDirectory`

      ```javascript
      loader: 'babel-loader?cacheDirectory=true'
      ```

   2. **HappyPack**

      因为受限于Node的单线程运行，所以`webpack`的打包也是单线程的，**使用`HappyPack`可以将`Loader`的同步执行转为并行，**从而执行`Loader`时的编译等待时间。

      ```javascript
      module: {
          loaders: [
              test: /\.js$/,
              include: [resolve('src')],
              exclude: /node_modules/,
              loader: 'happypack/loader?id=happybabel' //id对应插件下的配置的id
          ]
      },
      plugins: [
          new HappyPack({
              id: 'happybabel',
              loaders: ['babel-loader?cacheDirectory'],
              threads: 4, // 线程开启数
          })
      ]
      ```

   3. **DllPlugin**

      **该插件可以将特定的类库提前打包然后引入**，这种方式可以极大的减少类库的打包次数，只有当类库有更新版本时才会重新打包，并且也实现了将公共代码抽离成单独文件的优化方案。

      ```javascript
      // webpack.dll.conf.js
      const path = require('path')
      const webpack = require('webpack')
      module.exports = {
          entry: {
              vendor: ['react'] // 需要统一打包的类库
          }，
          output: {
          	path: path.join(__dirname, 'dist'),
              filename: '[name].dll.js',
              library: '[name]-[hash]'
          },
          plugins: [
              new webpack.DllPlugin({
                  name: '[name]-[hash]', //name必须要和output.library一致
                  context: __dirname, //注意与DllReferencePlugin的context匹配一致
                  path: path.join(__dirname, 'dist', '[name]-manifest.json')
              })
          ]
      }
      ```

      然后在`package.json`文件中增加一个脚本

      ```javascript
      'dll': 'webpack --config webpack.dll.js --mode=development'
      //运行后会打包出react.dll.js和manifest.json两个依赖文件
      ```

      最后使用`DllReferencePlugin`将刚生成的依赖文件引入项目中

      ```javascript
      // webpack.conf.js
      module.exports = {
          //...其他配置
          plugins: [
              new webpack.DllReferencePlugin({
                  context: __dirname,
                  manifest: require('./dist/vendor-manifest.json') //此即打包出来的json文件
              })
          ]
      }
      ```

      更多有关`webpack`配置`DllPlugin`请参考[如何使用 Webpack 的 Dllplugin](<https://blog.csdn.net/lhb215215/article/details/80690710>)和[webpack中DllPlugin用法](https://www.cnblogs.com/wuxianqiang/p/11183736.html)

   4. **代码压缩相关**

      - 启用`gzip`压缩

      - `webpack3`中，可以使用`UglifyJS`压缩代码，但是它是单线程的，因此可以使用`webpack-parallel-uglify-plugin`来运行`UglifyJS`，但在`webpack4`中只要启动了`mode`为`production`就默认开启了该配置

      - 压缩`html和css`代码，通过配置删除`console.log`和`debugger`等，防止可能造成的内存泄漏

        ```javascript
        new UglifyJsPlugin({
            UglifyOptions: {
                compress: {
                    warnings: false,
                    drop_console: true,
                    pure_funcs: ['console.log']
                }
            },
            sourceMap: config.build.productionSourceMap,
            parallel: true
        })
        //或使用以下配置
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false,
                drop_debugger: true,
                drop_console: true
            }
        })
        ```

   #### 减少打包大小

   1. **按需加载，首页加载文件越小越好，将每个页面单独打包为一个文件，**（同样对于`loadsh`类库也可以使用按需加载），原理即是使用的时候再去下载对应的文件，返回一个`promise`，当`promise`成功后再去执行回调。

   2. **Scope Hoisting**

      它会分析出模块之前的关系，尽可能的把打包出来的模块合并到一个函数中去

      ```javascript
      // 如在index.js问价中引用了test.js文件
      export const a = 1  // test.js
      import {a} from './test.js'  // index.js
      // 以上打包出来的文件会有两个函数，类似如下
      [
          function(module, exports, require) {} // **0**
          function(module, exports, require) {} // **1**
      ]
      ```

      如果使用`scope hoisting`的话会尽量打包成一个函数，在`webpack 4`中只需开启`concatenateModules`即可

      ```javascript
      module.exports = {
          optimize: {
              concatenateModules: true
          }
      }
      ```

   3. **Tree shaking**

      它会删除项目中未被引用的代码，而如果在`webpack 4`中只要开启生产环境就会自动启动这个功能

2. **打包工具的原理**

   - 找出入口文件的所有依赖关系

   - 然后通过构建`CommonJS`代码来获取`exports`导出的内容（注意浏览器是不支持`CommonJS`的，所以需要`bundle`)

     附[官方视频](https://www.youtube.com/watch?v=Gc9-7PBqOC8)，需翻墙，[网友版](https://github.com/yhlben/diy-webpack)

   ***

   #### Babel转换代码（ES6转ES5）

   ​    如何实现`babel`转换代码

   1. 传入文件路径参数，通过`fs`读取文件内容

   2. 通过`babylon`解析代码获取`AST`，目的是为了分析代码中是否还引入了别的文件

   3. 通过`dependencies`来存储文件中的依赖，然后将`AST`转换为`ES5`

   4. 最后该函数返回一个对象，该对象包括当前文件路径、依赖和转换后的代码

      ```shell
      yarn add babylon babel-traverse babel-core babel-preset-env
      ```

      引入这些工具

      ```javascript
      const fs = require('fs')
      const path = require('path')
      const babylon = require('babylon')
      const traverse = require('babel-traverse').default
      const {transforFromAst} = require('babel-core')
      // 实现读取文件代码函数
      function readCode(filePath) {
        // 读取文件内容
          const content = fs.readFileSync(filePath, 'utf-8')
          // 生成 AST
          const ast = babylon.parse(content, {
            sourceType: 'module'
          })
          // 寻找当前文件的依赖关系
          const dependencies = []
          traverse(ast, {
            ImportDeclaration: ({ node }) => {
              dependencies.push(node.source.value)
            }
          })
          // 通过 AST 将代码转为 ES5
          const { code } = transformFromAst(ast, null, {
            presets: ['env']
          })
          return {
            filePath,
            dependencies,
            code
          }
      }
      ```

      未完待续......


2. 深入浅出`Babel`，架构和原理 + 实战

   参考[深入浅出 Babel 上篇：架构和原理 + 实战](<https://juejin.im/post/5d94bfbf5188256db95589be>)

   1. Babel是什么？

      Babel is a JavaScript compiler.即是说，babel是一个`JS`编译器。[官网](<https://babeljs.io/>)

      Babel doesn't do type checking，不做类型检查

      Babel is built out of plugins. Babel由插件构成

      Debuggerable & Compact，尽可能减少代码量而不依赖庞大的运行时。

      Babel支持转换最新的`JavaScript`规范语法，还支持`JSX`，`Typescript`，`Flow`

   2. Babel处理流程

      1. 词法解析：将字符串形式的代码转换为`Token`令牌。
      2. 语法解析：`Token`数组转成`AST`，`AST`会有很多节点类型，如果节点类型很多，那么无疑抽象树也是庞大的，但是，审查解析后的`AST`可以使用强大的`ASTExplorer`。
      3. 转换`AST`:对其进行遍历，这个过程会对节点进行增删改查，Babel所有插件的工作如语法转换和代码压缩等都是在这个阶段进行。
      4. `AST`转换回字符串形式的`JS`,同时还会生成`sourceMap`

   3. Babel架构

      1. 通过[微内核](<https://juejin.im/post/5d7ffad551882545ff173083#heading-10>)实现，核心小，大部分功能通过插件扩展实现。

      2. 架构如图

         ![img](https://user-gold-cdn.xitu.io/2019/10/2/16d8d0cd5a3f3a0c?imageslim)

      3. `@babel/core` 这也是上面说的‘微内核’架构中的‘内核’。主要用来加载和处理配置，加载插件，调用`Parser`进行语法解析，生成`AST`，遍历`AST`，并使用`访问者模式`应用插件对其进行转换，生成字符串代码，包括`Source Map`

      4. 内核周边支持，包括`Parser(@babel/parser)   @babel/traverse   @babel/generator`

      5. 插件支持：语法插件，转换插件，预定义集合(`@babel/presets-*`)

      6. 插件开发辅助：`@babel/template   @babel/helper-&`

      7. 工具：`@babel/node   @babel/register  @babel/cli`

   4. 访问者模式

      `AST`遍历和装换一般会使用访问者模式，

      插件定义的顺序需要考虑向后兼容

   5. 节点的上下文

   6. 副作用的处理：`AST`转换本身会存在副作用，如节点替换过程中，旧的节点被替换了，访问者就没有必要再向下访问了。

   7. 作用域的处理

      `AST`转换的前提是保证程序的正确性 ==> `Scope`

      [AST Explorer](<https://astexplorer.net/>)

   8. 手动实现一个Babel插件

   