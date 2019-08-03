# Interview

1. TCP/IP原理/socket通信/HTTP

   传输层协议，通过三次挥手建立连接，四次握手断开连接，TCP即传输控制协议，面向连接，传输慢开销大，但可靠

   UDP用户数据报协议，不可靠但高效，长连接，传输快，开销小

   HTTP 无状态协议，服务器不会再两个请求之间保留任何数据

   HTTPS：添加了SSL加密传输，建立了信息安全通道，确保了网站的真实性

2. 基础数据类型和引用数据类型的区别

   基础数据类型包括5中，Number，Boolean，Null，undefined, String

   引用数据类型包括Object，Array，Date，Function

   两者在访问机制上和复制变量时有区别，引用类型赋值时实际上改变的是堆内存，存储在变量处的值是一个指针，指向存储对象的内存地址

3. 如何创建HttPRequest请求

   包括判断状态码，是否跨域，是否携带cookie等

4. 如何操作浏览器cookie

5. 浏览器事件模型（冒泡、捕捉）

6. 行内元素和块级元素的区别

7. 盒子模型

8. relative和absolute的区别

   1. 两者都脱离正常文本流
   2. 在正常文本流中的位置：relative是依然存在的，而absolute是不存在的
   3. relative相对于最近的父元素
   4. absolute定位层相对于最近的定义为relative或absolute的父层，若没有父层则相对于body

9. 浮动样式

10. 样式覆盖优先级

11. localstorage和sessionStorage

12. 跨域问题

13. 弹性盒问题

14. JS原型链

15. Vue函数式组件

16. webpack构建项目代码

17. Vue组件生命周期过程

18. vue组件的监听属性和计算属性

19. watch的熟练使用

20. Vue插槽

21. Vue继承

22. 理解minxins和extend的区别

23. element-ui的源码

24. 敏捷开发

25. PLM/制造相关行业知识

26. 技术方案设计/项目交付流程