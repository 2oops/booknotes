**More about Golang**

1. **time.Sleep  time.Tick()  time.newTicker()**的对比

   - Sleep和Tick都都会被放在同一个协程中统一处理
   - 但实际上，Sleep是通过睡眠完成定时任务，需要被调度唤醒，而Tick则是使用channel阻塞当前协程完成定时任务
   - 使用channel阻塞协程完成定时任务比较灵活，可以结合select设置超时时间以及默认执行方法，且可以设置timer的主动关闭，不需要每次都生成一个timer

   综上，建议使用Tick完成定时任务

2. 

