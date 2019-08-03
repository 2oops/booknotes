# ES7

1. ### Array.prototype.includes()

   `includes()`属性本是ES6打算出的标准，但是当时没来得及，因此它最终出现在ES7中

   ```javascript
   // includes()接收两个参数，要搜索的值及开始搜索的索引位置
   let values = [1,2,3,NaN]
   console.log(values.includes(1)) // true
   console.log(values.includes(1,2)) // false 从索引为2的地方搜索1
   // includes()是可以认为NaN === NaN
   console.log(values.includes(NaN))// true
   console.log(values.indexOf(NaN)) // -1
   // indexOf()和includes()均认同+0 === -0，而Object.is()方法会将+0 和 -0视为不同的值
   ```

   

2. **指数操作符

   ```javascript
   let a = 5 ** 2
   let b = Math.pow(5,2)
   a === b // true
   let c = 2 * 5 ** 2 // 50
   let d = - 5 ** 2 //语法错误
   let e = - (4 ** 2) // -16
   
   let num1 = 2
   let num2 = 3
   console.log(++num1 ** 2) // 9
   console.log(num1) // 3
   console.log(num2-- ** 2) // 9
   console.log(num2) // 2
   ```

   注意：一元运算符的优先级高于**，并且有运算限制。在求幂运算符左侧无须使用括号就可以使用`++`和`--`，因为这两个运算符都明确定义了作用域操作数的行为。

3. 函数作用域严格模式下的改动

   ```javascript
   // use strict的使用
   function Do(first = this) {
       "use strict"
       return first
   }
   // 这里参数默认值是可以为函数的，故此很多JS引擎均不实现此功能，所以在ES7中规定在参数被解构或有默认参数
   // 的函数中禁止使用"use strict"
   // 简单参数列表，可以运行
   function okay(first, second) {
       "use strict"
       return first
   }
   // 以下均语法错误
   function wrong(first, second = first) {
       "use strict"
       return first
   }
   function wrong2({first, second}) {
       "use strict"
       return first
   }
   ```
