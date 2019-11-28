# Vue-Component

1. 组件类别：
   - `Vue-Router`引用的每个页面，可看做一个大的组件，不会被复用，也不会对外暴露接口
   - 基础组件：不包含业务，独立和具体功能，如`UI`组件库，侧重`API`的设计、性能、兼容性等
   - 业务组件：业务中多个页面可复用，须考虑可复用性和可维护性

2. `prop & event & slot`

   组件通信：

   - `ref `及`this.$refs`

   - `$parent`及`$children`
   - `provide/inject`(主要为高阶组件或组件库提供用例)

   ```javascript
   const s = Symbol()
   
   var Provider = {
   	provide() {
           return {
           	[s]: 'foo'
           }
       }
   }
   
   var Child = {
       inject: { s }
   }
   ```

   ```javascript
   // A.vue
   export default {
       provide: {
           name: '2oops' // 提供给所有的子组件
       }
   }
   // provide和inject的绑定不是可响应的，就是说如果A的name值改变了，B的name值还是不会变,
   // 若传入一个可监听的对象，那么可响应
   // B.vue
   export default {
       inject: ['name'],
       mounted() {
           console.log(this.name) // 2oops
       }
   }
   ```

   ```javascript
   // app.vue中使用provide 替代vuex
   // 如获取到用户信息后可以在此注入，然后任何页面或组件只要inject注入app即可访问到数据
   // App.vue
   export default {
       provide() {
           return {
               app: this
           }
       },
       data() {
           return {
               userInfo: null
           }
       },
       mounted() {
           this.getUserInfo()
       },
       methods: {
           getUserInfo() {
           	//请求
               this.userInfo = data
           }
       }
   }
   
   // C.vue
   <template>
     <div>
       {{ app.userInfo }}
     </div>
   </template>
   <script>
     export default {
       inject: ['app'],
       // 用户信息更改
           methods: {
               //修改完用户数据后通知App.vue
               changeUserInfo() {
                   axios.get('/usr').then(() => {
                       this.app.getUserInfo()
                   })
               }
           }
     }
   </script>
   ```

   ```javascript
   // 当app.vue文件东西太多的时候可以使用mixins混入不同的Js逻辑文件
   // user.js
   export default {
       data() {
           return {
               userInfo: null
           }
       },
       mounted() {
           this.getUserInfo()
       },
       methods: {
           getUserInfo() {
               //请求
               this.userInfo = data
           }
       }
   }
   
   // App.vue
   <script>
   import mixins_user from './mixins/user.js'
   export default {
       mixins: [mixins_user],
       data() {}
   }
   </script>
   ```


3. 



