# Css

#### CSS3

1. CSS3：简化设计，加快载入页面速度，
2. 支持度：Chrome，Safari => (-webkit)，friefox => (-moz)，IE10 => (-ms)，opera => (-o)，360支持大部分特性
3. 颜色支持：HSL，HSLA，CMYK，RGBA
4. 属性：

- border-radius：左上  右上  右下  左下;  也支持em，百分比，但兼容性不够好

- box-shadow：X轴偏移量  Y轴偏移量 【阴影模糊半径】【阴影扩展半径】【阴影颜色】【投影方式】
  A. 投影方式省略则为外阴影，inset则为内阴影
  B. 如果需要添加多个阴影，则用逗号隔开即可
  C. 阴影模糊半径：可选，只能是为正值，若值为0，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
  D. 阴影扩展半径：可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
  E. [X, Y]  正正 ，右下两边阴影；正负，右上；负负，左上；负正，左下，可以看作三维立体中以面朝向自己方向为Z轴方向，向下为Y轴正方向，向右为X轴正方向。

- border-image：border-image:url(borderimg.png) 70 repeat

  - 注意这里70参考边框border的宽度，不带单位，repeat可由round, stretch ,no-repeat等替代，效果不一样。

- background-color：rgba(255,255,255,0.5)  半透明白色

- background-image: linear-gradient(to top left,red, yellow,white);

  - 第一个参数，to到A即指从相反方向到A，可省略，省略则默认`to bottom`（180deg)，此外还有`to left rigth`等，颜色参数至少为起始颜色和结束颜色两种，中间可增加颜色种类
  - 除`linear-gradient`之外还有`radial-gradient`，表示径向渐变

- text-overflow：

  ![img](http://img.mukewang.com/53070cc00001a5bc06000200.jpg)

  - text-overflow只是用来说明文字溢出用什么方式显示，要实现溢出产生省略号的效果，还需要定义强制文本在一行内显示（`white-space：nowrap`) 如下：

  ```css
  text-overflow:ellipsis; 
  overflow:hidden; 
  white-space:nowrap;
  ```

  - word-wrap也可以用来设置文本行为，当前行超过指定容器的边界时是否断开转行

    ![img](http://img.mukewang.com/53070cf700018a2b06000200.jpg)

  - normal为浏览器默认值，break-word设置在长单词或 URL地址内部进行换行，此属性不常用，用浏览器默认值即可。

- font-face: 用来加载服务器端的字体，让浏览器可以显示用户电脑里没有安装的字体

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

- text-shadow: X-Offset Y-Offset blur color; blur为模糊程度

- background-origin ： border-box | padding-box | content-box;

  - 设置元素背景图片的**原起始位置**
  - 参数分别表示背景图片是从**边框**，还是**内边距（默认值）**，或者是**内容区域**开始显示

- background-clip ： border-box | padding-box | content-box | no-clip

  - 参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。no-clip表示不裁切，和参数border-box显示同样的效果。backgroud-clip默认值为border-box

- background-size: auto | <长度值> | <百分比> | cover | contain
  1、auto：默认值，不改变背景图片的原始高度和宽度；

  2、<长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；

  3、<百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；

  4、cover：顾名思义为覆盖，即将背景图片等比缩放以填满整个容器；

  5、contain：容纳，即将背景图片等比缩放至某一边紧贴容器边缘为止。（即至少有三边贴紧容器边缘）

- background-position

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

- 导航栏设计：[参考](https://www.imooc.com/code/1881)

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

- 结构性伪类选择器-root
  `<div>Root修改本块背景颜色</div>`
  `:Root {background:blue;}`

- 结构性伪类选择器-not

  - ```css
    div:not([id="footer"]){
      background: orange;
    }
    ```

- empty

  - `:empty`选择器表示的就是空。用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格

  - ```css
    div:empty {
      border: 1px solid green;
    }
    ```

- target

  - ```html
    <div class="menuSection" id="brand">
            <h2><a href="#brand">Brand</a></h2>
            <p>content for Brand</p>
        </div>
    ```

  - ```css
    #brand:target p {
      background: orange;
      color: #fff;
    }
    ```

- first-child

  - 选择器表示的是选择父元素的第一个子元素的元素E。简单点理解就是选择元素中的第一个子元素，记住是子元素，而不是后代元素

- last-child

- nth-child(n)

  - :nth-child(n)”选择器中的n为一个表达式时，其中n是从0开始计算，当表达式的值为0或小于0的时候，不选择任何匹配的元素

- nth-last-child(n) (从某父元素的最后一个子元素开始计算，选择倒数第n个) 

- first-of-type：`:first-of-type`选择器类似于“:first-child”选择器，不同之处就是**指定了元素的类型**,其主要用来定位一个父元素下的某个类型的第一个子元素

- last-of-type：

  ```css
  .wrapper > div:last-of-type{
   	 background: orange;
  }
  ```

- nth-last-of-type(n)

- only-child: 选择器选择的是父元素中只有一个子元素，而且只有唯一的一个子元素

- only-of-type: 表示一个元素他有很多个子元素，而其中只有一种类型的子元素是唯一的

6. 征服CSS选择器

- enabled disabled:

  ```css
  input[type="text"]:enabled{
    border: 1px solid #f36;
    box-shadow: 0 0 5px #f36;
  }
  input[type="text"]:disabled{
    	  box-shadow: none;
  }
  ```

- checked

- ::selection   `::selection`伪元素是用来匹配突出显示的文本(用鼠标选择文本时的文本)。浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的

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

- read-only：需Html代码中设置`readonly="readonly"`

- read-write: 效果与`read-only`正好相反

- before after   主要用来给元素的前面或后面插入内容，这两个常和"content"配合使用，使用的场景最多的就是清除浮动

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

- rotate(45deg)  旋转

- skew(45deg)  扭曲  `skew(X,Y)`第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则值为0，也就是Y轴方向上无斜切。skewX(),skewY()

- scale(1.5)  缩放 `scale(X,Y)`  scale()的取值默认的值为1，当值设置为0.01到0.99之间的任何值，作用使一个元素缩小；而任何大于或等于1.01的值，作用是让元素放大  scaleX(),scaleY()

- translate(X,Y)  把元素从原来的位置移动，而不影响在X、Y轴上的任何Web组件

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

- 矩阵matrix() `matrix(1,0,0,1,50,50)`实现的是`translate(100px,100px)`的效果

- transform-origin

  ```css
  .transform-origin div {
    -webkit-transform-origin: left top;
    transform-origin: left top;
  }
  ```

- transition-property 需要过渡状态的属性

  - transition-property:指定过渡或动态模拟的CSS属性
  - transition-timing-function:指定过渡函数
  - transition-delay:指定开始出现的延迟时间

- transition-duration  过渡所需时间

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

- transition-timing-function  过渡函数

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

- transition-delay 过渡延迟函数

- @keyframes chrome和safari需加前缀

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

- animation-name  主要是用来调用 @keyframes 定义好的动画。需要特别注意: animation-name 调用的动画名需要和“@keyframes”定义的动画名称完全一致（区分大小写），如果不一致将不具有任何动画效果

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

- animation-duration

- animation-timing-function

  ```
  animation-timing-function:ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>) [, ease | linear | ease-in | ease-out | ease-in-out | cubic-bezier(<number>, <number>, <number>, <number>)]*
  
  ```

- animation-delay

- animation-iteration-count 动画播放次数

- animation-direction 
  *Chrome和safari需前缀，主要有两个值，`normal`和`alternate`，前者为默认，动画每次播放都是向前播放，后者动画播放在第偶数次向前，第奇数次向反方向。

- animation-play-state 

  - 主要用来控制元素动画的播放状态，主要有两个值：`running`和`paused`，前者为默认值

- animation-fill-mode

  | 默认值    | 效果                                                         |
  | --------- | ------------------------------------------------------------ |
  | none      | 默认值，表示动画将按预期进行和结束，在动画完成其最后一帧时，动画会反转到初始帧处 |
  | forwards  | 表示动画在结束后继续应用最后的关键帧的位置                   |
  | backwords | 会在向元素应用动画样式时迅速应用动画的初始帧                 |
  | both      | 元素动画同时具有forwards和backwards效果                      |

- 3D旋转导航 [参考](https://www.imooc.com/code/1883)

8. 布局样式

- columns
  *` column-width: auto | <200px>`

  - `column-count: auto | <integer>`
  - `columns-gap: normal | 2em`
  - column-rule  主要是用来定义列与列之间的边框宽度、边框样式和边框颜色* 规则：`column-rule:<column-rule-width>|<column-rule-style>|<column-rule-color>`
  - `column-span: none | all`

- 盒子模型

  - box-sizing

  ```css
  #footer {
    -webkit-box-sizing: border-box;
    -moz--box-sizing: border-box;
    -o--box-sizing: border-box;
    -ms--box-sizing: border-box;
    box-sizing:border-box;
  }
  
  ```

- 伸缩布局 flexbox

  1. 屏幕和浏览器窗口大小发生改变也可以灵活调整布局
  2. 可以指定伸缩项目沿着主轴或侧轴按比例分配额外空间（伸缩容器额外空间），从而调整伸缩项目的大小
  3. 可以指定伸缩项目沿着主轴或侧轴将伸缩容器额外空间，分配到伸缩项目之前、之后或之间
  4. 可以控制元素在页面上的布局方向
  5. 可以指定如何将垂直于元素布局轴的额外空间分布到该元素的周围
  6. 可以按照不同于文档对象模型（DOM）所指定排序方式对屏幕上的元素重新排序。也就是说可以在浏览器渲染中不按照文档流先后顺序重排伸缩项目顺序

  - `.flexcontainer{ display: -webkit-flex; display: flex; }`
  - 通过`flex-direction`来改变主轴方向修改为`column`，其默认值是`row`
  - 如何将flex项目移动到顶部，取决于主轴的方向。如果它是垂直的方向通过`align-items`设置；如果它是水平的方向通过`justify-content`设置
  - flex项目称动到左边或右边也取决于主轴的方向。如果`flex-direction`设置为`row`，设置`justify-content`控制方向；如果设置为`column`，设置`align-items`控制方向
  - 水平垂直居中： 设置`justify-content`或者`align-items`为`center`。另外根据主轴的方向设置`flex-direction`为`row`或`column`

  ```css
  body {
    display:flex;
    align-items: center;
    justify-content:center;
  }
  
  ```

  - 实现自动伸缩： 定义一个flex项目，如何相对于flex容器实现自动的伸缩。需要给每个flex项目设置flex属性设置需要伸缩的值(`flex：200`)

9. Media Queries 与Responsive 设计

- Screen、All和Print为最常见的三种媒体类型

- 媒体类型的引用方法也有多种，常见的有：link标签、@import和CSS3新增的@media(媒体查询)

  ```css
  @media screen and (max-width:480px){
   .ads {
     display:none;//当屏幕小于或等于480px时,页面中的广告区块（.ads）都将被隐藏
    }
  }
  
  ```

- Responsive设计  简称RWD，不是流体布局，也不是网格布局，而是一种独特的网页设计方法

  - `Responsive`设计最关注的就是：根据用户的使用设备的当前宽度，你的Web页面将加载一个备用的样式，实现特定的页面风格

  - `Media Queries`中，其中媒体特性`min-width`和`max-width`对应的属性值就是响应式设计中的断点值。设置断点应把握三个要点：满足主要的断点；有可能的话添加一些别的断点；设置高于1024的桌面断点

  - <meta name=”viewport” content=”” />

    ![img](http://img.mukewang.com/53660f2c0001190005270386.jpg)

10. 用户界面

- `resize`,允许用户通过拖动的方式来修改元素的尺寸来改变元素的大小。到目前为止，可以使用`overflow`属性的任何容器元素

  - `resize: none | both | horizontal | vertical | inherit`

  ```css
  textarea{
    resize:none | both | horizontal | vertical | inherit;
    background:green;
  }
  
  ```

- `outline: ［outline-color］ || [outline-style] || [outline-width] || [outline-offset] || inherit`

  | 属性值         | 说明                                                         |
  | -------------- | ------------------------------------------------------------ |
  | outline-color  | 定义轮廓线的颜色，属性值为CSS中定义的颜色值。在实际应用中，可以将此参数省略，省略时此参数的默认值为黑色。 |
  | outline-style  | 定义轮廓线的样式，属性为CSS中定义线的样式。在实际应用中，可以将此参数省略，省略时此参数的默认值为none，省略后不对该轮廓线进行任何绘制。 |
  | outline-width  | 定义轮廓线的宽度，属性值可以为一个宽度值。在实际应用中，可以将此参数省略，省略时此参数的默认值为medium，表示绘制中等宽度的轮廓线。 |
  | outline-offset | 定义轮廓边框的偏移位置的数值，此值可以取负数值。当此参数的值为正数值，表示轮廓边框向外偏离多少个像素；当此参数的值为负数值，表示轮廓边框向内偏移多少个像素。 |
  | inherit        | 元素继承父元素的outline效果。                                |

- CSS生成内容

  ```css
  h1:after{
    content:"我是被插进来的";
    color: red;
  }
  
  ```

- 3D旋转视频展示区 [参考](https://www.imooc.com/code/1963)

#### Grid布局

1. Grid布局的优势

   1. 固定或弹性的轨道尺寸
   2. 定位项目
   3. 创建额外的轨道来保存内容
   4. 对齐控制
   5. 控制重叠内容（类似z-index）
   6. 配合`flexbox`使用`grid`

2. 当元素设置了网格布局，`column,float,clear,vertical-align`属性无效
   `display: subgrid`目前所有浏览器不兼容

3. 网格线比网格项多一条

   ```css
   .container {
     display: grid;
     grid-template-columns: 100px 150px 200px 3fr 2fr; 
     /* 未确定宽度的情况下auto默认为0 */
     /* 列属性值不够布局完网格项则自动增加auto */
     grid-template-rows: 1fr;
   }
   ```

4. 网格区域`grid-template-areas`

5. grid-column-gap, grid-row-gap=>gap: 50px 100px(行列)

6. justify-items: start/end/center, align-items: start/end/center => place-items: 20px 30px(列行)

7. `start`和`stretch`的区别，在宽度固定的情况下基本无区别，宽度`auto`时start使该网格项最窄，stretch使其最宽

   justify-content: start/stretch;

   center: 整个网格在容器的中间

   space-around: 在网格项之间设置均宽空白间隙，外边缘间隙大小为中间空白间隙宽度的一半

   space-between: 在网格项之间设置均宽空白间隙，外边缘无间隙

   space-evenly: 在网格项之间设置均宽空白间隙，包括外边缘

   垂直和水平方向均有以上七个属性，可以直接使用`place-content: space-evenly`实现垂直水平居中，该属性两参数以列宽顺序排列

8. grid-auto隐式轨道=>不建议使用

9. repeat, fit-content, minmax

   1. `grid-template-columns: repeat(5, 1fr)`只可以使用在`grid-template-columns`和`grid-template-rows`这两个属性上；
      `repeat()`中的重复次数<auto-fll>以网格项为准自动填充，<auto-fit>以网格容器为准自动填充
      value值可以使用非负长度，百分比，<flex>(即fr), max-content, min-content,auto
      `repeat()`只是简化部分属性的写法=> `grid-template-columns: repeat(5, 1fr) 100px 200px`
   2. `fit-content`同样只能在columns和rows两个属性上使用
      `grid-template-columns: 100px fit-content(200px) fit-content(200px)`
   3. `minmax(A, B)`定义了一个范围，若A>B则以A为最大值

10. 网格项的属性：start-end，grid-area，self

    1. `grid-column-start: 2;grid-column-end: 4`

    2. span<number>

    3. span<name>

    4. ```css
       grid-column: 2 / span 3;//往后跨三行
         /* grid-column-end: 5; */
         grid-row: 2 / span 2;
         /* grid-row-end: 4; */
       ```

    5. 注意最好不要生成隐式网格轨道

    6. `grid-area`

    7. `gird-area: row-start1 / 2 / 3 /five;`
       `<row-start> / <column-start> / <row-end> / <column-end>`可以是number和name

    8. 使用`grid-template-areas`时，所形成的区域须是矩形

       ```css
       grid-template-areas:
         "device customers map province value"
         "industry customers map province weight"
         "tops customers map province height"
         "reminder totaltime statistics runtime trips";
         //就是说如map,customers,province所形成的区域必须是矩形，否则报错
       ```

    9. self: 沿着行轴对其grid item里的内容

       ```css
       .item {
           justify-self: <start> | <end> | <center> | <stretch>;
           align-self: ;
       }
       //example:
       .container > .first {
         grid-area: 2 / 2 / span 2 / span 3;
         justify-self: center;
         align-self: center;
         background-color: #aaaaaa;
       }
       ```

#### Css3进阶

1. `float`和`absolute`是兄弟关系

2. ```css
   .cover { //无固定width/height容器内的绝对定位元素拉伸
   	position: absolute; 
   	left: 0; top: 0; right: 0; bottom: 0;
   	background-color: #fff;
   	opacity: .5; filter: alpha(opacity=50);
   }
   
   .overlay { //没有宽度和高度声明实现的全屏自适应效果
       position: absolute;
   	left: 0; top: 0; right: 0; bottom: 0;
   	background-color: #000;
   	opacity: .5; filter: alpha(opacity=50);
   }
   ```

3. ```html
   <!doctype html> //高度自适应的九宫格效果
   <html>
   <head>
   <meta charset="utf-8">
   <title>高度自适应的九宫格效果</title>
   <style>
   html, body { height: 100%; margin: 0; }
   .page {
       position: absolute;
   	left: 0; top: 0; right: 0; bottom: 0;
   }
   .list {
   	float: left;
   	height: 33.3%; width: 33.3%;
   	position: relative;
   }
   .list:before {
   	content: '';
   	position: absolute;
   	left: 10px; right: 10px; top: 10px; bottom: 10px;
   	border-radius: 10px;
   	background-color: #cad5eb;
   }
   .list:after {
   	content:attr(data-index);
   	position: absolute;
   	height: 30px;
   	left: 0; right: 0; top: 0; bottom: 0;
   	margin: auto;
   	text-align: center;
   	font: 24px/30px bold 'microsoft yahei';
   }
   </style>
   </head>
   
   <body>
   <div class="page">
   	<div class="list" data-index="1"></div>
       <div class="list" data-index="2"></div>
       <div class="list" data-index="3"></div>
       <div class="list" data-index="4"></div>
       <div class="list" data-index="5"></div>
       <div class="list" data-index="6"></div>
       <div class="list" data-index="7"></div>
       <div class="list" data-index="8"></div>
       <div class="list" data-index="9"></div>
   </div>
   </body>
   </html>
   ```

4. ```css
   .image {
       position: absolute; left: 0; right: 0; width: 50%; //width>拉伸
       margin:auto;//添加改行后实现绝对定位居中布局
   }
   ```

5. ```css
   .content {  //整体布局
       position: absolute;
       top: 48px;
       bottom: 52px;
       left: 252px;//如果有侧边栏
       overflow: auto;
   }
   ```

6. ```css
   .overlay {
       position: absolute;
       top: 0;right: 0;bottom: 0;left: 0;
       background-color: rgba(0,0,0,.5);
       z-index: 9;
   }
   
   <div class="page"></div>
   <div class="overlay"></div>
   ```



------

7. `relative`

   1. 限制定位`left,right,top,bottom`，限制`z-index`(z-index设置为auto时在IE7+对内部元素不起限制作用)，限制`overflow`,relative限制不了`position:fixed`
   2. relative相对自身定位，`left，right`同时存在时绝对定位实现拉伸，相对定位则斗争。(left>right,top>bottom)
   3. 层叠上下文
   4. relative最小化影响原则：relative最小化，尽量避免使用relative

8. `margin`

   1. ```css
      .list {
          margin-top: 15px;
          margin-bottom: 15px;
      }
      ```

   2. relative中使用百分比margin以宽度为准，absolute中使用百分比margin以上一定位元素为准

   3. ```css
      margin-left: auto;
      margin-right: auto;//两者都为auto时实现居中效果，但图片（block）不居中
      ```

   4. `width/height`限制了absolute元素自动填满容器

   5. ```css
      position: absolute;
      margin: auto;//自动平分被变更的尺寸空间实现居中
      ```

   6. margin负值可实现两端对齐`margin-left: -20px; margin-right: 20px;`

   7. 负值下实现等高布局`margin-bottom: -20px; padding-bottom: 20px;`必须使用`overflow: hidden`限制

   8. 负值下实现两栏自适应

   9. margin重叠  `margin-start,margin-end`，设置`direction: ltl`，`writing-mode: vertical-lr;`

9. padding

   1. 水平padding影响尺寸，垂直padding不影响尺寸，但会影响背景色（占据空间）
   2. 使用百分比`div{padding: 50%}`构建固定比例布局结构
   3. 配合margin实现等高布局`margin-bottom: 10px; padding-bottom: 10px`
   4. 实现两栏自适应布局

10. overflow

    ```
              1. IE8+等浏览器默认`html {overflow: auto}`，想要去除默认滚动条时设置为hidden即可
              2. ![1552487178590](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552487178590.png)
              3. 滚动条宽度约为17px
              4. 水平居中跳动问题`.container {width: 1150px; margin: 0 auto;}`，修复方法:添加一行：`padding-left: calc(100vw - 100%)` 100%指可用内容宽度，注意减号两边是空格
              5. ![1552487743420](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552487743420.png)
              6. ![1552487779330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552487779330.png)
              7. ![1552571192168](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552571192168.png)
              8. ![1552571832530](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552571832530.png)
              9. overflow与`position: relative`结合使用防overflow失效
              10. ![1552572117691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552572117691.png)
              11. ![1552572190052](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552572190052.png)
              12. ![1552572604540](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552572604540.png)
    ```

11. border

    ```
                  1. `border-width`不支持百分比，但是有三个参数可选thin(1px),medium(3px默认)，thick(5px)
                  2. border-style: dashed/dotted/double/inset/outset/ridge;
                  3. ![1552574715031](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552574715031.png)
                  4. ![1552574841846](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552574841846.png)
                  5. ![1552574994490](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552574994490.png)
                  6. ![1552575201992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552575201992.png)
                  7. ![1552575246183](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552575246183.png)
                  8. ![1552575522139](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552575522139.png)
                  9. ![1552575668073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552575668073.png)
                  10. border可实现等高布局（局限：不支持百分比宽度）
    ```

12. line-height

    1. 基线之间的距离即行高（实现的居中并非真正的居中）

    2. input元素中加入`line-height: inherit`增加可控性

    3. ```css
       body {//body全局数值行高实践
            font-size: 14px;
            line-height: 1.4286；
            /font: 14px/1.4286;
             }
       ```

    4. 消除图片底部间隙的方法：A  图片块状化`img {display: block;}`   B 图片底线对齐  `img { vertical-align: bottom;}`  C  行高足够小，基线位置上移`.box {line-height: 0;}`

    5. ###### ######实际应用######

    6. 图片水平居中`.box {line-height: 300px; text-align: center} .box > img {vertical-align: middle;}`

    7. ```css
       //多行文本水平居中
       .box {
           line-height: 300px;
           text-align: center;
       }
       .box > text {
           display: inline-block;
           line-height: normal;
           text-align: left;
           vertical-align: middle;
       }
       ```

13. float

    1. 浮动的本身设计是为了实现文字环绕效果

    2. 伪元素的处理

       ```css
       .clearfix:after {
           content: '';
           display: none;
           clear: both;
       }
       .clearfix {
           *zoom: 1;//兼容IE6,IE7
       }
       ```

    3. float和clear均为left时文本行只显示文字长度

14. z-index

    1. z-index在position为非static时才有用
    2. 无嵌套时遵循后来居上原则，有嵌套时遵循祖先优先原则，前提是z-index为数值，而不是auto
    3. ![1552921399583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1552921399583.png)
    4. 对于非浮层元素，避免设置z-index值，z-index值不超过2

15. vertical-align

    1. 其百分比值是相对于line-height计算的
    2. `vertical-align: middle;`此时图片为近似垂直居中，要实现完全垂直居中加`font-size: 0`即可
    3. `vertical-align: text-top/text-bottom;`前者为盒子的顶部和父级content area 的顶部对齐，后者……底部
    4. 应用：表情图片（或原始尺寸背景图标）与文字的对齐效果--（使用文本底部较为合适，不受行高及其他内联元素影响）
    5. hmtl中的上标和下标（sup,sub)
    6. 兼容IE8+
    7. 实际应用：vertical-align取负值
    8. 图片居中：inline-block化，0宽度100%高度辅助元素，vertical-align: middle
    9. ![1553011503097](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553011503097.png)



#### Advance

1. 天狗食月效果

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <style type="text/css">
         body {
           display: flex;
           height: 100vh;
           background: #aaaaaa;
         }
         .circle {
           margin: auto;
           width: 50px; 
           height: 50px;
           border-radius: 50%;
           color: rgb(245, 178, 178);
           box-shadow: inset 5em 0 0;
           animation: move 10s linear infinite alternate-reverse;
         }
         @keyframes move {
           to {
             box-shadow: inset -5em 0 0;
           }
         }
       </style>
     </head>
   
     <body>
       <div class="circle">
       </div>
     </body>
   </html>
   ```

2. 美化表格

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <style type="text/css">
         table, tr, th, td {
           border: 1px solid #d6d6d6;
         }
         table {
           width: 100%;
           border-collapse: collapse;
           font-size: 14px;
           color: #555;
           table-layout: fixed;
         }
         th, td {
           padding: 6px 12px;
         }
         tr:nth-child(odd) {
           background: aliceblue;
         }
         tr:hover {
           background: lightpink;
         }
         tr {
           transition: background 1s;
         }
       </style>
     </head>
   
     <body>
       <colgroup>
         <col span="3"></col>
         <col style="width: 260px"></col>
       </colgroup>
       <table>
         <tr>
           <th>等级</th>
           <th>掘力值</th>
           <th>身份</th>
           <th>权限</th>
         </tr>
         <tr>
           <td>lv3</td>
           <td>200</td>
           <td></td>
           <td>开发中</td>
         </tr>
         <tr>
           <td>lv4</td>
           <td>500</td>
           <td>优秀作者</td>
           <td>小册写作权限</td>
         </tr>
         <tr>
           <td>lv6</td>
           <td>1000</td>
           <td>合伙人</td>
           <td>提交代码</td>
         </tr>
         <tr>
           <td>lv10</td>
           <td>2000</td>
           <td>ceo</td>
           <td>证书</td>
         </tr>
       </table>
     </body>
   </html>
   ```

3. 

---

#### CSS面试题

1. [50道CSS基础面试题(附答案)](<https://segmentfault.com/a/1190000013325778>)
2. [50道CSS基础面试题中的答案真的就只是答案吗？](<https://segmentfault.com/a/1190000013860482>)
3. [前端日刊-CSS面试题总结](<https://funteas.com/topic/5ada8eac230d1e5e25e45b89>)
4. [front-end-interview-handbook](<https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Chinese/questions/css-questions.md>)