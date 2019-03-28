# ES5

1. 弱类型  6种类型包括一种对象类型`object`和5种基本类型`number,boolean,null,undefined,string`

2. `NaN`不等于`NaN`，`null === null,undefined === undefined`

3. 类型检测：

   1. `typeof null -> "object", typeof NaN -> "number", typeof(undefined) -> "undefined", typeof [1,2] -> "object"`

   2. `instanceof`

   3. `obj instanceof Object` 

      `[1,2] instanceof Array === true`
      `Object.prototype.toString.apply([]); === "[Object Array]"`
      `Object.prototype.toString.apply(null/undefined); === "[object Null/undefined]"`
      `Object.protoype.toString.apply(function(){}); === "[object Function]"`

   4. `constructor`

   5. ```javascript
      // 判断两个数组是否类型相似，是则为true,否则为false
      function type(a){
          //解决typeof null为object情形
      	return  a === null ? '[object Null]':Object.prototype.toString.apply(a);
          }
      function arraysSimilar(arr1, arr2){
          if(!Array.isArray(arr1) || !Array.isArray(arr2) ||arr1.length!=arr2.length){return false;}
          var arr3=[];
          var arr4=[];
          var x;
          for(var i in arr1){
              arr3.push(type(arr1[i]));
              arr4.push(type(arr2[i]));
          }
          if(arr3.sort().toString()==arr4.sort().toString()){
              return true;
          }else{
              return false;
          }
      ```

   6. ```javascript
      function Foo() {}
      Foo.prototype.x = 1;
      var obj = new Foo();
      obj.x;//1
      obj.hasOwnProperty('x');//false
      obj._proto_.hasOwnProperty('x'); //true hasOwnProperty表示的是当前链节点是否含有该属性
      ```

   7. JS没有块级作用域，`for in`顺序不确定，`enumerable`为`false`时不会出现，`for in`对象属性时受原型链影响，可能会遍历到原型链上的属性

   8. 严格模式：不允许使用`with`，不允许未申明的变量赋值，arguments变为参数的静态副本，`delete`参数/函数名报错，禁止八进制字面量

   9. ![1553178431454](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553178431454.png)

4. 原型链

   1. 使用点赋值时也是相对于原型链上的该链节点的操作，并不会顺着原型链向上修改到某个属性的值，同理，删除该属性时也不会顺着原型链向上删除

   2. ![1553179125753](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553179125753.png)

   3. ```javascript
      //跨过该链节点创建属性
      var obj = Object.create({x: 1})
      obj.x;//1
      typeof obj.toString; //"funciton"
      obj.hasOwnproperty('x');//false
      
      var obj = Object.create(null);
      obj.toString;//undefined
      ```

   4. ```js
      //属性枚举
      var o = {x: 1, y: 2, z: 3}
      'toString' in o;// true 因为原型链上有toString
      o.propertyIsEEnumerable('toString');//false
      var key;
      for(key in o) {
          console.log(key);//x,y,z
      }
      
      var obj = Object.create(o);
      obj.a = 4;
      var key;
      for (key in obj) {
          console.log(key);//a,x,y,z
      }
      
      var obj = Object.create(o);
      obj.a = 4;
      var key;
      for(key in obj) {
          if(obj.hasOwnProperty(key)){
              console.log(key);//a
          }
      }
      ```

   5. get/set
      ![1553181215219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553181215219.png)

   6. 属性标签可使用`defineProperty`重复修改
      ![1553181885406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553181885406.png)

   7. ```js
      var obj2 = {x:1,y:true, z:[1,2,3],nullVal: null}
      JSON.stringify(obj2)//"{"x":1,"y":true,"z":[1,2,3],"nullVal":null}"
      var obj3 = {val:undefined,a:NaN,b:Infinity,c:new Date()};
      JSON.stringify(obj3);//"{"a":null,"b":null,"c":"2019-03-22T06:44:45.102Z"}"
      var obj4 = JSON.parse('{"x":1}')//obj4.x->1
      ```

5. `join`和`reverse`，`reverse`,`concat`,`slice`,`splice`

   1. ```js
      function repe(str,n){
          return new Array(n+1).join(str)//注意这里是n+1
      }
      repe('a',3)//'aaa'
      ```

   2. reverse会印象到原数组

   3. `sort`不按数字大小排序，原数组被修改，按字母大小排序，需从小到大排列时：`arr.sort(function(a,b){return a - b;})`

   4. `concat`和`slice`不改变原数组，`splice`改变原数组

   5. `forEach`,`map`,`every`,`some`,`reduce`,`reduceRight`

      ```js
      var arr = [1,2,3];
      arr.map((x) => {
          return x+10;//[11,12,13]
      });
      arr;//[1,2,3]
      var sum = arr.reduce((x,y) => {
          return x + y;//最终返回6
      }, 0);//不改变原数组
      ```

   6. `indexOf`和`lastIndexOf`,返回的均为从左到右的顺序对应的下标

      ```js
      indexOf(2);//查找是否存在2
      indexOf(1,2);//从第下标为2的元素开始寻找1
      indexOf(1,-2);//从倒数第二个元素开始寻找
      lastIndexOf(2,-3)
      
      var arr1= [1,2,3,4,2,1]
      arr1.indexOf(2);//1
      arr1.indexOf(2,3);//4
      arr1.indexOf(3,-1);//-1
      arr1.indexOf(2,-3);//4
      arr1.lastIndexOf(3);//2  lastIndexOf()倒数时依然是从后面数起
      arr1.lastIndexOf(2,-3);//1
      arr1.lastIndexOf(4,-3);//3
      arr1.lastIndexOf(4,-4);//-1
      ```

   7. 判断是否为数组

      ```js
      var arr1 = [1,2,3];
      Array.isArray([]);//true
      arr1 instanceof Array;// true
      ({}).toString.apply([]) === '[object Array]';//true
      arr1.constructor === Array;//true
      ```

   8. 字符串（类数组）

      ```js
      var str = 'hello world'
      str.charAt(0);//"h"
      str[1];//"e"
      Array.prototype.join.call(str,'_');//"h_e_l_l_o_ _w_o_r_l_d"
      ```

6. 函数

   1. Function构造器(很少使用)

      ```
      var func = new Function('a','b','console.log(a+b)');
      func(1,2);//3
      
      var func2 = Function('a','b','console.log(a+b)');
      func2(1,3);//4
      ```

   2. this

      ```js
      function func() {
          'use strict';
          return this;//严格模式下this === undefined,非严格模式下返回window
      }
      ```

   3. bind

      ```js
      this.x = 9;
      let module = {
          x: 81,
          getX: function() {
              return this.x;
          }
      }
      module.getX();//81
      let getX = module.getX;
      getX();//9
      let bindGetX = getX.bind(module);
      bindGetX();//81
      ```

   4. 闭包

      闭包是指一个函数或函数的引用，与一个应用函数绑定在一起，这个应用环境是一个存储该函数每个非局部变量（自由变量）的表。

      闭包不同于一般函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量。

      ```js
      !function() {//叹号表示函数表达式而不是函数声明
      var localData = "loacdhere";
      document.addEventListener('click',
          function() {
              console.log(localData);//每次点击屏幕都会打印localData
          })
      }()
      ```

   5. 执行上下文

      ```js
      if(true) {
          var a = 1;//a,b均用let声明则报错
      }else{
          var b = true;
      }
      alert(a);//1
      alert(b);//undefined 这里else没有执行但是不会报错，因为js没有块级作用域，声明被提前，报undefined
      ```

7. OOP面向对象编程(Object-oriented Programming) 继承 封装 多态 （抽象）

   1. 基于原型的继承

      ```js
      function Foo() {
          this.y = 1;
      }
      typeof Foo.prototype; //"object"这里的prototype是函数声明时给到的一个内置属性
      Foo.prototype.x = 2;
      var obj3 = new Foo()
      obj3.x;//2
      obj3.y;//1
      
      //这里来看一下Foo.prototype下有什么
      {
          constructor: Foo,//构造器指向本身
          _proto_: Object.prototype,//_proto_并非标准，Chrome带有，Object.prototype下含有toString
          //等方法，当前Chrome浏览器返回有更多属性
          x: 2
      }
      ```

      ```
      Student.prototype = Object.create(Person.prototype);
      Student.prototype.constructor = Student;
      ```

      ![1553764098126](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553764098126.png)

      ![1553764129136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553764129136.png)

   2. ```js
      var obj = {x: 1};
      obj; // Object {x: 1}
      obj.x; // 1
      obj._proto_;// 
      //输出 Chrome提供
      //{
      //    constructor: ƒ Object()
      //    hasOwnProperty: ƒ hasOwnProperty()
      //    isPrototypeOf: ƒ isPrototypeOf()
      //    propertyIsEnumerable: ƒ propertyIsEnumerable()
      //    toLocaleString: ƒ toLocaleString()
      //    ......
      //}
      Object.getPrototypeOf(obj);// Object {} ES5提供getPrototypeOf()
      Object.getPrototypeOf(obj) === obj._proto_;//false
      Object.getPrototypeOf(obj) === Object.prototype;//true
      
      function foo() {}
      foo.prototype;//{constructor: ƒ foo()  __proto__: Object}
      foo.prototype._proto_;// Object {}
      foo.prototype._proto_ === Object.prototype;//Chrome 返回false
      obj.toString();//"[object object]"该方法来源于Object.prototype
      obj.valueOf();// {x: 1}
      
      var obj2 = Object.create(null);并不是所有对象都有Object.prototype
      obj2.toString(); //not a function
      obj2._proto_;//undefined
      
      //并不是所有的函数都是有prototype
      function bar() {}
      bar.prototype; //{constructor: ƒ foo()  __proto__: Object}
      var binded = bar.bind(null);
      typeof binded;//"undefined"
      binded.prototype; //Cannot read property 'prototype' of undefined
      ```

   3. ```
      Object.defineProtperty(Object.prototype, 'x', {writable: true, value: 1})
      ```

      ![1553766867840](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553766867840.png)

   4. ```js
      function foo() {}
      foo.prototype.z = 3;
      var obj = new foo();
      obj.y = 2;
      obj.x = 1;
      obj.x;
      obj.x;//1
      obj.y;//2
      obj.z;//3
      typeof obj.toString;//"function"
      'z' in obj;//true
      obj.hasOwnProperty('x');//true
      foo.prototype.hasOwnProperty('z');//true
      obj.hasOwnProperty('z');//false
      ```

   5. `instanceof`

      1. `[1,3] instanceof Array === true;`  `instanceof`右边一定要是函数
      2. `new Object() instanceof Object === true` 判断左边对象的原型链上有没有右边函数的prototype属性

   6. 实现继承的方式

      1. `Student.prototype = new Person();`  但会出现一些问题
      2. `Student.prototype = Object.create(Person.prototype); Student.prototype.constructor = Person`相对理想的方法
      3. `Student.prototype = Person.prototye` 考虑不够周全，不推荐

   7. 模拟重载，链式调用，模块化

      1. ![1553768552782](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553768552782.png)

      2. `Person.call(this, name); Person.prototype.init.apply(this, arguments);`//arguments为数组，注意call和apply的传参区别

      3. ```js
         //链式调用
         function classManager() {}
         classManager.prototype.addClass = function(str) {
             return this; //通过return this 实现的链式调用
         }
         var manager = new classManager();
         manager.addClass('classA').addClass('classB');
         ```

      4. 探测器

8. 

