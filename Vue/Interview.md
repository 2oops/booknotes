# Interview

1. `v-if`和`v-show`的区别
  `v-show`只是`CSS`层面的`display: none`和`display: block`之间的切换，而`v-if`决定的是代码块的内容（或组件）是否渲染；
  频繁操作时可以适合用`v-show`，一次渲染完的适用`v-if`，写复用组件时用`v-if`也比较多；
  `v-if`在性能优化上的经验：对于某些不重要的内容，可以先将其置为`false`，这样可以优先渲染其他重要的内容，如在`nextTick`之后将其置为`true`。

2. `computed`和`watch`的区别
  若需要自动监听依赖值的变化，需要动态值，则适合用`computed`，需要监听到值的改变然后执行业务逻辑，才用`watch`；

  延伸问题：

  1. `computed`为一个对象时，有哪些操作？
  2. `computed`和`methods`的一些区别？
  3. `computed`是否能依赖其他组件的数据？
  4. `watch`为一个对象时，有哪些操作？

  解答：

  1. 还有`set`和`get`方法

     ```javascript
     computed: {
         get() {}
         set(val) {}
     }
     ```

  2. `computed`不能传参，`methods`能；`computed`可以缓存，`methods`不会；
  3. `computed`可以依赖其他的计算属性，也可以依赖其他组件的`data`；
  4. `handler  / deep / immediate`