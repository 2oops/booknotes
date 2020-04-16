# Babel

1. 定义：工具链，将ES6+的代码转换为向后兼容的JS语法代码，从而可以让项目运行在当前及旧版本浏览器或其他环境中。

2. 主要解决以下问题：

   - 语法转换
   - 通过 `Polyfill` 方式在目标环境中添加缺失的特性(`@babel/polyfill模块`)
   - 源码转换

3. 核心库：@babel/core  命令行工具：@ba bel/cli

4. babel插件分两种：

   转换插件：转换插件会启用相应的语法插件(因此不需要同时指定这两种插件)

   语法插件：只允许babel解析特定类型的语法（不是转换），如在AST转换时可以使用

   ` .babelrc`文件需要的配置项主要有预设(presets)和插件(plugins)，`babel6.X`版本之后，所有的插件都是可插拔的，也就是说只安装babel依然无法正常的工作，我们需要配置对应的.babelrc文件才能起作用。

5. 插件的使用

   ```js
   // 直接写插入名称
   //.babelrc
   {
       "plugins": ["@babel/plugin-transform-arrow-functions"]
   }
   // 或指定插件的相对/绝对路径
   {
       "plugins": ["./node_modules/@babel/plugin-transform-arrow-functions"]
   }
   
   ```

6. presets的妙用

   通过使用或创建一个 `preset` 即可**轻松**使用一组插件。它会根据配置的目标环境，生成插件列表来编译

   @babel/preset-env 主要作用是对当前使用的和目标浏览器缺失的功能进行代码转换和加载polyfill，在不进行任何配置的情况下，其包含的插件将支持除stage阶段的最新特性。

   

