# ES6

1. `let & const`
2. 箭头函数
  * 箭头函数没有arguments

  * 箭头函数没有prototype属性，没有constructor，即不能用作构造函数，不能用new调用

  * 箭头函数没有自己的this, this等于它的上下文

  ```javascript
let let controller = {
makeRequest: function() {
setTimeout(function() {
  console.log(this.a) //undefined,此时this指向window，变量a丢失
})
},
a: 1
}
controller.makeRequest()
  ```
  ```javascript
let controller = {
makeRequest: function() {
let that = this;
setTimeout(function() {
  console.log(that.a) //1,不推荐该ES5写法
})
},
a: 1
}
controller.makeRequest()
  ```
  ```javascript
let controller = {
makeRequest: function() {   
setTimeout(() => {
  console.log(this.a) //1,ES6写法,注意这里makeRequest若再用箭头函数，this将指向window
})
},
a: 1
}
controller.makeRequest()
  ```
  * 不要在可能改变this指向的函数中使用箭头函数，类似Vue的methods，computed中的方法

3. iterator迭代器
  * 对于可迭代的数据结构，ES6内部部署了一个`Symbol.iterator`属性，数组中的该属性默认部署在数组原型上
  ```javascript
  let arr = [1,2,3];
  let iterator = arr[Symbol.iterator](); //这里Symbol.iterator不能写成arr.Symbol.iterator()
  iterator.next(); //{value: 1, done: false}
  iterator.next(); //{value: 2, done: false}
  iterator.next(); //{value: 3, done: false}
  iterator.next(); //{value: undefined, done: true},此为逐步执行的结果，4个next()全执行则只返回最后结果
  ```

4. 解构赋值
  * 举例Vuex中的actions
  ```javascript
  // 不使用对象解构
  actions: {
  increment(ctx) {
    ctx.commit('increment')
  }
  }
  ```
  ```javascript
  // 解构
  actions: {
  increment({commit}) {
    commit('increment')
  }
  }
  ```
  * 对axios的响应结果解构
  ```javascript
let {data} = await axios.get("http:localhost:3000")
  ```

5. 扩展运算符(ES9中支持其在对象中使用)
  * 数组扩展(只要含有iterator接口的数据结构都可以用扩展运算符)
  ```javascript
let arr = [1,3,4];
let arr1 = [...arr,6,7]; // [1,3,4,6,7]
  ```

  * 扩展运算符和解构赋值一起使用，但是必须是**最后一个**
  ```javascript
let[first, ...arr] = [1,2,3];
first //1
arr // [2,3]
let [...arr1, last] = [2,4,5] // Uncaught SyntaxError: Rest element must be last element,此时没有迭代器可用，故报错
  ```
  * 剩余运算符
  ```javascript
function func1(a,b,c) {
    console.log(arguments[0],arguments[1],arguments[2]) // 1 2 3
}
func1(1,2,3);
function func2(...rest) { //rest是形参
    console.log(rest); //[4,5,6]
}
func2(4,5,6);
  ```
6. for...of循环
  * 内部机制可以如下理解：
  ```javascript
let arr = [1,2,3];
let iterator = arr[Symbol.iterator]()
for(let value, res; (res = iterator.next()) && ！res.done;) {
  value = res.value;
}
  ```
  * for...of 循环同时支持break，continue,return(在函数中调用的话)，并且可以和对象的解构赋值一起使用
  ```javascript
let arr = [{a: 1},{a: 2},{b: 1}, {a: 0}]
let obj = {};
for ({a: obj.a} of arr) {
console.log(obj.a); //1 => 2 => undefined => 0
}
  ```
7. Promise
  ```javascript
let promise = new Promise((resolve, reject) => {
setTimeout(() =>{
resolve("I have been resolved")
 }, 2000)
})
promise.then((res) => {
 console.log(res)
}) 
  ```

   ```javascript
   axios.get("http://localhost:3000")
   .then(res => {
       //do something
   })
   .catch(err => {
       //catch error
   })
   ```

8. Module
  * `export {x}` 导出的是一个对变量x的引用，`export default y`导出的是y这个值
  * import Vue中路由的懒加载的ES6写法就是使用了这个技术,使得在路由切换的时候能够动态的加载组件渲染视图
9. 函数默认值
  ```js
function func(a) { //ES5
return a || 1;
}
func();
function func1(a = 1) { // ES6
return a;
}
func1();
  ```
```js
function func2({x = 10} = {}, {y} = {y: 10}) {
console.log(x,y)
}
func2({}, {}); // 10 undefined
func2(undefined, {}); // 10 undefined
func2(undefined, undefined); // 10 10
func2(); // 10 10
func2({x: 1}, {y:2}); // 1 2
```

10. Proxy对象代理

    ```js
    let Person = {
      name: '2oops',
      age: 22,
      desc: 'handsome'
    }
    let person = new Proxy(Person, {
      get(target, key) {
        return target[key];
      },
      set(target, key, value) {
        if(key !== 'age') {
          target[key] = value;
        }
      }
    })
    console.table({
      name: person.name,
      age: person.age,
      desc: person.desc
    })
    
    try {
      person.age = '23';//这里是不可以改的
    }
    catch(e){
      console.log(e)
    }
    finally {
      
    }
    ```



      ![img](https://user-gold-cdn.xitu.io/2019/2/12/168df9ec56c9a8f0?imageslim)

11. Object.assign(属于浅拷贝)
       ```js
    let target = {};
    let obj = Object.assign(target, {a: 1}, {b: 2})
    obj //{a: 1, b: 2}
       ```

12. Promise

    1. ```js
       //多步执行
       console.log("here we go");//(1)here we go
       new Promise( resolve => {
         setTimeout(() => {
           resolve('hello');
         }, 2000)
       })
       .then(value => {
         console.log(value);//(2)hello
         //状态响应函数可以返回新的promise，或其他值，如果返回新的promise，那么下一级`.then()`会在新
         //的promise改变后执行
         return new Promise( resolve => {
           setTimeout(() => {
             resolve('second')
           }, 3000);
         })
       })
       .then((value) => {
         console.log(value + ' 2oops');//(3)second 2oops
       })
       ```

    2. ```js
       //promise已经完成了再接then
       console.log("start");//(1)
       let promise = new Promise(resolve => {
         setTimeout(() => {
           console.log("promise fulfilled");//(2)
           resolve("I have resolved");
         }, 2000);
       })
       setTimeout(() => {
         promise.then(value => {
           console.log(value);//(3)I have resolved
         })
       }, 3000);
       // 例子展示了promise作为队列的重要特性，可以给promise添加任意个then,若该promise没有完成，则接着执行
       // 若已完成，后面的then也会得到前面promise返回的值 
       ```

    3. ```js
       //then里面不返回promise
       console.log("here we go");//(1)
       new  Promise(resolve => {
         setTimeout(() => {
           resolve('hello')
         }, 2000);
       })
       .then(value => {
         console.log(value);//(2)
         console.log("everyone");//(3)
         ((function() {
           return new Promise(res => {
             setTimeout(() => {
               console.log('I am 2oops');//(5)
               res('I am');
             }, 0);//即使这里不是0也要等外面的promise(整个进程)执行完才会执行当前promise
           })
         })())
         return "ddd";//如果返回其他值，则会立即执行下一级`.then()`
       })
       .then( value => {
         console.log(value + ' 2oops');//(4) ddd 2oops
       })
       .catch(err => {
         console.log(err)
       })
       ```

    4. `.then`接受两个函数作为参数，分别代表`fulfilled`和`rejected`;
       `.then`返回一个新的Promise实例，所以它可以链式调用
       状态响应函数可以返回新的promise，或其他值，如果返回新的promise，那么下一级`.then()`会在新的promise改变后执行,如果返回其他值，则会立即执行下一级`.then()`

    5. ```js
       //then里面有then
       console.log('start');
       new Promise(resolve => {
         console.log('step 1')
         setTimeout(() => {
           resolve(100)
         }, 1000);
       })
       .then( value => {
         // 可以在这里return value使得第一级promise直接往下调用
         console.log("value: " + value);//会输出value: 100
         return new Promise( resolve => {
           console.log(value);//新promise中能否拿到上一级promise传过来的value?100，答案为是
           console.log("step 1-1");
           setTimeout(() => {
             resolve(110)
           }, 1000);
         })
         .then(value => {
           console.log("step 1-2")
           return value
         })
         .then(value => {
           console.log("step 1-3");
           return value;
         })
       })
       .then(value => {//此层的then可以前提到上一行}）里面，返回结果是不变的
         console.log(value);
         console.log("step 2");
       })
       // start => step 1-1 => step 1-1 => step 1-2 => step 1-3 => 110 => step 2
       ```

    6. 四种promise的区别

       ```js
       dosomething().then(function() {
           return dosomethingelse();//正常
       })
       
       dosomething().then(function() {
           dosomethingelse();//undefined
       })
       
       dosomething().then(dosomethingelse());//dosomethingelse被忽略掉，不存在队列当中，undefined
       
       dosomething().then(dosomethingelse);//正常
       ```

    7. 错误处理的两种方法：

       1. `throw new Error('err msg').catch(msg => {})`

       2. `reject('err msg').then(null, mesg => {})`

          ```js
          console.log("start")
          new Promise((resolve, reject) => {
            setTimeout(() => {
              throw new Error("err msg");
              // resolve('ok')
              // reject("err")
            }, 1000);
          })
          .then((value) => {
            // throw new Error("then err")
            console.log("value: " + value);//因为报错，这里不会执行
          })
          .catch(msg => {
            console.log(msg)
          })
          ```

    8. ```js
       //catch() 后接then() catch也返回了一个promise实例
       new Promise(resolve => {
         console.log("start");
         setTimeout(() => {
           resolve('resolve value')
         }, 1000);
       })
       .then( value => {
         console.log(value);//resolve value
         throw new Error('test err')
       })
       .catch(err => {
         console.log(err);//test  err
       })
       .then(() => {
         console.log("arrive here");//有输出
       })
       .catch((error) => {
         console.log(error)//没有被执行
       })
       ```

    9. Promise.all()包装多个promise实例，然后返回一个普通的promise实例

       ```js
       console.log("start")
       Promise.all([1,2,3])
       .then( all => {
         console.log("step1 :" + all);//传入的数组元素不是实例，直接返回数组
         return Promise.all([function(){//验证下一个promise.all
           console.log("这里不打印");
         }, 'dayin', false])
       })
       .then( all => {
         console.log("stet2: " + all);//同样直接返回
       
         let p1 = new Promise( resolve => {
           setTimeout(() => {
             resolve("p1")
           }, 2000);
         })
         let p2 = new Promise( resolve => {
           setTimeout(() => {
             resolve("p2")
           }, 1000);
         })
         return Promise.all([p1, p2]);
       })
       .then(all => {
         console.log("step3: " + all);//p1,p2
         let p3 = new Promise( resolve => {
           setTimeout(() => {
             resolve("p3")
           }, 2000);
         })
         let p4 = new Promise( (resolve,reject) => {
           setTimeout(() => {
             reject("p4 reject")
           }, 1000);
         })
         let p5 = new Promise( (resolve, reject) => {
           setTimeout(() => {
             reject("p5 reject")
           }, 3000);
         })
         return Promise.all([p3,p4,p5]);
       })
       .then(all => {
         console.log(all);//被reject了不会执行
       })
       .catch(err => {
         console.log(err);//p4 reject 这里输出的是最先抛出的reject
       })
       ```

    10. `promise.all()` 与 `.map`一起用

    11. promise 实现队列

        ```js
        //1.forEach
        function queue(things) {//things为Array
          let promise = Promise.resolve();
          things.forEach(item => {
            promise = promise.then(() => {
              return new Promise(resolve => {
                dothings(item, () => {
                  resolve();
                })
              })
            })
          });
          return promise;
        }
        queue(['first','second','thrid'])
        ```

        ```js
        //2.reduce
        function queue(things) {
          return things.reduce((promise, thing) => {
            return promise.then(() => {
              return new Promise(resolve => {
                dothing(thing, () => {
                  resolve();
                })
              })
            })
          }, Promise.resolve())
        }
        queue(['first','second','thrid'])
        ```

    12. Promise.resolve()

        ```js
        Promise.resolve()
        .then( value => {
          console.log("step1: " + value);//undefined
          return Promise.resolve("hello");
        })
        .then (value => {
          console.log(value, "2oops");//hello 2oops
          return Promise.resolve(new Promise(resolve => {
            setTimeout(() => {
              resolve("good")
            }, 1000);
          }))
        })
        .then(value => {
          console.log(value, "afternoon");//good afternoon
          return Promise.resolve({
            then() {//注意这里是then(),即thenable,而Promise.reject()中没有
              console.log("bye");//bye
            }
          })
        })
        ```

    13. Promise.reject()

    14. Promise.race()

        与`Promise.all()`类似，区别在于，只要任意一个完成就算完成，有一个完成即进程结束，后面的不再执行

        常见用法：把异步操作和定时器放在一起；如果定时器先触发，就认为超时，告知用户

        ```js
        let p1 = new Promise(resolve => {
          setTimeout(() => {
            resolve('p1')
          }, 10000);
        })
        let p2 = new Promise(resolve => {
          setTimeout(() => {
            resolve('p2')
          }, 1000);
        })
        Promise.race([p1,p2])
        .then(value => {
          console.log(value);//p2先执行即先输出
        })
        ```

13. class 和extends

    ```js
    class Animal {
      constructor(name, age) {//若不添加constructor,Animal本身只有__proto__
        this.name = name;
        this.age = age;
      }
      toString1() {
        console.log(`name: ${this.name}, age: ${this.age}`)
      }
    }
    let animal = new Animal('dog','4');
    animal.toString1();//name dog, age: 4
    animal.hasOwnProperty('name');//true
    animal.hasOwnProperty("toString");//false
    animal.__proto__.hasOwnProperty('toString');//true
    
    class Cat extends Animal {
      constructor(action) {
        super('cat',6)
        this.action = action;
      }
      toString2() {
        console.log(super.toString1())
      }
    }
    let cat = new Cat('catch');
    cat.toString2();//name: cat, age: 6
    console.log(cat instanceof Cat); // true
    console.log(cat instanceof Animal); // true
    ```

14. 函数参数默认值

    ```js
    function foo(height = 50, color = 'red')
    {
        // ...
    }
    //避免以下写法
    function foo(height, color)
    {
        var height = height || 50;
        var color = color || 'red';
        //...
    }
    foo(0,'');//避免参数的布尔值为false
    ```

15. 解构赋值

    ```js
    let a = [[23],3,5,"last"]
    let [first,,,b] = a;
    first;//[23]
    b;//"last"
    let [one, two, three, four] = a;
    one;//[23]
    let c, d;
    [c,d] = [12,34];
    c;//12
    let e, f;
    [e = 3, f = 5] = [4];
    e;//4
    
    const student = {
      name:'Ming',
      age:'18',
      city:'Shanghai'  
    };
    const {name,age,city} = student;
    console.log(name); // "Ming"
    ```

16. 延展操作符

    ```js
    function sum(x, y, z) {
      return x + y + z;
    }
    const numbers = [1, 2, 3];
    
    //不使用延展操作符
    console.log(sum.apply(null, numbers));//6
    //使用延展操作符
    console.log(sum(...numbers));// 6
    
    const stuendts = ['Jine','Tom']; 
    const persons = ['Tony',... stuendts,'Aaron','Anna'];//
    ```

17. 对象属性简写

    ```js
    const name='Ming',age='18',city='Shanghai';
      
    const student = {
        name,
        age,
        city
    };
    console.log(student);//{name: "Ming", age: "18", city: "Shanghai"}
    ```
