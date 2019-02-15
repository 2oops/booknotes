# CSS3

1. CSS3：简化设计，加快载入页面速度，
2. 支持度：Chrome，Safari => (-webkit)，friefox => (-moz)，IE10 => (-ms)，opera => (-o)，360支持大部分特性
3. 颜色支持：HSL，HSLA，CMYK，RGBA
4. 属性：
  * border-radius：左上  右上  右下  左下;  也支持em，百分比，但兼容性不够好

  * box-shadow：X轴偏移量  Y轴偏移量 【阴影模糊半径】【阴影扩展半径】【阴影颜色】【投影方式】
    A. 投影方式省略则为外阴影，inset则为内阴影
    B. 如果需要添加多个阴影，则用逗号隔开即可
    C. 阴影模糊半径：可选，只能是为正值，若值为0，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
    D. 阴影扩展半径：可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
    E. [X, Y]  正正 ，右下两边阴影；正负，右上；负负，左上；负正，左下，可以看作三维立体中以面朝向自己方向为Z轴方向，向下为Y轴正方向，向右为X轴正方向。

  * border-image：border-image:url(borderimg.png) 70 repeat

      * 注意这里70参考边框border的宽度，不带单位，repeat可由round, stretch ,no-repeat等替代，效果不一样。

  * background-color：rgba(255,255,255,0.5)  半透明白色

  * background-image: linear-gradient(to top left,red, yellow,white);
    * 第一个参数，to到A即指从相反方向到A，可省略，省略则默认`to bottom`（180deg)，此外还有`to left rigth`等，颜色参数至少为起始颜色和结束颜色两种，中间可增加颜色种类
    * 除`linear-gradient`之外还有`radial-gradient`，表示径向渐变

  * text-overflow：

    ![img](http://img.mukewang.com/53070cc00001a5bc06000200.jpg)

    + text-overflow只是用来说明文字溢出用什么方式显示，要实现溢出产生省略号的效果，还需要定义强制文本在一行内显示（`white-space：nowrap`) 如下：
    ```css
    text-overflow:ellipsis; 
    overflow:hidden; 
    white-space:nowrap;
    ```
    + word-wrap也可以用来设置文本行为，当前行超过指定容器的边界时是否断开转行

      ![img](http://img.mukewang.com/53070cf700018a2b06000200.jpg)
    * normal为浏览器默认值，break-word设置在长单词或 URL地址内部进行换行，此属性不常用，用浏览器默认值即可。

  * font-face: 用来加载服务器端的字体，让浏览器可以显示用户电脑里没有安装的字体
    ```css
    @font-face {
        font-family: font-name;
        src: 字体文件在服务器上的绝对路径或相对路径
    }
    p {
    	font-size :12px;
    	font-family : "My Font";
    	/*必须项，设置@font-face中font-family同样的值*/
    }
    ```

  * text-shadow: X-Offset Y-Offset blur color; blur为模糊程度

  * background-origin ： border-box | padding-box | content-box;

    * 设置元素背景图片的**原起始位置**

    * 参数分别表示背景图片是从**边框**，还是**内边距（默认值）**，或者是**内容区域**开始显示

  * background-clip ： border-box | padding-box | content-box | no-clip

    * 参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box

  * background-size: auto | <长度值> | <百分比> | cover | contain
    1、auto：默认值，不改变背景图片的原始高度和宽度；

    2、<长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；

    3、<百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；

    4、cover：顾名思义为覆盖，即将背景图片等比缩放以填满整个容器；

    5、contain：容纳，即将背景图片等比缩放至某一边紧贴容器边缘为止。（即至少有三边贴紧容器边缘）

  * background-position
    ```css
    .task {
        width: 300px;
        height: 140px;
        border: 1px solid #999;
    
        background:url(http://static.mukewang.com/static/img/logo_index.png) no-repeat, 
                   url(http://static.mukewang.com/static/img/logo_index.png) no-repeat;
        background-position: center, right bottom;//center是以整个元素为定位对象
        background-size:200px 75px, 160px 60px;
    }
    ```

  * 导航栏设计：[参考](https://www.imooc.com/code/1881)

5. CSS3属性选择器

   ```html
   <a href="xxx.pdf">我链接的是PDF文件</a>
   <a href="#" class="icon">我类名是icon</a>
   <a href="#" title="我的title是more">我的title是more</a>
   ```

   ```css
   a[class^=icon]{
     background: green;
     color:#fff;
   }
   a[href$=pdf]{
     background: orange;
     color: #fff;
   }
   a[title*=more]{
     background: blue;
     color: #fff;
   }
   ```

  * 结构性伪类选择器-root
    `<div>Root修改本块背景颜色</div>`
    `:Root {background:blue;}`

  * 结构性伪类选择器-not

      * ```css
        div:not([id="footer"]){
          background: orange;
        }
        ```

  * empty

    * `:empty`选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格
    * ```css
      div:empty {
        border: 1px solid green;
      }
      ```

  * target

    * ```html
      <div class="menuSection" id="brand">
              <h2><a href="#brand">Brand</a></h2>
              <p>content for Brand</p>
          </div>
      ```

    * ```css
      #brand:target p {
        background: orange;
        color: #fff;
      }
      ```

  * first-child

    * 选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个子元素，记住是子元素，而不是后代元素

  * last-child

  * nth-child(n)

    * :nth-child(n)”选择器中的n为一个表达式时，其中n是从0开始计算，当表达式的值为0或小于0的时候，不选择任何匹配的元素
  * nth-last-child(n) (从某父元素的最后一个子元素开始计算，选择倒数第n个) 
  * 
