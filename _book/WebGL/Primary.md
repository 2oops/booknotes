# Primary

1. GLSL`强类型语言

2. html文件至少需要包含一个`canvas`标签，另外需要两个存储`着色器源码`(顶点，片元)的`<script>`标签

3. 

   1. 获取WebGL绘图环境

      ```js
      var canvas = document.querySelector(#canvas);
      var gl = canvas.getContext('webgl') || canvas.getContext("experimental-webgl");//加实验前缀做浏览器兼容处理
      ```

   2. 创建顶点着色器对象

      ```js
      // 获取顶点着色器源码
      var vertexShaderSource = document.querySelector('#vertexShader').innerHTML;
      // 创建顶点着色器对象
      var vertexShader = gl.createShader(gl.VERTEX_SHADER);
      // 将源码分配给顶点着色器对象
      gl.shaderSource(vertexShader, vertexShaderSource);
      // 编译顶点着色器程序
      gl.compileShader(vertexShader);
      ```

   3. 创建片元着色器

      ```js
      // 获取片元着色器源码
      var fragmentShaderSource = document.querySelector('#fragmentShader').innerHTML;
      // 创建片元着色器程序
      var fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);
      // 将源码分配给片元着色器对象
      gl.shaderSource(fragmentShader, fragmentShaderSource);
      // 编译片元着色器
      gl.compileShader(fragmentShader);
      ```

   4. 创建着色器程序

      ```js
      var program = gl.createProgram();
      //将顶点着色器挂载在着色器程序上。
      gl.attachShader(program, vertexShader); 
      //将片元着色器挂载在着色器程序上。
      gl.attachShader(program, fragmentShader);
      //链接着色器程序
      gl.linkProgram(program);
      ```

   5. `attribue` 变量：只能在`顶点着色器`中定义；uniform 变量：既可以在`顶点着色器`中定义，也可以在`片元着色器中`定义；最后一种变量类型 `varing` 变量：它用来从`顶点着色器`中往`片元着色器`传递数据。使用它我们可以在顶点着色器中声明一个变量并对其赋值，经过插值处理后，在片元着色器中取出插值后的值来使用。

4. 使用缓冲区向GPU传递数据