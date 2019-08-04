# Less

Leaner Style Sheets(更精简的样式表)，向后兼容的`CSS`扩展语言。

1. 当某个属性值出现次数较多时，或某个类或`ids`可以重用时

   ```less
   <style lang = "less">
   @width: 100vh;
   .commonClass {
       // #commonClass也可
       border: solid 1px red;
   }
   .container {
       width: @width;
       .commonClass();
   }
   .sider {
       width: @width;
       .commonClass();
   }
   </style>
   ```

2. 嵌套

3. `&:伪选择器`

   ```less
   .clearfix {//经典的clearfix
     display: block;
     zoom: 1;
   
     &:after {
       content: " ";
       display: block;
       font-size: 0;
       height: 0;
       clear: both;
       visibility: hidden;
     }
   }
   ```

4. `@media` or `@supports`冒泡

   ```less
   .component {
     width: 300px;
     @media (min-width: 768px) {
       width: 600px;
       @media  (min-resolution: 192dpi) {
         background-image: url(/img/retina2x.png);
       }
     }
     @media (min-width: 1280px) {
       width: 800px;
     }
   }
   //编译为
   .component {
     width: 300px;
   }
   @media (min-width: 768px) {
     .component {
       width: 600px;
     }
   }
   @media (min-width: 768px) and (min-resolution: 192dpi) {
     .component {
       background-image: url(/img/retina2x.png);
     }
   }
   @media (min-width: 1280px) {
     .component {
       width: 800px;
     }
   }
   ```

5. 运算

   ```less
   @incompatible-units: 12 + 5px - 3cm;//14px 尽量避免此种运算
   @base: 5%;
   @filler: @base * 2; // result is 10%
   @other: @base + @filler; // result is 15%
   
   @color: #224488 / 2; //results in #112244
   background-color: #112244 + #111; // result is #223355
   
   @var: 50vh/2;
   width: calc(50% + (@var - 20px));  // result is calc(50% + (25vh - 20px))
   ```

6. ~符

   ```less
   @min768: ~"(min-width: 768px)";
   .element {
     @media @min768 {
       font-size: 1.2rem;
     }
   }
   //3.5+版本这样写
   @min768: (min-width: 768px);
   .element {
     @media @min768 {
       font-size: 1.2rem;
     }
   }
   //编译为
   @media (min-width: 768px) {
     .element {
       font-size: 1.2rem;
     }
   }
   ```

7. 函数，Less内置多种函数用于转换颜色，处理字符串，算术运算等

8. 命名空间和访问器

   ```less
   #bundle() {
     .button {
       display: block;
       border: 1px solid black;
       background-color: grey;
       &:hover {
         background-color: white;
       }
     }
     .tab { ... }
     .citation { ... }
   }
   
   #header a {
     color: orange;
     #bundle.button();  // can also be written as #bundle > .button，
       //后面加括号是为了它不被输出在命名  空间中
   }
   ```

9. Maps(As of Less 3.5)从less3.5开始

   ```less
   #colors() {
     primary: blue;
     secondary: green;
   }
   
   .button {
     color: #colors[primary];
     border: 1px solid #colors[secondary];
   }
   ```

10. 作用域（从内而外寻找定义的At-rules等）

11. 与`CSS`不同的是支持了块注释和行注释

12. 导入`.less`文件可以省略后缀`@import "library"; // library.less`