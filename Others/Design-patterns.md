# Design-patterns

1. map

   ```javascript
   let data = [{oid: "111",name: "zhangsan",age: "20"},{oid: "22",name: "lisi",age: "220"}]
   let oids = data.map(item => item.oid)
   console.log(oids)
   ```

2. ES5非严格模式下,在div节点的事件函数内部,有一个局部的callback方法,callback被作为普通函数调用时,callback内部的this指向了window,但在严格模式下,则返回`undefined`

   ```html
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

3. 大部分的JS函数都可以当做构造器使用,构造器的外表和普通函数一模一样,区别在于被调用的方式.当用new运算符调用函数时,该函数会返回一个对象,通常情况下,构造器里的this就指向返回的这个对象.
   new 调用构造器时，还要注意一个问题，如果构造器显式地返回了一个object 类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的this：
   如果构造器不显式地返回任何数据，或者是返回一个非对象类型的数据，就不会造成上述问题

4. 因为JS的参数在内部就是用一个数组来表示的,从这个意义上来说,apply比call的使用率更高
   call 是包装在apply 上面的一颗语法糖，如果我们明确地知道函数接受多少个参数，而且想一目了然地表达形参和实  参的对应关系，那么也可以用call 来传送参数.
   当使用call 或者apply 的时候，如果我们传入的第一个参数为null，函数体内的this 会指
   向默认的宿主对象，在浏览器中则是window：但严格模式下依然为null

   ```javascript
   func.apply(null,[1,2,3])
      func.call(null, 1,2,3)
   ```

5. 实现一个bind

   ```javascript
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

6. 往arguments里面添加一个新元素,通常会使用`Array.prototype.push`

   ```javascript
   (function(){
     Array.prototype.push.call(arguments, 3);
     console.log(arguments); 
   })(1,2)
   ```

   ```javascript
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

7. 在某种业务背景下,闭包和promise有着相同的功能

   ```javascript
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

   ```javascript
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

8. 跟闭包和内存泄露有关系的地方是，使用闭包的同时比较容易形成循环引用，如果闭包的作
   用域链中保存着一些DOM节点，这时候就有可能造成内存泄露。但这本身并非闭包的问题，也
   并非JavaScript 的问题。在IE 浏览器中，由于BOM 和DOM中的对象是使用C++以COM 对象
   的方式实现的，而COM对象的垃圾收集机制采用的是引用计数策略。垃圾回收机制中，如果两个对象之间形成了循环引用，那么这两个对象都无法被回收，但循环引用造成的内存泄露在本质上也不是闭包造成的。
   同样，如果要解决循环引用带来的内存泄露问题，我们只需要把循环引用中的变量设为null
   即可。将变量设置为null 意味着切断变量与它此前引用的值之间的连接。当垃圾收集器下次运
   行时，就会删除这些值并回收它们占用的内存。

9. 高阶函数:

- <u>函数可以作为参数被传递</u>
- <u>函数可以作为返回值输出</u>

10. 在Java 语言中，可以通过反射和动态代理机制来实现AOP 技术。而在JavaScript 这种动态
      语言中，AOP 的实现更加简单，这是JavaScript 与生俱来的能力。

11. 单例模式

    ```javascript
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

12. AOP

    ```javascript
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

13. 单例模式

核心是确保只有一个实例,并且提供全局访问.

1)用一个变量来标志当前是否已经为某个类创建过对象,如果是,则在下一次获取该类的实例时,直接返回之前创建的对象:

```
  var Singleton = function( name ){
    this.name = name;
    this.instance = null;
  };
  Singleton.prototype.getName = function(){
    alert ( this.name );
  };
  Singleton.getInstance = function( name ){
    if ( !this.instance ){
      this.instance = new Singleton( name );
    }
    return this.instance;
  };
  
  var a = Singleton.getInstance( 'sven1' );
  var b = Singleton.getInstance( 'sven2' );
  alert ( a === b ); // true
```

2)用代理实现单例模式

```
let createDiv = function(html) {
    this.html = html;
    this.init();
  }
  createDiv.prototype.init = function() {
    let div = document.createElement('div');
    div.innerHTML = this.html;
    document.body.appendChild(div);
  }
  // 引入代理
  let proxySingtonCreateDiv = (function() {
    let instance;
    return function(html) {
      if(!instance) {
        instance = new createDiv(html);
      }
      return instance;
    }
  })()

  let a2 = new proxySingtonCreateDiv("a2");
  let b2 = new proxySingtonCreateDiv("b2");
  console.log(a2 === b2);
```

3)降低全局变量带来的命名污染

<u>使用命名空间</u>

<u>使用闭包封装私有变量</u>

```
  var user = (function(){
    var __name = 'sven',
    __age = 29; // 使用_name,_age表示私有变量
    return {
      getUserInfo: function(){
        return __name + '-' + __age;
      }
    }
  })();
  user.getUserInfo(); //seven-29
```

4)*惰性单例

```javascript
<html>
  <body>
    <button id="loginBtn">登录</button>
  </body>
<script>
  let createLoginLayer = (function(){
    let div;
    return function(){
      if ( !div ){
        div = document.createElement( 'div' );
        div.innerHTML = '我是登录浮窗';
        div.style.display = 'none';
        document.body.appendChild( div );
      }
      return div;
    }
  })();

  document.getElementById( 'loginBtn' ).onclick = function(){
    let loginLayer = createLoginLayer();
    loginLayer.style.display = 'block';
  };
</script>
</html>
```

5)创建对象和管理单例(单一职责原则)分布在两个不同的方法中,结合这两种方法实现单例模式

```html
<html>
  <body>
    <button id="loginBtn">登录</button>
  </body>
<script>
  
  let getSingle = function( fn ){
    let result;
    return function(){
      return result || ( result = fn .apply(this, arguments ) );
    }
  };
  // let createLoginLayer = function(){
  //   let div = document.createElement( 'div' );
  //   div.innerHTML = '我是登录浮窗';
  //   div.style.display = 'none';
  //   document.body.appendChild( div );
  //   return div;
  // };
  // let createSingleLoginLayer = getSingle( createLoginLayer );
  // document.getElementById( 'loginBtn' ).onclick = function(){
  //   let loginLayer = createSingleLoginLayer();
  //   loginLayer.style.display = 'block';
  // };
  let createSingleIframe = getSingle( function(){
    let iframe = document.createElement ( 'iframe' );
    document.body.appendChild( iframe );
    return iframe;
  });
  document.getElementById( 'loginBtn' ).onclick = function(){
    let loginLayer = createSingleIframe();
    loginLayer.src = 'http://baidu.com';
  };
</script>
</html>
```

2.策略模式

定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换

3.20190107

1)代理模式(虚拟代理，缓存代理)

```javascript
let mult = function() {
	console.log("start");
	let a = 1;
	for(let i = 0; i < arguments.length; i++) {
		a *= arguments[i];
	}
	return a;
}
mult(2,3,4);

proxyMult = (function() {
	let cache = {};
	return function() {
		let args = Array.prototype.join.call(arguments, ',');
		if(args in cache) {
			return cache[args];
		}
		return cache[args] = mult.apply(this, arguments);
	}
})();
proxyMult(2,3,4);
```

缓存代理可以为一些开销大的运算结果提供暂时的存储，在下次运算时，如果传递进来的参数跟之前一致，则可以直接返回前面存储的运算结果。

2）迭代器模式

```javascript
// 倒序迭代器
let reverseIterator = function(arr, callback) {
	for (let l = arr.length - 1; l >= 0; l--) {
		callback(l, arr[l]);
	}
}
reverseIterator([1,2,3], function(i, n) {
	console.log(n) // 分别输出3 ，2 ，1
})
```

```javascript
// 中止迭代器
let breakIterator = function(arr, callback) {
	for(let i = 0; i < arr.length; i++) {
		if(callback(i, arr[i]) === false) {
			break;
		}
	}
}
breakIterator([1,2,3,4,5], function(i, n) {
	if(n > 2) {
		return false
	}
	console.log(n); // 分别输出1，2
})
```

3）发布-订阅模式（观察模式）

```javascript
document.body.addEventListener('click', function() {
	alert(2)
}, false)
document.body.click();
```

4）命令模式

命令模式最常见的应用场景是：有时候需要向某些对象发送请求，但是并不知道请求的接收者是谁，也不知道被请求的操作是什么。

5）模板方法模式

```javascript
let soakingDrink = function() {};
soakingDrink.prototype.boilWater = function() {
	console.log("烧开水");
}
soakingDrink.prototype.brew = function() {
	// console.log("冲泡")
}
soakingDrink.prototype.pourInCup = function() {
	// console.log("倒入杯中")
}
soakingDrink.prototype.addMaterials = function() {
	// console.log("加料")
}

soakingDrink.prototype.init = function() {
	this.boilWater();
	this.brew();
	this.pourInCup();
	this.addMaterials();
}

let Coffee = function() {}
Coffee.prototype = new soakingDrink();

Coffee.prototype.brew = function() {
	console.log("用开水冲泡咖啡")
}
Coffee.prototype.pourInCup = function() {
	console.log("咖啡倒入杯中")
}
Coffee.prototype.addMaterials = function() {
	console.log("加入糖")
}

let coffee = new Coffee();
coffee.init();

let Tea = function() {}
Tea.prototype = new soakingDrink();

Tea.prototype.brew = function() {
	console.log("用开水泡茶")
}
Tea.prototype.pourInCup = function() {
	console.log("茶倒入杯中")
}
Tea.prototype.addMaterials = function() {
	console.log("茶中加入柠檬")
}

let tea = new Tea();
tea.init();
```

模板方法即为`soakingDrink.prototype.init`

6）享元模式

享元模式是为解决性能问题而生的模式，在一个存在大量相似对象的系统中，享元模式可以很好地解决大量对象带来的性能问题。

7）装饰者模式

```javascript
let Plane = function() {};
Plane.prototype.fire = function() {
	console.log("shoot")
};
let missileDecorator = function(plane) {
	this.plane = plane;
}
missileDecorator.prototype.fire = function()  {
	this.plane.fire();
	console.log("发射导弹")
}
let atomDecorator = function(plane) {
	this.plane = plane;
}
atomDecorator.prototype.fire = function() {
	this.plane.fire();
	console.log("发射原子弹")
}

let feiji = new Plane();
feiji = new missileDecorator(feiji);
feiji = new atomDecorator(feiji);

feiji.fire();
```

数据上报、统计函数的执行时间、动态改变函数参数以及插件式的表单验证等场景中很适合装饰者模式，除了上面提到的例子，它在框架开发中也十分有用。作为框架作者，我们希望框架里的函数提供的是一些稳定而方便移植的功能，那些个性化的功能可以在框架之外动态装饰上去，这可以避免为了让框架拥有更多的功能，而去使用一些 if、else 语句预测用户的实际需要。 

8）状态模式

9）程序设计原则

单一职责原则（SRP）

```
* 代理模式
* 迭代器模式
* 单例模式
* 装饰者模式
```

最少知识原则

```
* 中介者模式
* 外观模式
```

开放-封闭原则

```
* 发布-订阅模式
* 模板方法模式
* 策略模式
* 代理模式
* 职责链模式
```

开放-封闭原则，假设一个onload函数有500行，此时需要添加新功能，这里使用动态装饰
的方式，完全不用理会之前的onload函数有多复杂及其内部实现，只要它是稳定运行的函数，那么
之后也不会因为我们添加的函数而产生错误。

```javascript
Function.prototype.after = function( afterfn ){
	let __self = this;
	return function(){
		let ret = __self.apply( this, arguments );
		afterfn.apply( this, arguments );
		return ret;
	}
};
window.onload = ( window.onload || function(){} ).after(function(){
 console.log( document.getElementsByTagName( '*' ).length );
}); 
```

10）代码重构

```
* 提炼函数
* 合并重复的条件片段
* 把条件分支语句提炼成函数
* 合理使用循环
* 提前让函数退出代替嵌套条件分支
* 传递对象参数代替过长的参数列表
* 尽量减少参数数量
* 合理使用三目运算符
* 合理使用链式调用
* 用return退出多重循环
```

