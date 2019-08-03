# Scss

`Scss`为`Sass`两种语法之一，是一个CSS3`语法的扩充版本。Sass则是对CSS的扩展，允许使用变量，嵌套规则，mixins，导入等功能，并且完全兼容CSS语法。

 Sass 有助于保持大型样式表结构良好， 同时也让你能够快速开始小型项目， 特别是在搭配 [Compass 样式库](http://compass-style.org/)一同使用时。

1. vue项目中使用sass

   ```
   npm install node-sass --save-dev
   npm install sass-loader --save-dev
   //报错可使用cnpm
   npm install -g cnpm --registry=https://registry.npm.taobao.org  （安装淘宝镜像）
   cnpm install node-sass  --save （使用淘宝镜像安装node-sass）
   ```

   若还遇到报错[参考](https://www.cnblogs.com/crazycode2/p/6535105.html)

2. `$width: 5em;`区别于`less`

   ```scss
   #main {
     width: $width;
   }
   ```

