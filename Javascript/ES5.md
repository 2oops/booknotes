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

   6. 

4. 