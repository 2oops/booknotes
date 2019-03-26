# layout

1. 两列自适应布局

    1. float + overflow: hidden

       ```html
       <div class="parent" style="background-color: #aaaaaa;">
          <div class="left"  style="background-color: #cccccc;">
            <p>left</p>
          </div>  
          <div class="right" style="background-color: #bbbbbb;">
            <p>right</p>
            <p>right</p>
          </div>
         </div>
       ```

       ```css
          .parent {
            /* 注意这里最好给一个整体overflow,而不是直接body给overflow属性 */
            overflow: hidden;
            /* 由于设置overflow:hidden并不会触发IE6-浏览器的haslayout属性，所以需要设置zoom:1来兼容IE6-浏览器 */
            zoom: 1;
          }
          .left {
            float: left;
            margin-right: 30px; 
          }
          .right {
            overflow: hidden;
            zoom: 1;
          }
       ```

    2. Flex

       ```css
       //html同上
       .parent {
         display:flex;
       }  
       .right {
         margin-left:20px; 
         flex:1;
       }
       ```

    3. Grid

       ```css
       .parent {
          display: grid;
          grid-template-columns:  auto 1fr;//fr相等时实现等宽两列布局
          grid-gap: 1px
       } 
       ```

>  当侧边栏在右边时，需注意渲染顺序，即HTML中，需先写侧边栏再写主内容

2. 三栏布局
   1. 圣杯布局

      ```css
      .container {
          padding-left: 220px;
          padding-right: 220px;
        }
        .left {
          /* 三部分都设为左浮动 */
          float: left;
          width: 200px;
          height: 100vh;
          background: #aaaaaa;
          /* 通过设置margin-left为负值让left和right部分回到与center部分同一行
          左边的-100% 对应左边宽度，右边只能写右边宽度的负值 */
          margin-left: -100%;
          /* 两侧position均为relative */
          position: relative;
          /* 两侧对应给左右属性负值padding-left+left = padding-right+right = 0 */
          left: -220px;
        }
        .center {
          float: left;
          /* 中间宽度100% */
          width: 100%;
          height: 100vh;
          background: #bbbbbb;
        }
        .right {
          float: left;
          width: 200px;
          height: 100vh;
          background: #cccccc;
          margin-left: -200px;
          position: relative;
          right: -220px;
        }
      ```

      ```html
        <article class="container">
          <div class="center">
            <h2>圣杯布局</h2>//先写中间列，以实现优先加载
          </div>
          <div class="left"></div>
          <div class="right"></div>
        </article>
      ```

   2. 双飞翼布局

      ```css
      //解决圣杯布局错乱的问题，实现内容和布局的分离，而且任一栏都可以是最高栏
          .container {
              min-width: 600px;//确保中间内容可以显示出来,必须要大于left宽+right宽
          }
          .left {
              float: left;
              width: 200px;
              height: 400px;
              background: #aaaaaa;//margin同样实现三者同行的作用
              margin-left: -100%;
          }
          .center {
              float: left;
              width: 100%;
              height: 500px;
              background: #bbbbbb;
          }
          .center .inner {
              margin: 0 200px; //新增部分，左右两翼各200px
          }
          .right {
              float: left;
              width: 200px;
              height: 400px;
              background: #cccccc;
              margin-left: -200px;
          }
      
      ```

      ```html
      <article class="container">
          <div class="center">
              <div class="inner">双飞翼布局</div>
          </div>
          <div class="left"></div>
          <div class="right"></div>
      </article>
      ```

      **圣杯布局是利用父容器的左、右内边距+两个从列相对定位； 双飞翼布局是把主列嵌套在一个新的父级块中利用主列的左、右外边距进行布局调整**

3. 等高布局

   1. 正padding + 负margin

4. 粘连布局

   1. footer设计

5. 其他布局

   1. 参考[实现三栏布局的6种方式](https://www.jianshu.com/p/3046eb050664)
   2. [div块元素垂直水平居中](https://www.cnblogs.com/Youngly/p/6796922.html)
