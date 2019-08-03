# ES6-Advance

1. **改变对象的原型**，ES5的`getPrototypeOf()`方法和ES6添加的`setPrototypeOf()`方法

   ```java
   // 定义两个基对象
   let person = {
     getGreeting() {
       return "hello"
     }
   }
   
   let dog = {
     getGreeting() {
       return "wowo"
     }
   }
   // 以person对象为原型
   let friend = Object.create(person)
   console.log(friend.getGreeting())// hello
   console.log(Object.getPrototypeOf(friend) === person)//true
   // 将原型设置为dog
   Object.setPrototypeOf(friend, dog)
   console.log(friend.getGreeting())//wowo
   console.log(Object.getPrototypeOf(friend) === dog)// true
   ```

2. 使用**super**引用简化原型访问

   ```javascript
   // 先来看一段代码，在ES5中，如果你想重写对象实例的方法，又需要调用与它同名的原型方法
   let person = {
       getGreeting() {
           return "hello"
       }
   }
   let dog = {
       getGreeting() {
           return "woow"
       }
   }
   let friend = {
       getGreeting() {// 这里注意必须要在使用简写方法的对象中使用super引用
           return Object.getPrototypeOf(this).getGreeting.call(this) + ', hi'
           //使用super如下
           // return super.getGreeting() + ', hi'
       }
   }
   // 将原型设置为person
   Object.setPrototypeOf(friend, person)
   console.log(friend.getGreeting())// hello, hi
   console.log(Object.getPrototypeOf(friend) === person)// true
   // 其实以上friend对象中Object.getPrototypeOf(this)就相当于super
   
   // 在多重继承中，super引用非常有用，并且，使用Object.setPrototypeOf()会出现问题，
   //因为setPrototypeOf()中的this问题最终会导致栈溢出报错
   // 如下使用super
   let person = {
       getGreeting() {
           return 'hello'
       }
   }
   let friend = {
       getGreeting() {
           return super.getGreeting() + ',hi'
       }
   }
   Object.setPrototypeOf(friend, person)
   
   let relative = Object.create(friend)
   console.log(person.getGreeting())// 'hello'
   console.log(friend.getGreeting())// 'hello,hi'
   console.log(relative.getGreeting())// 'hello,hi'
   ```

3. ES6正式定义了方法

   ```javascript
   let person = {
       // getGreeting()是方法
       getGreeting() {
           return 'hello'
       }
   }
   // 不是方法
   function share() {
       return 'hi'
   }
   // 这个示例中定义了一个person对象，该对象有一个getGreeting()方法，由于直接把函数赋值给了person对象
   // 因此getGreeting()方法的[[HomeObject]]属性值为person，而share函数并未明确定义该属性，虽然这是
   // 个小差别，但是在super引用中这很重要，这就是为什么在2中要求super引用要在对象简写方法中使用。
   ```

4. ES6扩展对象功能性中还扩展了`assign()`以及`Object.is()`(该方法对所有值进行严格等价判断，比三等号操作符更安全)

   ```javascript
   Object.is(5, '5')// false
   Object.is(NaN, NaN)// true
   NaN === NaN // false
   +0 === -0 // true
   Object.is(+0, -0) // false
   
   // assign()
   let receiver = {}
   Object.assign(receiver,
       {
       name: 'xiaoa',
       type: 'js'
   },{
       type: 'string'// 排位靠后的源对象会覆盖靠前的
   })
   console.log(receiver.type) // "string"
   ```

5. ###### ES6中的类(一等公民)

   1. 其实在ES5中已经存在近类结构，思路是这样的：简单来说就是创建一个自定义类型，首先创建一个构造函数，然后定义另一个方法并赋值给构造函数的原型。

      ```javascript
      function PersonType(name) {
      	this.name = name // 创造构造函数
      }
      PersonType.prototype.sayName = function() { // 给函数原型定义一个方法
          console.log(this.name)
      }
      let person = new PersonType("Nical")
      person.sayName() // "Nical"
      person instanceof PersonType // true
      person instanceof Object // true
      ```

      `PersonType`的原型在添加了sayName()方法后，`PersonType`对象的所有实例都将共享这个方法，然后使用new操作符创建了`person`实例。这里person对象是PersonType的实例，由于使用了**原型继承**，所以person也是Objec的实例

   2. 而基本类是这样声明的，与1中的原型继承有相似之处

      ```javascript
      class PersonClass { // 注意标识符后是没有括号的
          constructor(name) { // 创建构造函数
              this.name = name 
          }
          sayName() { // 定义一个方法
              console.log(this.name)
          }
      }
      let person = new PersonClass("Nical")
      person.sayName() // "Nical"
      person instanceof PersonClass // true
      person instanceof Object // true
      typeof PersonClass // "function"
      typeof PersonClass.prototype.sayName // "function"
      ```

      `PersonClass`中除`constructor`外没有其他保留的方法名，因此可以方便的添加方法。与近类结构不同的是，类属性不可被赋予新值，`PersonClass.prototype`是一个只可读的类属性。

      使用类语法时应注意：

      1）类声明中的所有代码将自动运行在严格模式下，且无法脱离严格模式；

      2）在类中，所有方法都是不可枚举的；

      3）每个类都有[[construct]]内部方法，new关键字只调用该构造函数且只能用new调用它；

      4）类声明不能像函数声明那样被提升，在真正执行声明语句前，它们会一直存在于临时死区中；

      5）类中不能修改类名，因为类名在类中为常量，而在外部是可以修改的。

   3. 类表达式

      ```javascript
      let PersonClass = class {
          constructor(name) {
              this.name = name // 建议在构造函数中声明所有自有属性
          }
          sayName() {
              console.log(this.name) // this.name即为自有属性
          }
      }
      ```

      类声明和类表达式均不会像函数声明和函数表达式那样被提升。类表达式不需要标识符在类后，除了语法上有差异外，类表达式在功能上等价于类声明。

   4. 命名类表达式

      ```javascript
      let PersonClass = class PersonClass2 {
          constructor(name) {
              this.name = name
          }
          sayName() {
              console.log(this.name)
          }
      }
      typeof PersonClass // "function"
      typeof PersonClass2 // "undefined"
      ```

      这里`PersonClass2`只存在于类定义中，而在类的外部并不存在一个名为`PersonClass2`的绑定，所以，为`undefined`，进一步了解原理，如下：

      ```javascript
      // 不用class关键字等价声明PersonClass
      let PersonClass = (function() {
          "use strict" // 前面有说到必须严格模式下使用类
          const PersonClass2 = function(name) {
              // 确保通过new关键字调用该函数
              if(typeof new.target === "undefined") {
                  throw new Error("必须通过new调用该函数")
          }
              this.name = name
      }
          Object.defineProPerty( PersonClass2.prototye, "sayName", {
              value: function() {
                  if(typeof new.target !== "undefined") {
                      throw new Error("不能使用new调用该方法")
                  }
                  console.log(this.name)
              },
              enumerable: false,
              writable: true,
              configurable: true
          });
          return PersonClass2
      }())
      
      ```

      命名类表达式通过const定义名称，从而`PersonClass2`只能在类的内部使用

   5. 将类作为参数传入函数中

      ```javascript
      function createObject(classDef) {
          return new ClassDef()
      }
      let obj = createObject( class { // 传入匿名类表达式
          sayHi() {
              console.log("HI")
          }
      })
      obj.sayHi() // "HI"
      
      ```

   6. 通过立即调用类构造函数创建单例

      ```javascript
      let person = new Class { // 使用new调用类表达式
          constructor(name) {
              this.name = name
          }
          sayName() {
              console.log(this.name)
          }
      }("Nical") // 直接使用小括号调用这个表达式
      person.sayName() // "Nical"
      
      ```

   7. 类的访问器属性

      类支持直接在原型上定义访问器属性，创建`getter`时，需要在关键字get后紧跟一个空格和相应的标识符即可，`setter`同理

      ```javascript
      class CustomHtmlElement{
          constructor(el) {
              this.el = el
          }
          get html() {
              return this.el.innerHTML
          }
          set html(val) {
              this.el.innerHTML = val
          }
      }
      let descriptor = Object.getOwnPropertyDescriptor(CustomHtmlElement.prototype, "html")
      "get" in descriptor // true
      "set" in descriptro // true
      descriptor.enumerable // false
      
      ```

      该类是对el元素的包装器，并通过getter和setter方法将这个元素的`innerHTML`方法委托给html属性，而这个访问器属性是在原型上创建的。

   8. 可计算成员名称

      ```javascript
      let methodsName = "sayName"
      class PersonClass {
          constructor() {} // 构造器自定义属性
          [methodsName]() {
              // 方法
          }
      }
      let me = new PersonClass("Nical")
      me.sayName() // "Nical"
      
      ```

      同理可在访问器属性中应用可计算名称 ` get [propertyName]() {}`

   9. 生成器方法

      在对象字面量中，可通过在方法名前附加一个星号的方式来定义生成器，类中同理，可以将任何方法定义成生成器

      ```javascript
      class MyClass {
          *createIterator() {
              yield 1;
              yield 2;
              yield 3;
          }
      }
      let instance = new MyClass()
      let iterator = instance.createIterator()
      
      ```

      通过`Symbol.iterator`定义生成器方法即可为类定义默认迭代器

      ```javascript
      class Collection {
          constructor() {
              this.items = []
          }
          *[Symbol.iterator]() {
              yield *this.items.values()
          }
      }
      let collection = new Collection()
      collection.items.push(1)
      collection.items.push(2)
      collection.items.push(3)
      for(let x of collection) {
          console.log(x) // output: 1 2 3
      }
      
      ```

      `collection`即为`Collection`的实例，可以将其直接用于`for-of`或展开运算符中

   10. 静态成员

       ```javascript
       class PersonClass {
           constructor(name) {
               this.name = name
           }
           sayName() {
               console.log(this.name)
           }
           static create(name) {
               return new PersonClass(name)
           }
       }
       let person = PersonClass.create("Nical")
       
       ```

       定义的静态方法和`sayName()`方法的区别在于是否使用了static关键字，类中的所有方法和访问器属性都可以用`static`关键字定义，出来构造函数`constructor`

6. Promise

   1. 自Promise继承

      ```javascript
      class MyPromise extends Promise {
          success(resolve, reject) {
              return this.then(resolve, reject)
          }
          failure(reject) {
              return this.catch(reject)
          }
      }
      let promise = new MyPromise((resolve, reject) => {
          reject(22)
      })
      promise.success(value => {
          console.log(value + 1)
      }).failure(value => {
          console.log(value) // 22
      })
      
      ```

      `success`和`failure`均为静态方法，派生Promise与内建Promise功能一样，多出来的静态方法也拥有resolve(),reject(),race()和all()这四个方法。

      ```javascript
      let p1 = new Promise((resolve, reject) => {
          resolve(23)
      })
      let p2 = MyPromise.resolve(p1)
      p2.success(value => {
          console.log(value) // 23
      })
      console.log(p2 instanceof MyPromise) // true
      
      ```

      p1是一个内建promise，被传入MyPromise.resolve()方法后得到p2，它是MyPromise的一个实例，来自p1的resolve值被传入p2完成处理程序。

   2. 基于Promise的异步任务执行（生成器）

7. 代理和反射API

   1. `Proxy`是一种可以拦截并改变底层JavaScript引擎操作的包装器。代理可以拦截JavaScript引擎内部目标的底层对象操作，这些底层操作被拦截后会触发印象特定操作的陷阱函数。
   2. 反射API以`Reflect`对象形式出现，对象中方法的默认特性与相同的底层操作一致，而代理可以覆写这些操作，每个代理陷阱对应一个命名和参数都相同的`Reflect`方法。
   3. 



