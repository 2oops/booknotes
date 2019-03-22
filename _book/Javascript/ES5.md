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

   6.

