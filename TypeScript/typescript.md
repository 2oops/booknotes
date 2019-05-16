# TypeScript

1. 基础类型，添加了枚举类型(`enum`)

   数组：`let list: number[] = [1, 2, 3];`

   使用数组泛型：`let list:Array<number> = [1,2,3]`

   元组tuple:`let x = [number, string]; x = ['aaa', 10]`,当访问一个已知索引的元素，会得到正确的类型：`x[0].substr(1)`,当访问一个越界的元素，会使用联合类型替代：

