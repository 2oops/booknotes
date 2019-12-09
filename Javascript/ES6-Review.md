# ES6-Review

```javascript
let a = 1
console.log('a')

const b = 1

let binary = 0B0101
console.log(binary)

let oo = 0o32
console.log(oo)

console.log(Number.isNaN(1)) // false
console.log(Number.isNaN(NaN))

console.log(Number.MAX_SAFE_INTEGER)

console.log(Number.isSafeInteger(oo))


// Array.form
let json = {
  '0': '1',
  '1': '2',
  '2': '222',
  length: 3
}
console.log(Array.from(json))

// Array of
let arr = Array.of(3,4,5,6)
console.log(arr)

// find()实例方法
console.log(arr.find((value, index, arr) => {
  return value > 4
})) // 5

// fill
let arr1 = [1,2,4,5]
console.log(arr1.fill('aaa', 1, 3)) // [1, "aaa", "aaa", 5]

// for of
for(let i of arr1.keys()) {
  console.log(i)
}

// entries条目
for(let [index, val ] of arr1.entries()) {
  console.log(`${index}: ${val}`) 
  //0: 1
  // 1: aaa
  // 2: aaa
  // 3: 5
}


let list = arr1.entries()
console.log(list.next().value) //[0, 1]
console.log(list.next()) // {value: Array(2), done: false}

// throw new Error('aaa')

// 对象的函数解构
let json1 = {
  'name': '2oops',
  'age': 20
}
function fun({ name, age = 21 }) {
  console.log(name, age)
}
fun(json1)

// in
let obj = {
  name: '2oops',
  age: 20
}
console.log(name in obj) // 0 2oops

// forEach
let arr2 = ['2oops', 21]
arr2.forEach((val, index) => {console.log(index, val)})

// filter
let res = arr2.filter(x => { return x === 21})
console.log(res) // [21]

// some
arr2.some(x => console.log(x)) // 2oops 21

// map 替换
let res1 = arr2.map(item => 'haha')
console.log(res1) // ['haha', 'haha']

// join
console.log(arr2.join('-')) //2oops-21


// key值的构建
let key = "name"
let obj2 = {
  [key]: '2oops'
}
console.log(obj2)

// is严格相等
let obj3 = {name: '2oops'}
let obj4 = {name: '2oops'}
console.log(Object.is(obj3.name, obj4.name)) //true

console.log( +0 === -0) // true
console.log(NaN === NaN) // false
console.log(Object.is(+0, -0)) //false
console.log(Object.is(NaN, NaN)) //true

// assign()
let obj5 = {age: 20}
let obj6 = Object.assign(obj4, obj5)
console.log(obj6)

// symbol
let f = Symbol()
console.log(typeof(f)) // Symbol
let oops = Symbol('2oops')
console.log(oops) // Symbol(2oops)
console.log(oops.toString()) // Symbol(2oops)

let obj7 = {
  [f]: '2oops'
}
obj7[f] = '2'
console.log(obj7[f])

// symbol的对象保护
let xiaowang = {
  name: 'xiaowang',
  age: '20',
  // phoneNumber: '2131241234'
}
// 保护phoneNumber
let phoneNumber = Symbol()
xiaowang[phoneNumber] = '31432432'
for( let item in xiaowang) {
  console.log(xiaowang[item])
} 
console.log(xiaowang[phoneNumber])

// set去重
let setArr = new Set(['2oops', 20, 20]) //不允许重复

console.log(setArr) //Set(2) {"2oops", 20}
setArr.add('xiaopp') // 添加
setArr.delete(20) // 删除
// setArr.clear() // 全部删除 Set {}
console.log(setArr)
// has查找
console.log(setArr.has(20)) // false

for( let item of setArr) {
  console.log(item)
}
setArr.forEach(x => console.log(x))

console.log(setArr.size) //去重后的大小

// weakSet
let weakObj = new WeakSet()
// 须要使用add添加对象
let oops1 = {name: '2oops'}
weakObj.add(oops1) //add 的对象要写在外面
// 且即使对象完全相等，但栈内存不一样就都会add进weakSet
console.log(weakObj)

// map数据类型
let json2 = {
  name: 'xiaopp',
  age: 21
}
console.log(json2.name)

let map1 = new Map()
map1.set(json2, 'hello')
map1.set('oops', json2)
console.log(map1) // Map(2) {{…} => "hello", "oops" => {…}}

// 增删改查
console.log(map1.get('oops'))
console.log(map1.get(json2))
map1.delete(json2)
// map1.clear()
console.log(map1.size)
console.log(map1.has('oops'))

// Proxy 增强对象和函数（方法） 生命周期 预处理
let pro = {
  add: (x) => {
    return x + 100
  },
  name: '2oops'
}
let proxy1 = new Proxy({
  add: (x) => {
    return x + 100
  },
  name: '2oops'
}, {
  // get set apply
  get: (target, key, property) => {
    console.log('come in get')
    return target[key]
  },
  set: (target, key, value, receiver) => {
    console.log(`setting ${key} to value ${value}`)
    return target[key] = value
  }
})
console.log(proxy1.name) // 2oops
proxy1.name = 'changed'
console.log(proxy1.name) // changed

let target = () => 'hello 2oops'
let handler = {
  apply(target, ctx, args) {
    console.log('applying')
    return Reflect.apply(...arguments)
  }
}
let proxy2 = new Proxy(target, handler)
console.log(proxy2()) // applying hello 2oops

// Promise 解决回调地狱的问题
let state = 1 
function step1(resolve, reject) {
  console.log('start step1')
  if(state === 1) {
    resolve('step1 resolved')
  } else {
    reject('step1 rejected')
  }
}
function step2(resolve, reject) {
  console.log('start step2')
  // state = 0
  if(state === 1) {
    resolve('step2 resolved')
  } else {
    reject('step2 rejected')
  }
}
function step3(resolve, reject) {
  console.log('start step3')
  if(state === 1) {
    resolve('step3 resolved')
  } else {
    reject('step3 rejected')
  }
}

new Promise(step1).then(val => {
  console.log(val)
  return new Promise(step2)
}).then(val => {
  console.log(val)
  return new Promise(step3)
}).then(val => {
  console.log(val)
})

// class
class Animal{
  name(val) {
    console.log(val)
    return val
  }
  // 方法之间不要写逗号
  skill() {
    // this指的是class这层
    console.log(this.name('xiaop'))
  }

  constructor(a, b) {
    this.a = a 
    this.b = b
  }
  add() {
    return this.a + this.b
  }
}

let dog = new Animal(1,3)
dog.name('pp')
dog.skill()
console.log(dog.add()) // 4

class Cat extends Animal {

}
let cat = new Cat
cat.name('miaomiao')
```