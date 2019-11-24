# Vue-Router

1. 子路由

   `path (name) component children`

2. 通过`URL`参数传递

   `path: '/home/:id'

3. 单页面多路由操作

   ```javascript
   routes: [
       {
         path: '/',
         name: 'HelloWorld',
         components: {
           default: HelloWorld,
           router1: Router1,
           router2: Router2,
         }
       },
       {
         path: '/oops',
         name: 'HelloWorld',
         components: {
           default: HelloWorld,
           router1: Router2, //两个router-view交换位置
           router2: Router1,
         }
       }
   ]
   ```

   

4. 参数传递

   `<router-link :to="{name: 'oops', params: {id: 20 }}"></router-link>`

5. `redirect`重定向

6. `alias`别名

7. 路由过渡动画

   `mode="out-in"`或`mode="in-out"`

   `fade-enter fade-enter-active fade-leave fade-leave-active`

   `transition: opacity .5s;` 

8. `mode='history'/mode='hash'`

   {path: '*', component: Error}

9. 钩子函数

   ```javascript
   beforeRouteEnter (to, from, next) {
       console.log("before route enter")
       next()
     },
     beforeRouteLeave (to, from, next) {
       console.log('before route leave')
       // next() // 一定要next()才会走下一步
     }
   ```

10. 编程式导航

    ```javascript
    this.$router.go(-1)
    this.$router.push({
        path: '/xxx'
    })
    ```

    