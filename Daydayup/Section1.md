# Section1

1.20190102

1)函数被频繁调用

<u>window.resize事件</u>

<u>mousemove事件</u>

<u>上传进度</u>

2)函数节流(setTimeout)

3)分时函数

```
var throttle = function ( fn, interval ) {
    var __self = fn, // 保存需要被延迟执行的函数引用
    timer, // 定时器
    firstTime = true; // 是否是第一次调用
    return function () {
    var args = arguments,
    __me = this;
    if ( firstTime ) { // 如果是第一次调用，不需延迟执行
    __self.apply(__me, args);
    return firstTime = false;
    }
    if ( timer ) { // 如果定时器还在，说明前一次延迟执行还没有完成
    return false;
  }
  timer = setTimeout(function () { // 延迟一段时间执行
  clearTimeout(timer);
  timer = null;
  __self.apply(__me, args);
  }, interval || 2000 );
  };
  };
  window.onresize = throttle(function(){
  console.log( 1 );
  }, 2000 );
```

4)惰性加载函数

```
<html>
  <body>
    <div id="div1">点我绑定事件</div>
<script>
  var addEvent = function( elem, type, handler ){
    if ( window.addEventListener ){
      addEvent = function( elem, type, handler ){
        elem.addEventListener( type, handler, false );
      }
    }else if ( window.attachEvent ){
      addEvent = function( elem, type, handler ){
        elem.attachEvent( 'on' + type, handler );
      }
    }

    addEvent( elem, type, handler );
  };

  var div = document.getElementById( 'div1' );
  addEvent( div, 'click', function(){
    alert (1);
  });
  addEvent( div, 'click', function(){
    alert (2);
  });
</script>
</body>
</html>
```

2.单例模式

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

