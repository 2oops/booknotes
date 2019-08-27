# Base

1. 本质上，webpack是一个现代的JavaScript应用程序的静态模块打包器（module bundler)，打包时webpack会递归地构建一个依赖关系图，其中包含应用程序需要的每一个模块，然后会将这些模块打包成一个或多个bundle。

   1. 入口`entry`，可指定一个或多个，webpack会找出直接和间接依赖的模块和库

      - `entry`接受字符串，字符串数组和对象语法。对象语法的可扩展性最佳，单页应用和多页应用都可。
      - 使用`CommonsChunkPlugin`为每个页面间的应用程序共享代码创建bundle，由于入口起点增多，多页应用能复用入口起点之间的大量代码/模块，从而收益很大。

   2. `output`更多参数配置：<https://www.webpackjs.com/configuration/output/>

      - 多个入口起点
      - cdn和资源hash

   3. `loader`能让webpack处理非JavaScript文件，因为它本身只能处理js文件。`test`和`use`是说在`import()/require()`语句中被解析为某种文件路径时，使用`loader`转换一下。以下有三种使用loader的方式：

      - 配置[webpack.config.js]

      - 内联：import语句中显式指定loader

        `import Styles from 'style-loader!css-loader?modules!./styles.css';`

      - CLI: shell命令中指定

        `webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'

      loader运行在`Node.js`中，能够执行任何可能的操作；

      可在`package.json`文件中定义一个loader字段；

      可同步和异步；

      loader模块需要导出一个函数。

   4. `plugins`插件执行的是比`loader`范围更广的任务，可以用来处理各种各样的任务。

      1. webpack插件是一个具有`apply`属性的JavaScript对象，`apply`属性会被`webpack compiler`调用，并且该对象可在整个编译生命周期访问。

         ```javascript
         const pluginName = 'ConsoleOnBuildWebpackPlugin'
         class ConsoleOnBuildWebpackPlugin {
             apply(compiler) {
                 complier.hooks.run.tap(pluginName, compilation => {
                     console.log('webpack开始构建');
                 })
             }
         }
         ```

      2. 

   5. `mode`

   6. ```javascript
      const path = require("path");
      const HtmlWebpackPlugin = require('html-webpack-plugin');
      
      module.exports = {
          entry: {
              app: './src/app.js',
              vendor: './src/vendor.js'
          },
          output: {
      		publicPath: '/',
               path: path.resolve(__dirname, 'dist'),//必填
               filename: 'bundle.js'//必填
          },
          module: {
              rules: [
                  {
                      test: /\.vue$/,
                      loader: 'vue-loader',
                      options: {
                          extractCSS: true
                      }
                  },
                  {
                      test: /\.less$/,
                      loader: ['style-loader', 'css-loader', 'less-loader']//注意顺序
                  }
              ]
          },
          plugins: [
              new HtmlWebpackPlugin({template: './src/index.html'})
          ],
          mode: 'production'
      };
      ```

2. 