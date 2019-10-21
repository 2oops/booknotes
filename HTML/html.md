# HTML

1. #### 图标的用法

   1. `icomoon`: icon的`unicode`写法和`font+CSS`写法

      ```html
      <div>
        <span class="icon-">&#xe949</span>
        <span class="icon- font">&#x1f330</span>
      
        <span class="icon-location"></span>
      </div>
      ```

   2. 阿里`iconfont:unicode font+CSS Symbol(彩色)-需引入iconfont.js`

      ```html
      <div>
          <span class="iconfont">unicode</span>
          <span class="iconfont icon-xxx"></span>
      
          <svg class='icon' aria-hidden='true'>
            <use xlink:href='#icon-xxx'></use>
          </svg>
        </div>
      ```

   3. 引用图标在线链接--1)更换`style.css`中的@font-face(注意添加http:),然后引用

      2)`link`生成的`css`文件链接

      3）`symbol`引入`Js`文件