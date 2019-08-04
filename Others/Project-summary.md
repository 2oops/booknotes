# Project-summary

#### 1. vue-three-echarts

首先这是一个`vue-threejs+Echarts+elemenUI+mockjs`的`vue-cli3`项目，将从以下几点对该项目做一个总结。

1. 项目的整体布局以及在开发时遇到的一些小问题

2. 如何使用`dat.gui`实现一个3D模型的定位与位置调整

3. 如何在canvas下渲染`threej`的3D模型及`sprite`精灵

4. vue项目结合`Echarts`在canvas下加载图表

5. 结合mockjs的实现数据的实时更新

6. Bus总线的使用

   最后谈一下不足之处：项目建目录时一定要有一定的规则约束，如js文件尽量写在一个文件夹下，公共组件写在一个文件夹下

   ------

7. 项目整体自适应布局的实现

   1. [实现三栏布局的六种方式](https://www.jianshu.com/p/3046eb050664)

      ```html
      <body>
          <div class="container">
              <!-- 优先渲染 -->
              <div class="center">
                  center
              </div>
              <div class="left">
                  left
              </div>
              <div class="right">
                  right
              </div>
          </div>
      </body>
      ```

      给 container 设置为一个 flex 容器`display: flex` 

      center 的宽度设置为`width: 100%`，left 和 right 设置为`width: 200px` 

      为了不让 left 和 right 收缩，给它们设置`flex-shrink: 0` 

      使用`order`属性给三个部分的 DOM 结构进行排序：left：`order: 1`，center：`order: 2`，right：`order: 3`

      > 这里如果需要实现边栏固定的话，可以使用`calc()`（注意计算的中间减号两边一定是空格）计算属性并配合`100vh,100vw`

   2. [div块元素垂直水平居中](https://www.cnblogs.com/Youngly/p/6796922.html)，有四种常用方法

      比较简单实用的一种就是父元素`display: flex`,子元素`margin: auto`

   3. 图标实现两列布局，图标如果是由多个`li`标签或`div`实现的，可以使用`li:nth-child(2n-1)`实现一个两列的布局，然后宽度可以使用padding去撑开

8. `dat.gui`

9. sprite

10. echarts

11. [mockjs](https://blog.csdn.net/museions/article/details/79289320)

12. Bus总线的使用

    一般对于不是很大的项目来说，可以使用bus实现非父子组件的通信，大项目则可以使用vuex作为状态管理方案。

    这里参考[博客](https://www.cnblogs.com/lalalagq/p/9960336.html)

    定义bus到全局

    ```js
    var eventBus = { //app.js或main.js中
        install(Vue, options) {
            Vue.prototype.$bus = vue
        }
    };
    Vue.use(eventBus);
    ```

    然后在组件中，可以使用$emit， $on， $off 分别来分发、监听、取消监听事件

    ```js
    methods: {
      todo: function () {
        this.$bus.$emit('todoSth', params);  //params是传递的参数
        //...
      }
    }
    ```

    ```js
    // ...
    created() {
      this.$bus.$on('todoSth', (params) =&gt; {  //获取传递的参数并进行操作
          //todo something
      })
    },
    // 最好在组件销毁前
    // 清除事件监听
    beforeDestroy () {
      this.$bus.$off('todoSth');
    },
    ```

    ```js
    // ...
    created() {
      this.$bus.$on('firstTodo', this.firstTodo);
      this.$bus.$on('secondTodo', this.secondTodo);
    },
    // 清除事件监听
    beforeDestroy () {
      this.$bus.$off('firstTodo', this.firstTodo);
      this.$bus.$off('secondTodo', this.secondTodo);
    },
    ```

    或者定义一个`bus.js`

    ```js
    import Vue from 'vue'
    const Bus = new Vue()
    export default Bus
    //然后在需要用到的地方import bus from '@bus'就行了
    ```

2. 对象转数组

   ```javascript
   //假设有这么个对象res
   let res = {name:'gouzi',age:20,sex:'male',area:'Beijing'}
   // 现在我们要给它转成以下这种格式
   //[{props:'name',label:'名称',value:'gouzi'},
    	//{props:'age',label:'年龄',value:20},
   	//{props:'sex',label:'性别',value:'male'},
    	//{props:'area',label:'地区',value:'Beijing'}]
   // 就是说不仅对象要转成对象数组，还要添加一个给对象数组添加一个属性，下面给出转化方法
   function dataHandle(obj) {
       let arr1 = [];
       let result = [];
       let labelArr = ['名称','年龄','性别','地区']
       for(let key in obj) {
           let objItem = {};
           objItem.props = key;
           objItem.value = obj[key];
           arr1.push(objItem);
       }
       //这里返回的arr1如下
       //[{props:'name',value:'gouzi'},
    	//{props:'age',value:20},
   	 //{props:'sex',value:'male'},
    	//{props:'area',value:'Beijing'}]
       
       //然后我们给arr1添加一个对象属性
       arr1.map((item, index) => {
           result.push(
           Object.assign({},item,{label:labelArr[index]})
               //注意这里有一个item，不加item则label不会到数组对象元素中
           )
       }) 
       return result;
   }
   ```

   