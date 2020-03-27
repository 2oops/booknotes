# Basic

**Go basic**

***

基础包括以下：

- 基本语法 =>类型 => 匿名变量 => 变量作用域 => 变量生命周期 => 指针 => 常量 => 类型别名
- 容器 => 数组 => 切片 => range

***

**简介**

2. 特性及优势：

   具有较高的生产效率、先进的依赖管理和类型系统，以及原生的并发计算支持。

3. 为并发而生

   从底层原生支持并发，go语言的并发基于goroutine，类似于线程，但并非线程，可以将其理解为一种虚拟线程。go语言运行时会参与调度goroutine，并将goroutine合理的分配到每个CPU中，最大限度的使用CPU性能。

   多个goroutine中，go使用通道进行通信，通道是一种内置的数据结构，可以让用户在不同的goroutine之间同步发送具有类型的消息。

4. 所有在go中的内存都是经过初始化的

***

**go语法**

1. 基本类型

   - bool

     优先级：&& > ||     * > +

     布尔型不可强制转为整型，也不可以转为其他类型

   - string

   - int int8 int16 int32 int64

     Int8(-128 - 127).  uint8(0 - 255)

     通常 int 类型的处理速度也是最快的

     `0x%x`十六进制，`%d`十进制，

   - uint8 16 32 64 uintptr

     uintptr无符号整数类型，没有指定具体的 bit 大小但是足以容纳指针，uintptr 类型只有在底层编程时才需要，特别是Go语言和C语言函数库或操作系统接口相交互的地方

     程序逻辑对整型范围没有特殊要求，但是**在二进制传输、读写文件的结构描述时，为了保持文件的结构不会受到不同编译目标平台字节长度的影响，不要使用 int 和 uint**

   - byte // uint8的别名

   - rune //int32的别名 代表一个unicode码

   - float32 float64

     `fmt.Printf("%f\n", math.Pi).  fmt.Printf("%.2f", math.Pi)`

   - complex64(32位实数和虚数)  complex128

     complex128 为复数的默认类型，`name := complex(x, y)` x为实部，y为虚部

     real(z)获取实部  imag(z) 获取虚部

2. 申明  标准格式  批量格式  简短格式

   `:=`声明时左值变量必须是没有定义的变量

   ```go
   var conn net.Conn
   var err error
   conn, err = net.Dial("tcp", "127.0.0.1:8080")
   // 在多个短变量声明和赋值中，至少有一个新声明的变量出现在左值中，即便其他变量名重复声明的，编译器也不会报错
   conn, err = net.Dial("tcp", "127.0.0.1:8080")
   conn2, err = net.Dial("tcp", "127.0.0.1:8080")
   ```

3. 匿名变量

   不占用内存空间，也不会分配内存，匿名变量与匿名变量之间也不会因为多次声明而无法使用。

   ```go
   func getData() (int, int) {
     return 1, 2
   }
   func main() {
     a, _ := getData()
     _, b := getData()
     fmt.Println(a, b) // 1,2
   }
   ```

4. 变量的作用域

   - 局部变量

     局部变量名和全局变量名可以相同，局部变量名优先级更高

   - 全局变量

     全局变量声明必须以var关键字开头，如果想要在外部包中使用全局变量的首字母必须大写

   - 形式参数

     只在函数调用时生效，调用结束时销毁，未被调用时，行参不占用实际的存储单元，也没有实际值

5. go语言指针

   go具有控制数据结构指针的能力，但是却不能进行指针运算。

   指针在golang中可以分为两个核心概念：

   - 类型指针：可以对这个指针类型的数据进行数据，也可以使用指针传递数据，而无需拷贝数据，但不能进行偏移和运算。
   - 切片，由指向起始元素的原始指针、元素数量和容量组成

   **对比C语言的指针，指针是其保持高性能的根本所在，在操作大数据块时方便快捷，故此是操作系统使用的是C及其指针特性编写。**

   使用new()关键字也可以创建指针，创建过程会分配内存，被创建的指针指向默认值。

6. go语言变量的生命周期

   堆：存放进程执行中被动态分配的内存段，大小不固定

   栈：也即堆栈，用来存放程序暂时创建的局部变量，如函数大括号内的变量。

7. go常量

   ```go
   const (
     a = .12 
     b = 123   ) // 声明多个常量
   ```

   iota常量生成器

   ```go
   type weekday int
   const (
   	sunday weekday = iota
     monday // 1
   )
   ```

   无类型常量

   ```go
   var x float32 = math.Pi
   // math.Pi 无类型的浮点数常量，可以直接用于任意需要浮点数或复数的地方
   ```

8. 类型别名

   ```go
   // 类型别名和类型定义
   type IntAlias = int // 给int取IntAlias的别名
   type newInt int // 声明newInt为int类型
   ```

   非本地类型不能定义方法

   在结构体成员嵌入时使用别名

***

**go语言容器**

1. 数组

   ```go
   var arr [3]int 
   // 数组的每个元素都会被初始化为改类型的零值
   var a [3]int = [3]int{1,2}
   a[2] // 0
   q := [...]int{1,2,3} // ...表示初始化时会根据初始值的个数确定数组的长度
   // 数组的长度是数组的一部分，必须是常量表达式，举例来说就是长度确定后不能再给它赋值
   ```

   比较两个数组是否相等，一定要长度、类型、所有元素都相等时才会相等

   数组遍历

   ```go
   var team [3]int = [3]int{1,2}
   team[2] = 3
   for k, v := range team {
     fmt.Println(k,v)
   }
   ```

   多维数组

   ```go
   var array [4][2]int
   // 声明并初始化索引为1，2的数组
   array = [4][2]int{1: {2,3}, 2: {3, 4}}
   // 按维度初始化数组中指定的元素
   array = [4][2]int{1: {0: 1}, 2: { 1: 4}}
   // 为元素赋值
   array[0][0] = 1
   ```

   只要类型一致，多维数组可以相互赋值

2. 切片

   切片默认指向一段连续内存区域，可以是数组，也可以是切片本身

   表示原有的切片：`a := []int{1,2,3}  fmt.Println(a[:]) // [1,2,3]`

   使用`a[0:0]` // []`可以清空数组

   声明新的切片

   ```go
   var stringList []string // 声明字符串切片 stringList == nil
   // 空切片
   var empty = []int // empty != nil
   ```

   切片是动态结构，只能与nil判定相等，不能互相判定相等，声明新的切片后，可以使用`append()`函数向切片中添加元素。

   **使用内建函数make()构造切片**

   提前分配空间，以降低多次分配空间造成的性能问题，使用该函数生成切片必定发生了内存分配操作，但给定开始和结束为止的切片只是将新的切片结构指向已经分配好的内存区域，设定开始和结束位置，并不会发生内存分配操作。

   ```go
   make ([]Type, size, cap)
   a = make([]int, 2, 10)
   ```

   **使用append()为切片添加元素**

   ```go
   var a []int
   a = append(a, 1, 2)
   a = append(a, []int{1,2,3}...) // 直接追加切片时需要解包
   a = append([]int{1,2,3}, a...) // 开开头添加切片
   // 链式操作
   a= append(a[:i], append([]int{1, 2, 3}, a[i:]...)...) // 在第i个位置插入切片
   ```

   为切片添加元素时，如果空间不足，切片会自动扩容，容量会按原有容量的2倍进行扩充。1，2，4，8，16……

   由于append()能返回新切片的特性，所以切片也支持链式操作。

   **切片复制**

   ```go
   slice1 := []int{1,2,3,4}
   slice2 := []int{5,4,3,2,1}
   copy(slice1, slice2)
   
   fmt.Println(slice1) // [5,4,3,2]
   ```

   **切片删除**

   golang没有提供特定的接口，只能从切片本身的特性来删除元素，分别可以从开头位置删除、中间位置、尾部位置

   ```go
   a := []int{1,2,3}
   a = a[N:] // 删除开头N个元素
   // 或者使用append()删除开头N个元素
   a = append(a[:0], append(a[:N])...)
   a = a[:copy(a, a[N:])] // 将N+之后的元素拷贝过来，之前的不要了
   // 删除中间N个元素
   a = append(a[:i], a[i+N:]...)
   a = a[:i+copy(a[i:], a[i+N:])]
   // 尾部删除
   a = a[:len(a) - N]
   // 删除指定元素
   str := []string{"a","b", "c", "d", "e"}
   index := 2
   str = append(str[:index], str[index + 1:]...)
   fmt.Println(str) // [a b d e]
   ```

   **多维切片**


   多维切片操作起来会涉及众多的布局和值，相比于在函数间传递数据结构的复杂，切片由于本身结构简单的优势，在函数间传递消耗的成本很小。

3. **range关键字**

   ```go
   // range
   slice := []int{1,2,3,4}
   for index, value := range slice {
     fmt.Println(index, value, &value, &slice[index])
   }
   // range返回的value是一个副本，而不是对应元素的引用，所以可以看见value的地址和元素地址是不一样的
   // 且value地址总是一样的，因为其只是在迭代过程中不断赋值
   // 如果不需要index，可以使用_匿名变量忽略
   for index := 2; index < len(slice); index++ {
     fmt.Println(slice[index])
   }
   ```

4. **map**

   在其他语言中又称为字典、hash等

   `var mapname map[keyType]valueType`

   map是动态增长的，未初始化的值默认为nil，len()可以获取map中pair的对数

   ```go
   var mapLit map[string]int
   mapLit = map[string]int{"name": 1, "value":2 }
   fmt.Println(mapLit["name"]) // 1
   fmt.Println(mapLit["one"]) // 0 使用未声明的key值默认为零值
   ```

   **map容量**

   `mapLit := make(map[string]int, 100)`

   **用切片作为map值**

   ```go
   mapSlice := make(map[string][]int)
   mapslice2 := make(map[string]*[]int)
   ```

   **map遍历**

   ```go
   scene := make(map[string]int)
   for k, v := range scene {
     fmt.Println(k, v)
   }
   for k := range scene {
     // 只遍历key值
   }
   // 如果想要特定的顺序的遍历结果，需要先排序
   // 声明一个切片
   var sceneList []string
   for k := range scene {
     sceneList = append(sceneList, k)
   }
   sort.Strings(sceneList)
   ```

   **map元素的删除和清空**

   go并没有提供清空元素的方法，清空map元素最直接的方法就是新make一个map，因为go的并行垃圾回收效率远比清空函数高效。

   **go语言的同步map sync**

   go中，map在并发情况下，只读是线程安全的，同时读写是线程不安全的

   ```go
   // 举例来说
   m := make(map[int]int) // 创建一个int到int的映射
   // 开启一段并发代码，不停写入
   go func() {
     for {
       m[1] = 1
     }
   }
   // 继续并发，不停的读取map
   go func() {
     for {
       _ = m[1]
     }
   }
   // 无限循环，让并发不停地执行
   for {}
   // 这里会报错 fatal error: concurrent map read and map write
   // 不停的同时读和写会引起竞态问题，map内部会对这种并发操作检查和提前发现
   ```

   需要并发读写时，一般的做法是加锁，但是这种效率不高，go在1.9版本后提供了一种效率较高的并发安全的sync。Map，其并不是以语言原生形态提供，而是在sync包下的特殊结构。

   其有以下特性：

   - 不需要初始化，直接声明即可
   - 不能使用map的方式进行取值和设置等操作，而是是使用 sync.Map 的方法进行调用，Store 表示存储，Load 表示获取，Delete 表示删除
   - 使用range配合一个回调函数进行遍历操作，通过回调函数返回内部遍历的值，Range 参数中回调函数的返回值在需要继续迭代遍历时，返回 true，终止迭代遍历时，返回 false。

   ```go
   import (
   	"fmt"
     "sync"
   )
   func main() {
     var scene sync.Map
     // 保存键值对
     scene.Store("green", 90)
     scene.Store("age", 20)
     // 删除
     scene.Delete("age")
     // 遍历所有sync.Map的键值对
     // Range() 方法可以遍历 sync.Map，遍历需要提供一个匿名函数，参数为 k、v，类型为 interface{}，
     //每次 Range() 在遍历一个元素时，都会调用这个匿名函数把结果返回
     scene.Range(func(k, v interface {}) bool ) {
       fmt.Println("iterate: ", k, v)
       return true
     }
   }
   ```

   sync.Map并没有提供获取map数量的方法，需要在其遍历时自行计算，同时在保证并发安全的情况下有一些性能损失，所以在非并发情况下，建议使用map。

5. List(列表)

   列表是一种非连续的存储容器，由多个节点组成，节点间通过一些变量记录彼此之间的关系，实现方法包括单链表、双链表等。列表使用`container/list`包实现，内部实现原理是双链表，能高效的进行任意位置的插入和删除操作。

   ```go
   // 初始化
   var myList list.List
   myList := list.New()
   ```

   列表的元素可以是任意类型，这既带来了便利，同时也会带来问题，比如放入了接口类型的值，取出值后，如果要进行类型转换会发生宕机。

   **列表插入元素**`PushFront  PushBack`

   ```go
   // 列表插入方法
   InsertAfter(v interface {}, mark * Element) * Element
   InsertBefore(v interface {}, mark * Element) *Element
   PushBackList(other *List)
   PushFrontList(other *List)
   ```

   **删除元素**`Remove(element)`

   **遍历元素**

   ```go
   myList := list.New()
   myList.PushFront("2oops")
   myList.PushBack(20)
   // 保存一个句柄
   element := myList.PushBack("center")
   // 插入
   myList.InsertBefore("left", element)
   myList.InsertAfter("right", element)
   myList.Remove(element)
   
   for i := myList.Front(); i != nil; i = i.Next() {
     fmt.Println(i.Value) // 2oops 20 left center right
   }
   ```

6. **nil**

   零值：

   - bool - false， number - 0, string - ""

   - 指针、切片、map、通道、函数、接口 - nil

   nil和null有许多不同点：

   - nil 不可比较， 但在Python中，None == None

   - nil不是关键字或者保留字，也就是说变量名可以叫nil

   - nil没有默认类型，也就是说 `fmt.Printf("%T", nil)`这种写法是错的

   - 不同类型nil的指针是一样的

     ```go
     var arr []int
     var num *int
     fmt.Printf("%p\n", arr)
     fmt.Printf("%p\n", num) // 0x0
     ```

   - 不同类型的nil不能比较

   - 相同类型的nil值可能也无法比较

   - 不同类型的nil占用内存的大小可能不一样

     ```go
     var m map[int]bool
     fmt.Println( unsafe.Sizeof( m ) ) // 8
     var c chan string
     fmt.Println( unsafe.Sizeof( c ) ) // 8
     ```

     
