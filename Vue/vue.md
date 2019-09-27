# Vue

1. 服务端渲染SSR

   与传统SPA相比，SSR的优势在于：

   1. 更好的SEO，若SEO对你的站点至关重要，而你的页面又是异步获取内容（带类似loading菊花图异步加载），则可以考虑使用SSR渲染，因为Google和Bing相对来说对同步JavaScript应用程序有更好的索引性能。
   2. 更快的内容到达时间，可以产生更好的用户体验。
   3. 是否选用SSR需权衡的一些点：开发条件；涉及构建及部署的更多要求，这里服务器渲染应用程序，是需要处于NodeJS运行环境；可能需要更多的服务端负载，并采用缓存策略，因为服务器CPU资源将大量被占用。

2. Vue的双向绑定机制

   Vue 的双向绑定机制采用**数据劫持结合发布/订阅模式**实现的: 通过 `Object.defineProperty()`来劫持各个属性的 `setter，getter`，在数据变动时发布消息给订阅者，触发相应的监听回调。

   **观察者模式和发布/订阅模式**不能混淆一谈，其实订阅模式有一个调度中心，对订阅事件进行统一管理。而观察者模式可以随意注册事件，调用事件。

***

#### Vue的内部运行机制

1. 响应式系统

   `MVVM`模型如何通过操作对象更新到视图，核心实现在于**响应式系统**，`Vue`基于`Object.defineProperty`实现该系统，然后通过`observer`使对象变成可观察的。在`new Vue()`到`$mount`这个阶段，会对数据进行初始化。

   以下模拟简略的响应式系统

   ```javascript
   // 用来模拟视图更新的函数
   function cb(val) {
     // 这里可以是渲染视图的方法
     console.log("view updated")
   }
   
   // 实现响应式
   function defineReactive(obj, key, val) {
     Object.defineProperty(obj, key, {
       configurable: true,
       enumerable: true,
       get: function reactiveGetter() {
         return val;
       },
       set: function reactiveSetter(newVal) {
         if( newVal === val) return;
         cb(newVal)
       }
     })
   }
   
   // observer传入value，即需要做响应式的对象
   // 通过遍历所有属性的方式对该对象的每个属性都通过defineReactive处理
   // 实际上observer会进行递归调用，此处去掉了该过程
   function observer(value) {
     if(!value || (typeof value !== 'object')) {
       return
     }
   
     Object.keys(value).forEach((key) => {
       defineReactive(value, key, value[key])
     })
   }
   
   class Vue { // Vue构造类
     //options的data在Vue的构造函数中做处理，
     //data本为函数，这里暂当作对象处理
     constructor(options) {
       this._data = options.data;
       observer(this._data);
     }
   }
   
   let myVue = new Vue({
     data: {
       test: 'this is test'
     }
   })
   // 视图更新
   console.log(myVue._data.test) // 'this is test'
   myVue._data.test = "update" // 'view updated' => 'update'
   ```

2. 