# Grid

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
