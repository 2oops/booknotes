# Advanced

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

   函数体实现接口：

   结构体实现接口：

5. 闭包

   简单来说：函数 + 引用环境= 闭包，同一函数在不同的引用环境中组合，可以形成不同的实例

   golang的闭包是指引用了自由变量的函数，被引用的自由变量和函数一同存在，即使已经离开了自由变量的环境也不会被释放。

   函数是编译期静态的概念，闭包是运行期动态的概念，一般来说，函数是不具备“记忆性”的，但是在运行时与引用环境结合形成的闭包即具有的“记忆性”

   闭包对环境中变量的引用也叫做“捕获”，C++ 中有两种捕获，分别为复制捕获和引用捕获

   ```go
   // 闭包实现一个玩家生成器
   func playerGen(name string) func() (string, int) {
     hp := 200
     return func() (string, int) {
       return name, hp
     }
   }
   func main() {
     generator := playerGen("2oops")
     name, hp := generator()
     fmt.Println(name, hp) // '2oops 200'
   }
   ```

6. 可变参数

   ```go
   import (
   	"bytes"
   )
   func myfunc(args ...int) { // args为数组
     
   }
   myfunc(1,2,3,4)
   func myfunc2(args []int) {
     
   }
   myfunc2([]int{1,2,3})
   func myfunc3(args ...interface{}) { // 数组参数为任意类型
     
   }
   func joinString(list ...string) string {
     var b bytes.Buffer // 定义一个字节缓冲, 快速地连接字符串
     for _, str := range list {
       //str.(type))
       b.WriteString(str)
     }
     return b.String()
   }
   func main() {
     fmt.Println("aa","bb", "cc")
   }
   ```
   
   可变参数使用`...`进行传递与切片间使用 append 连接是同一个特性

7. **defer**

   

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

3. 使用 export 指令可以将当前目录的值设置到环境变量 GOPATH中。

   export GOPATH=`pwd`

   建立gopath中的源码目录：**mkdir -p src/hello**

   编译和运行：`go install hello`

   bin目录下执行./hello

   GOPATH路径建议随项目路径设置，避免不可预知的错误

4. 功能常用内置包

   - fmt io
   - bufio 对io包的封装，并提供了缓冲数据功能
   - sort 
   - strconv 字符串和基本数据类型之间转换
   - sync 实现中锁机制和其他同步互斥机制
   - flag 命令行参数规则定义和解析
   - encoding/json 对象《=》JSON字符串间转换
   - html/template 主要实现了 web 开发中生成 html 的 template 的一些函数
   - net/http
   - ox/exec linux命令相关实现
   - reflect
   - bytes  log

5. 自定义包

   Go 编译速度快受益于以下三点：

   - 所有导入的包必须在每个文件的开头显示的声明
   - 禁止包的环状依赖，包的依赖关系会形成一个有向无环图，每个包可以独立编译，且很可能会并发编译
   - 编译后的文件不仅记录了本身的导出信息，目标文件还记录了包的依赖关系。因此在编译一个包时，不需要遍历所有引入的包，只需要读取直接导入的目标文件即可。

6. 包中的标识符，让外部的包可以访问包的类型和值

   ```go
   type MyStruct struct {
     OutPackage int // 包外可用
     innerPack string // 包内访问
   }
   type MyInterface interface {
     ExportedMethod() // 包外
     innerMethod()// 包内
   }
   ```

7. import导入包

   - 导入后自定义包名
   - 只希望导入包，而不使用任何包内的结构和类型，也不调用包内的任何函数时，可以使用匿名导入包
   - 包在程序启动前的初始化入口：init()会在main函数执行前执行，被最后导入的包会最先初始化并调用 init() 函数。

8. 包与锁：限制线程对变量的访问

   在处理并发过程中可能出现两个或多个协程读或写同一个变量的情况，因此sync包提供了互斥锁Mutex和读写锁RWMutex解决这个问题。

   锁是sync包的核心，主要有两个方法，分别是加锁Lock和解锁Unlock

   在并发情况下，使用锁可以保证在某一时间内，只有一个线程操作修改这一变量

   **互斥锁**

   一个互斥锁只能同时被一个协程锁定，其他协程将阻塞到互斥锁被解锁

   ```go
   func main() {
     ch := make(chan, struct{}, 2)
     var lock sync.Mutex
     go func() {
       lock.Lock()
       defer lock.Unlock()
       fmt.Println("协程1: 我会锁定两秒")
       time.Sleep(time.Second * 2)
       fmt.Println("协程1: 我解锁了，你们去抢锁吧")
       ch <- struct{}{}
     }()
     go func() {
       fmt.Println("协程2: 等待解锁中")
       lock.Lock()
       defer lock.Unlock()
       fmt.Println("协程2: 我也解锁了")
       ch <- struct{}{}
     }()
     for i := 0; i < 2; i++ {
       <- ch
     }
   }
   ```

   **读写锁**

   有以下特点：

   - 一个协程在写，其他读或写锁定都要等它写完
   - 一个协程在读，其他读锁定可以继续
   - 一个或多个在读时，其他的写锁定要等这些读锁定读完才能开始写锁定

   即同时只能有一个协程可以写锁定，同时可以有任意个读锁定，读和写互斥，只能同时存在读锁定或写锁定

9. **big包**

   math/big 包实现了大数字（超过int64 uint64且精度要求高时）的多精度计算，支持Int, Rat（有理数）,Float等

   `big1 := new(big.Int).SetUint64(uint64(1000))`

   因为go不支持运算符重载，所以所有大数字类型都有Add()和Mul()等这样的方法

10. 正则表达式

    ```go
    buf := "abc a8b foa a99c"
    reg1 := regexp.MustCompile(`a\dc`)
    if reg1 == nil {
      fmt.Println("regexp error")
      return
    }
    result := reg1.FindAllStringSubmatch(buf, -1)
    ```

11. **time包**

    - time.UTC  time.Local

    - time.Now()

      ```go
      now := time.Now()
      year := now.Year()
      second := now.Second()
      timestamp1 := now.Unix() //Unix 时间戳
      timestamp2 := now.UnixNano() // 纳秒时间戳
      weekday := now.Weekday().String() // "Tuesday"
      afterOneHour := now.Add(time.Hour) // 当前时间往后推一小时
      ```

    - 时间操作函数

      Sub 求两个时间的差值 `func (t Time) Sub(u Time) Duration`

      Equal  比较两个时间是否相等，会考虑时区因素

      Before 判断一个时间点是否在另一个时间点之前 `func (t Time) Before(u Time) bool`

      After

    - 定时器 time.Tick()，定时器的本质是一个通道

    - 时间格式化，不是使用YYYY-MM-dd这一套，而是使用go的诞生时间**2006 年 1 月 2 号 15 点 04 分 05 秒**

      ```go
      now := time.Now()
      // 24小时制格式化
      now.Format("2006-01-02 15:04:05.000 Mon Jan")
      now.Format("2006-01-02 03:04:05.000 PM Mon Jan") // 12小时制
      ```

    - 解析字符串格式的时间

      ```go
      var layout string = "2006-01-02 15:04:05"
      var timeStr string = "2019-12-12 15:22:12"
      timeObj1, _ := time.Parse(layout, timeStr)
      fmt.Println(timeObj1)
      timeObj2, _ := time.ParseInLocation(layout, timeStr, time.Local)
      fmt.Println(timeObj2)
      ```

    11. **os包**

    12. **flag包：**命令行参数解析

        flag.Type()

        flag.TypeVar()

        flag.Parse()

    13. **mod包管理工具使用**

        - GO111MODULE=on 启用 go module，编译时会忽略 GOPATH 和 vendor 文件夹，只根据 go.mod下载依赖
        - off则是从GOPATH和vendor文件夹下载
        - auto，默认值，当项目不在GOPATH/src目录下，且根目录有go.mod文件时，会开启go modules

        `export GO111MODULE=on`

        常用go mod命令

        | 命令            | 作用                                       |
        | --------------- | ------------------------------------------ |
        | go mod download | 将包下载到本地，默认为GOPATH/pkg/mod目录下 |
        | go mod edit     |                                            |
        | go mod graph    |                                            |
        | go mod init     |                                            |
        | go mod tidy     | 增加缺少包，删除无用包                     |
        | go mod vendor   | 将依赖包复制到vendor目录下                 |
        | go mod verify   | 校验依赖                                   |
        | go mod why      | 解释为什么需要依赖                         |

        **创建一个新项目**

        `go mod init go-project1`

        go 会自动生成一个 go.sum 文件来记录 dependency tree，

        go module 安装 package 的原则是先拉取最新的 release tag，若无 tag 则拉取最新的 commit

        `go list -m -u all`来检查可以升级的 package，使用`go get -u`升级所有依赖

        使用replace替换无法直接获取的包

        在 go.mod 文件中使用 replace 指令替换成 github 上对应的库

        `replace (
          golang.org/x/crypto v0.0.0-20190313024323-a1f597ede03a => github.com/golang/crypto v0.0.0-20190313024323-a1f597ede03a
        )`



