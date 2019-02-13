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

10. Proxy

      ![img](https://user-gold-cdn.xitu.io/2019/2/12/168df9ec56c9a8f0?imageslim)

11. Object.assign(属于浅拷贝)
       ```js
    let target = {};
    let obj = Object.assign(target, {a: 1}, {b: 2})
    obj //{a: 1, b: 2}
       ```