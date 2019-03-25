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
   2. 双飞翼布局
3. 等高布局
4. 粘连布局