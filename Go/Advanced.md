**Go Advanced**

**流程控制**

***

1. **if else **

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
         fmt.Println(i, j) 
         break OuterLoop
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



