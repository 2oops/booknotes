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

   7. 