# Js

1. call,apply和bind的区别

   1. 三者均为`Function.prototype`下的方法，很好地体现了js函数式语言特性，都是用于改变函数运行时的上下文，最终返回的是调用的方法的返回值，若该方法没有返回值，则返回`undefined`

      ```js
      var max3 = Math.max.call(null,1,2,3);//3 
      //在非严格模式下，第一个参数为null或者undefined时会自动替换为指向全局对象
      var max4 = Math.max.apply(null,[1,2,3,4,5])//5 apply第二个参数为数组或类数组
      var max5 = Math.max.bind(null,[1,2,3])()//NaN
      var max6 = Math.max.bind(null,1,2,3,4,56)()//56
      //可以发现，call和apply的区别仅仅是传参的不同，而bind的区别是不会自动立即执行，需调用时才会执行
      ```

   2. `call()`是`apply()`的语法糖，三者均可实现继承；

   3. 在选用时：如果不需要考虑具体多少参数被传入函数时选`apply()`；如果确定函数可以接收参数个数， 并且想表达形参和实参的对应关系，用`call()`；如果不需要立即调用执行，按需调用使用`bind`

   4. 看以下实例：

      ```js
      var name = 'bigBoss', age = 25;
      let obj = {
        name: 'xiaosan',
        objAge: this.age,
        myFun: function() {
          console.log(this.name + ', age: ' + this.age);
        }
      }
      obj.objAge;//25
      obj.myFun();//xiaosan, age: undefined 因为这里this指向obj
      
      let dm = {
        name: 'dema',
        age: 100
      }
      
      obj.myFun.call(dm);
      obj.myFun.apply(dm);
      obj.myFun.bind(dm)();
      //均为 dema, age: 100
      ```

      ```js
      var name = 'bigBoss', age = 25;
      let obj = {
        name: 'xiaosan',
        objAge: this.age,
        myFun: function(from, to) {
          console.log(this.name + ', age: ' + this.age + ' from:' + from + ' to:' + to);
        }
      }
      obj.objAge;//25
      obj.myFun();//xiaosan, age: undefined 因为这里this指向obj
      
      let dm = {
        name: 'dema',
        age: 100
      }
      obj.myFun.call(dm,'成都','上海');//dema, age: 100 from:成都 to:上海
      obj.myFun.apply(dm,['成都','上海']);//dema, age: 100 from:成都 to:上海
      obj.myFun.bind(dm,'成都','上海')();//dema, age: 100 from:成都 to:上海
      obj.myFun.bind(dm,['成都','上海'])();//dema, age: 100 from:成都,上海 to:undefined
      ```

2. `localStorage`和`sessionStorage`的区别

   1. 两者的区别主要在于作用域和有效期，两者均不与服务器通信
   2. `localStorage`在相同文档源(协议，域名，端口号相同)下，相同浏览器的不同页面间可以共享数据，可以互相读取，甚至覆盖；但不同标签页或不同页面间无法共享`sessionStorage`数据，这里的标签页和页面指的是`顶级窗口`，如果一个标签页下包含多个`iframe`且属于同源页面，则`sessionStorage`数据是可以共享的
   3. 有效期：`localStorage`的有效期是永久的，除非通过浏览器`UI`去人为删除，而`sesionStorage`则是在关闭浏览器或标签页后便被删除，与目前情况不同的是，部分现代浏览器可以`Ctrl+shift+T`实现历史记录的重开，这时`sessionStorage`的有效期可能延长。

3. JS原型链

4. 深拷贝和浅拷贝

5. 

