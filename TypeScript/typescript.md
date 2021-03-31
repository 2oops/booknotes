# TypeScript

1. 基础类型

   - 数组：`let list: number[] = [1, 2, 3];`
   - 用数组泛型：

   ```typescript
   let list:Array<number> = [1,2,3]
   let arr: number[] = [12,2]
   ```

   - tuple:`let x = [number, string]; x = ['aaa', 10]`,当访问一个已知索引的元素，会得到正确的类型：`x[0].substr(1)`,当访问一个越界的元素，会使用联合类型替代：

   - 枚举类型：

   ```typescript
   enum Color {Red, Green, Blue};
   const c:Color = Color.Blue;
   console.log(c) // 输出0
   ```

   - void: 标识返回值类型，void表示该方法没有返回值

   - never: 从不会出现的值
   - null:
   - undefind:

2. 添加了可选静态类型、类和模块；并且支持所有的浏览器、主机和操作系统；编译时检查，不污染运行时

   ```typescript
   class Student {
     fullName: string;
     constructor(public firstName: string, public middleInitial: string, public lastName: string) {
       this.fullName = firstName + " " + middleInitial + " " + lastName;
     }
   }
   
   interface Person {
     firstName: string;
     lastName: string;
   }
   
   function greeter(person: Person) {
     return "Hello, " + person.firstName + " " + person.lastName;
   }
   
   let user = new Student("Jane", "M.", "User");
   
   document.body.textContent = greeter(user);
   ```

3. 安装及使用

   - 安装`yarn`，访问地址：`yarnpkg.com`
   - `npm i -g typescript`
   - `npm i -g ts-node` // 安装Typescript的运行时
   - 然后就可以在Vscode(Ctrl + J 打开终端)中使用`ts-node test.ts`运行Typescript文件

4. Ts与面向对象

   - 对象: 是类的一个实例，有状态和行为
   - 类：可以看作一个模版，描述了一类对象的状态和行为
   - 方法：方法是类操作的实现步骤

   ```typescript
   class Site {
   	name(): void {
   		console.log('2oops')
     }
   }
   const obj = new Site();
   obj.name();
   ```

   编译后生成如下js代码：

   ```javascript
   const Site = (function() {
   	function Site() {};
     Site.prototype.name = function() {
       console.log('js')
     }
     return Site;
   })()
   const obj = new Site();
   obj.name();
   ```

   普通类写法

   ```javascript
   class Site {
   	name() {
       console.log("normal")
     }
   }
   const obj = new Site();
   obj.name()
   ```

5. 关于基本类型的注意点

   - ----strictNullChecks：该模式下不可以对声明了类型的变量赋空或undefined，但可以作出如下声明

     `let x:number | null | undefined;`

     在js中undefined是一个没有设置值的变量，typeof一个没有值的变量会返回undefined

     null和undefined是其他任何类型的子类型，可以赋值给其他类型

   - never是其它类型(包括null和undefined)的子类型，代表从不会出现的值，只能赋给never类型，可以用在抛出异常或无限循环

     ```typescript
     function error(msg: string): never {
     	throw new Error(msg)
     }
     ```

6. 变量声明

   - 变量是一个使用方便的占位符，用于引用计算机内存地址
   - 命名规则：
     - 可以包含数字和字母，但不能以数字开头
     - 除了下划线和美元符号，不能使用其他字符，包括空格

7. 函数

   - 构造函数

     ```typescript
     const f = new Function('a', 'b', 'c', 'return a*b*c')
     const res = f(2,3,4)
     console.log(res) // 24
     ```

   - 递归函数

     ```typescript
     function factorial(number) {
       if(number <= 0) {
         return 1
       } else {
         return number * factorial(number - 1)
       }
     }
     
     console.log(factorial(3))
     ```

   - 可选参数 | 默认参数 | 剩余参数
   
     如果定义了参数，则必须要传，不过可定义可选参数，但可选参数须要放在默认参数后
   
     默认参数语法同JS语法
   
   - 匿名函数
   
     ```typescript
     const func = function([arguments]) {return 'hello world'}
     console.log(func())
     // 自调用
     (function(){return 'aaa'})()
     ```
   
   - 函数重载
   
8. type和interface类型定义的区别[参考](https://blog.csdn.net/glorydx/article/details/112625953)

   - type可以用于其他类型，包括原始值、元组、联合类型、typeof的返回值，interface不可以
   - type可以使用in关键字生成映射类型，interface不可
   - interface可以多次被定义然后被视为合并，type不可以
   - type必须先声明后到处，interface支持同时声明默认导出

9. Number

   - valueOf() 返回Number对象的原始数字值
   - toString() 传入基数2-36之间整数，默认10
   - toPrecision() 将数字格式化为指定长度，不传为原值
   - toLocalString() 转为本地数字格式顺序
   - toFixed() 对小数点指定位数，转为字符串，不传为整数
   - toExponential() 转为指数计算法

10. String

    属性：

    - length
    - constructor对创建该对象的函数的引用
    - prototype

    方法：

    - chartA() 不传为0 ，超出为空字符串
    - chartCodeAt() unicode编码返回
    - contact str.contact() 参数支持变量，字符串，数字，数组（会转成字符串值） 
    - indexOf() 返回某个字符串值在字符串中首次出现的位置，没找到为-1
    - lastIndexOf() 从后往前找，从起始位置开始计算返回字符串值最后出现的位置
    - match()
    - replace()
    - search() 类似indexOf()
    - slice 
    - split

11. Array

    - new Array(3) , new Array('a', 'b', 'c')

    - 数组迭代

      ```typescript
      const j:any; // 有无值无影响
      const nums:number[] = [1,2,3]
      for(j in nums) {
      	console.log(nums[j])
      }
      
      const d = 1;
      for(d in ['1',2]){console.log(['1',2][d]); console.log(d)}
      ```

    数组方法：

    - concat() 同string，
    - every()
    - some()
    - filter() 返回符合条件的所有元素数组
    - forEach为每个元素执行一次回调
    - indexOf() 同string
    - lastIndexOf() 同string
    - join()
    - map() 返回处理后的数组
    - pop() 删除最后一个元素并返回该元素
    - push() 会改变原数组，返回新数组长度
    - reduce() 将数组元素计算成一个值
    - reduceRight() 从右到左计算成一个值
    - reverse() 反转 会改变原数组
    - shift() 删除并返回数组的第一个元素
    - unshift() 向数组开头添加元素，并返回新长度
    - slice()
    - sort()
    - splice() 删除或添加数组
    - toString()

12. Map

13. Tuple

    - 存储的数据类型不同，则需要使用元组

      ```typescript
      const a = ['1', 1]
      ```

    - pop() push()

    - 直接通过索引赋值从而更新元组

    - 同数组解构

14. 联合类型

    - 联合类型`const a:string|number|bolean`
    - 联合类型数组`const a:string[]|number[]`

15. interface
    - 接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要具体的类去实现，给第三方调用

    - 普通接口

      ```typescript
      interface Person {
        	name: string,
          sayHi: () => string,
          commandLine: string[] | string, // 接口和联合类型
        }
        const person: Person = {
        	name: '2oops',
          sayHi: ():string => {return 'hello'},
          conmandLine: 'aaa'
        }
      ```

    - 接口和数组

      ```typescript
      interface name {
      	[index:number]: number
      }
      const n:name = ['a', 1] // a不是number类型会导致报错
      ```

    - 接口继承，接口可以继承多个接口

      单继承

      ```typescript
      interface A {
      	name: string
      }
      interface B extends A {
      	age: number
      }
      const ab = <B>{}
      ab.name = '2oops'
      ab.age = 20
      ```

      多继承

      ```typescript
      interface P1 {
      	age: number
      }
      interface P2 {
        name: string
      }
      interface Child extends P1, P2 {}
      const child: Child = {age: 20, name: '2oops'}
      ```


16. 类

    - 创建类

    ```typescript
    class Car {
    	engine: string; // 注意是分号
      constructor(engine: string) {
    		this.engine = engine
      };
      desc(): void {
        console.log(this.engine)
      }
    }
    ```

    - 类继承

    ```typescript
    class Shape {
    	Area: number;
      constructor(a: number) {
    		this.Area = a;
      }
    }
    class Circle extends Shape {
    	desc(): void {
        console.log(this.Area)
      }
    }
    const circle = new Circle(20)
    circle.desc(); // 20
    ```

    子类只能继承一个父类，不支持继承多个类，但支持多重继承

    `C继承B，B继承A`

    ```typescript
    class A {}
    class B extends A{}
    class C extends B{}
    const c = new C()
    ```

    - 类的方法重写

    ```typescript
    class A {
      print():void {
        console.log('A')
      }
    }
    class B extends A {
    	print():void {
        super.print(); // A super关键字是对父类的直接引用，可以引用父类的属性和方法
        console.log('B')
    }
    }
    ```

    - static关键字，可以直接使用类名调用，定义类的数据成员为静态

    - instanceof运算后为true

    - 访问控制修饰符

      分别为public（任何地方可调用），protected（可以被自身及其子类和父类访问），private（只能被定义所在的类访问）

    - 类和接口 implements （感觉没啥用）

17. 对象

    - 声明

      ```typescript
      const a = {
      	name: '2oops',
        age: '20'
      }
      ```

      与JS的区别在于声明了对象之后不能和JS那样直接添加属性，需要先给定类型

    - 对象可以作为参数传递给函数

    - 鸭子类型

18. 命名空间

    - 不同空间中的成员可以同名，因为已经属于不同的命名空间
    - 如果需要外部调用命名空间成员，须export
    - 使用`.`调用命名空间内成员

19. 模块

    ```typescript
    import A = require('./B')
    ```

20. 声明文件

    为应对引用第三方JS库，无法使用类型检查的功能，需要额外的声明文件只保留导出类型声明