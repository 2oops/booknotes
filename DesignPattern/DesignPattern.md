# DesignPattern

1. ```
   let data = [{oid: "111",name: "zhangsan",age: "20"},{oid: "22",name: "lisi",age: "220"}]
   let oids = data.map(item => item.oid)
   console.log(oids)
   ```

2. ES5非严格模式下,在div节点的事件函数内部,有一个局部的callback方法,callback被作为普通函数调用时,callback内部的this指向了window,但在严格模式下,则返回undefined

```
 <html>
    <body>
      <div id="div1">这是一个div</div>
    </body>
  <script>

    window.id = "window";
    function func() {
      "use strict"
      
      document.getElementById("div1").onclick = function() {
        console.log(this.id);
        // let that = this;
        let callback = function() {
          console.log(this);
          console.log(this.id);
        }
        
        callback();
      };
    }
    
    func();
  </script>
</html>
```

1. 大部分的JS函数都可以当做构造器使用,构造器的外表和普通函数一模一样,区别在于被调用的方式.当用new运算符调用函数时,该函数会返回一个对象,通常情况下,构造器里的this就指向返回的这个对象.
   new 调用构造器时，还要注意一个问题，如果构造器显式地返回了一个object 类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的this：
   如果构造器不显式地返回任何数据，或者是返回一个非对象类型的数据，就不会造成上述问题：

2. ```
   func.apply(null,[1,2,3])
      func.call(null, 1,2,3)
   ```

     因为JS的参数在内部就是用一个数组来表示的,从这个意义上来说,apply比call的使用率更高
     call 是包装在apply 上面的一颗语法糖，如果我们明确地知道函数接受多少个参数，而且想一目了然地表达形参和实参的对应关系，那么也可以用call 来传送参数.
     当使用call 或者apply 的时候，如果我们传入的第一个参数为null，函数体内的this 会指
     向默认的宿主对象，在浏览器中则是window：但严格模式下依然为null

5.

```
	// 实现一个bind
  Function.prototype.bind = function(context) {
    let self = this;
    return function() {
   return self.apply(context, arguments);
    }
  };

let obj6 = {
  name: "xiaowangba"
}

let getName3 = function() {
  console.log(this.name);
}.bind(obj6);

getName3();

// 优化bind
Function.prototype.bing = function() {
  let self = this,
​    context = [].shift.call(arguments),
​    args = [].slice.call(arguments);
​    return function() {
​      return self.apply(context, [].concat.call(args, [].slice.call(arguments)));
​    }
};

let obj7 = {
  school:"Oxford"
}

let getSchool = function(a,b,c,d) {
  console.log(this.school);
  console.log([a,b,c,d]);
}.bing(obj7,2,3);

getSchool(4,5);
```

6.往arguments里面添加一个新元素,通常会使用`Array.prototype.push`

```
(function(){
  Array.prototype.push.call(arguments, 3);
  console.log(arguments); 
})(1,2)
```

Node_Env: { '0': 1, '1': 2, '2': 3 }<br/>
Chrome_Env: Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
注意这里的arguments需是一个Object类型,对应的3必须是一个可读写的属性,或者说须有一个length属性
7.在JavaScript的设计模式中,许多模式是可以用闭包和高阶函数来实现的
闭包->作用域->生命周期

```
let Type = {};
for(let i = 0, type; type = ["Number", "Array", "String"][i++]; ) {
  (function(type) {
    Type['is' + type] = function(obj) {
      return Object.prototype.toString.call(obj) === '[object '+ type +"]"
    }
  })(type)
}
console.log(Type.isString([]));
console.log(Type.isNumber(2));
```

8.在某种业务背景下,闭包和promise有着相同的功能

```
var extent = function() {
  let value = 0;
  return {
    call: function() {
      value++;
      console.log(value)
    }
  }
}
var extent = extent();
extent.call();
extent.call();
```

```
let Extent = function() {
  this.value = 0;
}
Extent.prototype.call = function() {
  this.value++;
  console.log(this.value);
}
let extent = new Extent();
extent.call();
extent.call();
```

1. 跟闭包和内存泄露有关系的地方是，使用闭包的同时比较容易形成循环引用，如果闭包的作
   用域链中保存着一些DOM节点，这时候就有可能造成内存泄露。但这本身并非闭包的问题，也
   并非JavaScript 的问题。在IE 浏览器中，由于BOM 和DOM中的对象是使用C++以COM 对象
   的方式实现的，而COM对象的垃圾收集机制采用的是引用计数策略。垃圾回收机制中，如果两个对象之间形成了循环引用，那么这两个对象都无法被回收，但循环引用造成的内存泄露在本质上也不是闭包造成的。<br/>
   ​	同样，如果要解决循环引用带来的内存泄露问题，我们只需要把循环引用中的变量设为null
   即可。将变量设置为null 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运
   行时，就会删除这些值并回收它们占用的内存。<br/>
   10.高阶函数:<br/>

- <u>函数可以作为参数被传递</u>
- <u>函数可以作为返回值输出</u>
  11.在Java 语言中，可以通过反射和动态代理机制来实现AOP 技术。而在JavaScript 这种动态
  语言中，AOP 的实现更加简单，这是JavaScript 与生俱来的能力。<br/>
  12.单例模式

```
// 单例模式
let getSingle = function(fn) {
  let ret;
  return function() {
     (fn.apply(this, arguments))
  }
}
let getScript = getSingle(function() {
  return document.createElement("script");
})
let script1 = getScript();
let script2 = getScript();
console.log(script1 === script2);
```

13.AOP<br/>

```
Function.prototype.before = function(beforeFn) {
  let _self = this;
  return function() {
    beforeFn.apply(this, arguments);
    return _self.apply(this, arguments);
  }
};

Function.prototype.afterFn = function(afterFn) {
  let _self = this;
  return function() {
    let ret = _self.apply(this, arguments);
    afterFn.apply(this, arguments);
    return ret;
  }
};

let func4 = function() {
  console.log(2);
}

func4 = func4.before(function() {
  console.log(1)
}).afterFn(function() {
  console.log(3);
});

func4();
```

