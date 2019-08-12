# TypeScript

1. 基础类型，添加了枚举类型(`enum`)

   数组：`let list: number[] = [1, 2, 3];`

   使用数组泛型：`let list:Array<number> = [1,2,3]`

   元组tuple:`let x = [number, string]; x = ['aaa', 10]`,当访问一个已知索引的元素，会得到正确的类型：`x[0].substr(1)`,当访问一个越界的元素，会使用联合类型替代：

2. 添加了可选静态类型、类和模块；并且支持所有的浏览器、主机和操作系统；编译时检查，不污染运行时

   ```typescript
   class Student {
     fullName: string;
     constructor(public firstName: string, public middleInitial: string, public lastName: string) {
       this.fullName = firstName + " " + middleInitial + " " + lastName;
     }
   }
   
   interface Person {
     firstName: string;
     lastName: string;
   }
   
   function greeter(person: Person) {
     return "Hello, " + person.firstName + " " + person.lastName;
   }
   
   let user = new Student("Jane", "M.", "User");
   
   document.body.textContent = greeter(user);
   ```

3. 安装及使用

   - 安装`yarn`，访问地址：`yarnpkg.com`
   - `npm i -g typescript`
   - `npm i -g ts-node` // 安装Typescript的运行时
   - 然后就可以在Vscode(Ctrl + J 打开终端)中使用`ts-node test.ts`运行Typescript文件