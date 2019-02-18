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

  * first-of-type：`:first-of-type`选择器类似于“:first-child”选择器，不同之处就是**指定了元素的类型**,其主要用来定位一个父元素下的某个类型的第一个子元素

  * last-of-type：

    ```css
    .wrapper > div:last-of-type{
     	 background: orange;
    }
    ```

  * nth-last-of-type(n)

  * only-child: 选择器选择的是父元素中只有一个子元素，而且只有唯一的一个子元素

  * only-of-type: 表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的

6. 征服CSS选择器

  * enabled disabled:

      ```css
      input[type="text"]:enabled{
        border: 1px solid #f36;
        box-shadow: 0 0 5px #f36;
      }
      input[type="text"]:disabled{
        	  box-shadow: none;
      }
      ```

  * checked

  * ::selection   `::selection`伪元素是用来匹配突出显示的文本(用鼠标选择文本时的文本)。浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的
    ```html
    <body>
    <p>“::selection”伪元素是用来匹配突出显示的文本。浏览器默认情况下，选择网站文本		是深蓝的背景，白色的字体，
    有的设计要求不使用上图那种浏览器默认的突出文本效果，需要一个与众不同的效果，此		时“::selection”伪元素就非常的实用。不过在Firefox浏览器还需要添加前缀。</p>
    </body>
    ```

    ```css
    ::selection{
      background: orange;
      color: white;
    }
    //firefox需要加前缀
    ::-moz-selection{
      background: orange;
      color: white;
    }
    ```

* read-only：需Html代码中设置`readonly="readonly"`

* read-write: 效果与`read-only`正好相反

* before after   主要用来给元素的前面或后面插入内容，这两个常和"content"配合使用，使用的场景最多的就是清除浮动

  ```css
  .clearfix::before,
  .clearfix::after {
      content: ".";
      display: block;
      height: 0;
      visibility: hidden;
  }
  .clearfix:after {clear: both;}
  .clearfix {zoom: 1;}
  ```
7. CSS变形与动画
  * rotate(45deg)  旋转

  * skew(45deg)  扭曲  `skew(X,Y)`第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则值为0，也就是Y轴方向上无斜切。skewX(),skewY()

  * scale(1.5)  缩放 `scale(X,Y)`  scale()的取值默认的值为1，当值设置为0.01到0.99之间的任何值，作用使一个元素缩小；而任何大于或等于1.01的值，作用是让元素放大  scaleX(),scaleY()

  * translate(X,Y)  把元素从原来的位置移动，而不影响在X、Y轴上的任何Web组件
    ```css
    .wrapper {
      padding: 20px;
      background:orange;
      color:#fff;
      position:absolute;
      top:50%;
      left:50%;
      border-radius: 5px;
      -webkit-transform:translate(-50%,-50%);
      -moz-transform:translate(-50%,-50%);
      transform:translate(-50%,-50%);//垂直水平居中
    }
    ```

  * 矩阵matrix() `matrix(1,0,0,1,50,50)`实现的是`translate(100px,100px)`的效果

  * transform-origin
    ```css
    .transform-origin div {
      -webkit-transform-origin: left top;
      transform-origin: left top;
    }
    ```

  * transition-property 需要过渡状态的属性
    * transition-property:指定过渡或动态模拟的CSS属性
    * transition-timing-function:指定过渡函数
    * transition-delay:指定开始出现的延迟时间

  * transition-duration  过渡所需时间
    ```css
    div {
      width: 300px;
      height: 200px;
      background-color: orange;
      margin: 20px auto;
      -webkit-transition-property: height;
      transition-property: height;
      -webkit-transition-duration？: 1s;
      transition-duration: 1s;
      -webkit-transition-timing-function: ease-out;
      transition-timing-function: ease-out;
      -webkit-transition-delay: .2s;
      transition-delay: .2s;
    }
    div:hover {
      height: 100px;
    }
    ```

  * transition-timing-function  过渡函数
    ```css
    div {
      width: 200px;
      height: 200px;
      background-color: orange;
      margin: 20px auto;
      border-radius: 100%;
      -webkit-transition-property: -webkit-border-radius;
      transition-property: border-radius;
      -webkit-transition-duration: 1s;
      transition-duration: 1s;
      -webkit-transition-timing-function: ease-in-out;
      transition-timing-function: ease-in-out;
      -webkit-transition-delay: .2s;
      transition-delay: .2s;
    }
    div:hover {
      border-radius: 0px;
    }
    ```

  * transition-delay 过渡延迟函数

  * @keyframes chrome和safari需加前缀
    ```css
    @-webkit-keyframes changecolor{
      0%{
    background: red;
      }
      20%{
    background:blue;
      }
      40%{
    background:orange;
      }
      60%{
    background:green;
      }
      80%{
    background:yellow;
      }
      100%{
    background: red;
      }
    }
    div {
      width: 300px;
      height: 200px;
      background: red;
      color:#fff;
      margin: 20px auto;
    }
    div:hover {
      -webkit-animation: changecolor 5s ease-out .2s;
    }
    ```

  * animation-name  主要是用来调用 @keyframes 定义好的动画。需要特别注意: animation-name 调用的动画名需要和“@keyframes”定义的动画名称完全一致（区分大小写），如果不一致将不具有任何动画效果
    ```css
    @keyframes around{
      0% {
    transform: translateX(0);
      }
      25%{
    transform: translateX(180px);
      }
      50%{
     transform: translate(180px, 180px); 
      }
      75%{
    transform:translate(0,180px);
      }
      100%{
    transform: translateY(0);
      }
    }
    div {
      width: 200px;
      height: 200px;
      border: 1px solid red;
      margin: 20px auto;
    }
    div span {
      display: inline-block;
      width: 20px;
      height: 20px;
      background: orange;
      border-radius: 100%;
      animation-name:around;
      animation-duration: 10s;
      animation-timing-function: ease;
      animation-delay: 1s;
      animation-iteration-count:infinite;
    }
    ```

  * animation-duration

  * animation-timing-function
    ```
    animation-timing-function:ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [, ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)]*
    ```

  * animation-delay

  * animation-iteration-count 动画播放次数

  * animation-direction 
    *Chrome和safari需前缀，主要有两个值，`normal`和`alternate`，前者为默认，动画每次播放都是向前播放，后者动画播放在第偶数次向前，第奇数次向反方向。

  * animation-play-state 

    * 主要用来控制元素动画的播放状态，主要有两个值：`running`和`paused`，前者为默认值

  * animation-fill-mode

    | 默认值    | 效果                                                         |
    | --------- | ------------------------------------------------------------ |
    | none      | 默认值，表示动画将按预期进行和结束，在动画完成其最后一帧时，动画会反转到初始帧处 |
    | forwards  | 表示动画在结束后继续应用最后的关键帧的位置                   |
    | backwords | 会在向元素应用动画样式时迅速应用动画的初始帧                 |
    | both      | 元素动画同时具有forwards和backwards效果                      |

  * 3D旋转导航 [参考](https://www.imooc.com/code/1883)
8. 布局样式
  * columns
    *` column-width: auto | <200px>`
    * `column-count: auto | <integer>`
    * `columns-gap: normal | 2em`
    * column-rule  主要是用来定义列与列之间的边框宽度、边框样式和边框颜色* 规则：`column-rule:<column-rule-width>|<column-rule-style>|<column-rule-color>`
    * `column-span: none | all`
  * 盒子模型
    * box-sizing
    ```css
    #footer {
      -webkit-box-sizing: border-box;
      -moz--box-sizing: border-box;
      -o--box-sizing: border-box;
      -ms--box-sizing: border-box;
      box-sizing:border-box;
    }
    ```
  * 伸缩布局 flexbox
    1. 屏幕和浏览器窗口大小发生改变也可以灵活调整布局
    2. 可以指定伸缩项目沿着主轴或侧轴按比例分配额外空间（伸缩容器额外空间），从而调整伸缩项目的大小
    2. 可以指定伸缩项目沿着主轴或侧轴将伸缩容器额外空间，分配到伸缩项目之前、之后或之间
    4. 可以控制元素在页面上的布局方向
    5. 可以指定如何将垂直于元素布局轴的额外空间分布到该元素的周围
    6. 可以按照不同于文档对象模型（DOM）所指定排序方式对屏幕上的元素重新排序。也就是说可以在浏览器渲染中不按照文档流先后顺序重排伸缩项目顺序
    * `.flexcontainer{ display: -webkit-flex; display: flex; }`
    * 通过`flex-direction`来改变主轴方向修改为`column`，其默认值是`row`
    * 如何将flex项目移动到顶部，取决于主轴的方向。如果它是垂直的方向通过`align-items`设置；如果它是水平的方向通过`justify-content`设置
    * flex项目称动到左边或右边也取决于主轴的方向。如果`flex-direction`设置为`row`，设置`justify-content`控制方向；如果设置为`column`，设置`align-items`控制方向
    * 水平垂直居中： 设置`justify-content`或者`align-items`为`center`。另外根据主轴的方向设置`flex-direction`为`row`或`column`
    ```css
    body {
      display:flex;
      align-items: center;
      justify-content:center;
    }
    ```
    * 实现自动伸缩： 定义一个flex项目，如何相对于flex容器实现自动的伸缩。需要给每个flex项目设置flex属性设置需要伸缩的值(`flex：200`)
9. Media Queries 与Responsive 设计
  * Screen、All和Print为最常见的三种媒体类型
  * 媒体类型的引用方法也有多种，常见的有：link标签、@import和CSS3新增的@media(媒体查询)
    ```css
    @media screen and (max-width:480px){
     .ads {
       display:none;//当屏幕小于或等于480px时,页面中的广告区块（.ads）都将被隐藏
      }
    }
    ```

  * Responsive设计  简称RWD，不是流体布局，也不是网格布局，而是一种独特的网页设计方法
    * `Responsive`设计最关注的就是：根据用户的使用设备的当前宽度，你的Web页面将加载一个备用的样式，实现特定的页面风格
    * `Media Queries`中，其中媒体特性`min-width`和`max-width`对应的属性值就是响应式设计中的断点值。设置断点应把握三个要点：满足主要的断点；有可能的话添加一些别的断点；设置高于1024的桌面断点
    * <meta name=”viewport” content=”” />

      ![img](http://img.mukewang.com/53660f2c0001190005270386.jpg)
10. 用户界面
  * `resize`,允许用户通过拖动的方式来修改元素的尺寸来改变元素的大小。到目前为止，可以使用`overflow`属性的任何容器元素
    * `resize: none | both | horizontal | vertical | inherit`
    ```css
    textarea{
      resize:none | both | horizontal | vertical | inherit;
      background:green;
    }
    ```

  * `outline: ［outline-color］ || [outline-style] || [outline-width] || [outline-offset] || inherit`

    | 属性值         | 说明                                                         |
    | -------------- | ------------------------------------------------------------ |
    | outline-color  | 定义轮廓线的颜色，属性值为CSS中定义的颜色值。在实际应用中，可以将此参数省略，省略时此参数的默认值为黑色。 |
    | outline-style  | 定义轮廓线的样式，属性为CSS中定义线的样式。在实际应用中，可以将此参数省略，省略时此参数的默认值为none，省略后不对该轮廓线进行任何绘制。 |
    | outline-width  | 定义轮廓线的宽度，属性值可以为一个宽度值。在实际应用中，可以将此参数省略，省略时此参数的默认值为medium，表示绘制中等宽度的轮廓线。 |
    | outline-offset | 定义轮廓边框的偏移位置的数值，此值可以取负数值。当此参数的值为正数值，表示轮廓边框向外偏离多少个像素；当此参数的值为负数值，表示轮廓边框向内偏移多少个像素。 |
    | inherit        | 元素继承父元素的outline效果。                                |

  * CSS生成内容
    ```css
    h1:after{
      content:"我是被插进来的";
      color: red;
    }
    ```
  * 3D旋转视频展示区 [参考](https://www.imooc.com/code/1963)