**Go Advanced**

***

**流程控制**

1. if else **

   基本写法：---

   特殊写法：

    if 表达式之前添加一个执行语句，再根据变量值进行判断

   ```go
   if err := Connect(); err != nil {
     fmt.Println(err)
     return
   }
   // 执行Connect()后将值保存在err中，然后再判断
   ```

2. **for**

   无限循环

   ```go
   sum := 0
   for {
     sum++ 
     if sum > 100 {
       break
     }
   }
   
   // 循环中退出
   OuterLoop:
   for i := 0; i < 2; i++ {
     for j := 0; j < 5; j++ {
       if j == 2 {
         fmt.Println(i, j) // 0 2
         break OuterLoop
       }
       if j == 5 {
         fmt.Println(i,j)
         break OuterLoop // 不执行到此
       }
     }
   }
   // 初始语句
   step := 2
   for ; step > 0; step-- {
     fmt.Println(step)
   }
   fmt.Println(step) // 0
   
   // 初始语句；条件表达式；结束语句
   // 无限循环限制
   var i int
   for {
     if i > 10 {
       break
     }
     i++
   }
   fmt.Println(i) // 11
   
   for i <= 10 {
     i++
   }
   fmt.Println(i)
   // 在结束每次循环前执行的语句，如果循环被break,goto, return, panic等语句强制退出时，
   // 结束语句不会被执行
   ```

3. **for range**

   返回值：

   - 数组、切片、字符串返回索引和值（数组value实际类型是rune类型，十六进制字符编码）
   - map返回键和值
   - 通道只返回通道内的值

   ```go
   c := make(chan int)
   go func() {
     c <- 1
     c <- 2
     c <- 3
   }()
   for v := range c {
     fmt.Println(v)
   }
   ```

4. **switch**

   ```go
   var b = true
   switch b {
     case true : 
     fmt.Println("int")
     fallthrough // fallthrough关键字可以连接case, 否则不会像C紧接着执行下一个case，但是不建议用
     case 1 == 1:
     fmt.Println("fallthrough")
   }
   ```

5. **goto**

   ```go
   for x := 0; x < 10; x++ {
     if x == 2 {
       goto breakHere // ok
     }
     if x == 5 {
       goto breakHere // 不执行到这里
     }
   }
   // 手动返回，避免执行进入标签
   return
   breakHere:
   fmt.Println("ok")
   ```

6. **continue**

   ```go
   OuterLoop:
   for i := 0; i < 2; i++ {
     for j := 0; j < 5; j++ {
       switch j {
         case 2: 
         fmt.Println(i,j)
         continue OuterLoop // 结束当前循环，开启下一次外层循环
       }
     }
   }
   ```

***

**函数**

1. DRY原则（Don't Repeat Yourself)不要写重复代码

   不同于C，可有多个返回值，同一种类型返回值和命名返回值两种形式只能二选一，混用时将会发生编译错误

2. go中，函数也是一种类型

   ```go
   func write() {
     fmt.Println("write")
   }
   func main() {
     var f func()
     f = write
     f()
   }
   
   func namedRetValues() (a, b int, int) // 命名参数和非命名参数不能混合使用
   ```

3. **匿名函数**

   ```go
   func foo(a int) {
     fmt.Println(a)
   }(100)
   // 匿名函数用作回调函数
   func visit(list []int, f func(int)) {
     for _, v := range list {
       f(v)
     }
   }
   func main() {
     visit([]int{1,2,3,4}, func(v int)) {
       fmt.Println(v)
     }
   }
   ```

4. **函数类型实现接口**

***

**结构体**

***

**接口**

***

**包**

1. go使用包来组织源代码，Go语言没有强制要求包名必须和其所在的目录名同名，但是建议包名和所在目录名同名，包借助了目录树的组织方式。

2. 包的导入：包名是从`GOPATH/src/ `后开始计算的，使用`/ `进行路径分隔

   全路径导入：GOROOT/src后开始写

   包命名别名：`import F "fmt"`

   **省略引用格式：**`import . "Fmt"`引用时直接使用`Println()`

   **匿名引用：**只是希望执行包初始化的init 函数，而不使用包内部的数据时，可以使用`import _ "net/http"`

   **包加载：**在执行main包的main函数之前，go引导程序对整个程序的包进行初始化。从main函数引用的包开始，逐级查找包的引用，直到找到没有引用其他包的包，最终生成一个包引用的有向无环图。然后将图转化为一棵树，从树的叶子节点逐层向上对包进行初始化。单个包初始化时，先初始化常量，然后全局变量，最后执行init函数。

3. 



