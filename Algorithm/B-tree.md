# Algorithm

1. 排序二叉树

   应用：百度搜索

   ```html
   // 二叉树的构建，遍历，查找节点，删除等操作
   <html>
     <head>
   
     </head>
     <body>
     <script>
     function BinaryTree() {
     let Node = function(key) {
       this.key = key;
       this. left = null;
       this.right = null;
     }
   
     let root = null;
   
     let insertNode = function(node, newNode) {
       if(newNode.key < node.key) {
         if(node.left === null) {
           node.left = newNode;
         }else {
           insertNode(node.left, newNode)
         }
       }else {
         if(node.right === null) {
           node.right = newNode;
         }else {
           insertNode(node.right, newNode);
         }
       }
     }
   
     this.insert = function(key) {
       let newNode = new Node(key);
       if(root === null) {
         root = newNode;
       }else {
         insertNode(root, newNode)
       }
     }
   
     //中序遍历
     let inOrderTraversNode = function(node, callback) {
       if(node !== null) {
         // 先序，中序，后序只需调整以下三者的位置
         inOrderTraversNode(node.left, callback);
         inOrderTraversNode(node.right, callback);
         callback(node.key);
       }
     }
   
     this.inOrderTravers = function(callback) {
       inOrderTraversNode(root, callback);
     }
   
     // 查找最小节点
     let minNode = function(node) {
       if(node) {
         while(node && node.left !== null) {
           node = node.left;
         }
         return node.key;
       }
       return null;
     }
   
     this.min = function() {
       return minNode(root)
     }
   
     // 搜索节点
     let searchNode = function(node, key) {
       if(node === null) {
         return false;
       }
       if(key < node.key) {
         return searchNode(node.left, key)
       }else if(key > node.key) {
         return searchNode(node.right, key)
       }else {
         return true;
       }
     }
   
     this.search = function(key) {
       return searchNode(root, key);
     }
   
     // 删除节点，分三种情况，删除节点没有左右子树，有左子树或右子树，有左右子树。
     let findMinNode = function(node) { // 删除节点有左右子树时，查找最小节点
       if(node) {
         while(node && node.left !== null) {
           node = node.left;
         }
         return node;//返回节点，而不是返回key
       }
       return null;
     }
   
     let removeNode = function(node, key) {
       if(node === null) {
         return null;
       }
       if(key < node.key) {
         node.left = removeNode(node.left, key);
         return node;
       }else if(key > node.key) {
         node.right = removeNode(node.right, key);
         return node;
       }else {
         if(node.left === null && node.right === null) {
           node = null;
           return node;
         }
         if(node.left === null) {
           node = node.right;
           return node;
         }else if(node.right === null) {
           node = node.left;
           return node;
         }
   
         let aux = findMinNode(node.right);
         node.key = aux.key;// 用root右子树最小节点替换root节点
         node.right = removeNode(node.right, aux.key); // root右子树指向已删除最小节点的新的子树root节点
         return node;
       }
     }
       
     this.remove = function(key) {
       root = removeNode(root, key);
     }
   }
   
   let nodes = [8,3,10,1,6,14,4,7,13];
   let binaryTree = new BinaryTree();
   nodes.forEach((key) => {
     binaryTree.insert(key);
   })
   
   let callback = function(key) {
     console.log(key)
   }
   binaryTree.inOrderTravers(callback);
   
   console.log(`min mode is: ${binaryTree.min()}`)
   console.log(binaryTree.search(14));//true
   console.log(binaryTree.search(2));//false
   binaryTree.remove(3)
   </script>
     </body>
   </html>
   ```


2. 冒泡排序

   ```javascript
   function bubbleSort(arr) {
     let len = arr.length;
     for (let i = 0; i < len; i++) {
       for (let j = 0; j < len-1-i; j++) {
         if(arr[j] > arr[j+1]) {
           let temp = arr[j+1];
           arr[j+1] = arr[j];
           arr[j] = temp;
         }
       }
     }
     return arr;
   }
   bubbleSort([1,2,4,2,3,1])
   ```

3. 快速排序

***

#### 数据结构

1. 栈

   每一种数据结构都可以有很多种方式来实现，如下用数组实现栈

   ```javascript
   class Stack {
       constructor() {
           this.stack = []
       }
       push(item) {
         	this.stack.push(item)      
       }
       pop() {
   		this.stack.pop()
       }
       peek() {
          	return this.stack[this.getCount() - 1]     
       }
   	getCount() {
   		return this.stack.length            
       }
   	isEmpty() {
   		return this.stack.length === 0
       }
   }
   ```

2. 队列

   单链队列：如下可知，出队的时间复杂度为O(n)，而循环队列的时间复杂度为O(1)

   ```javascript
   class Queue {
       constructor() {
           this.queue = []
       }
       enQueue(item) {
           this.queue.push(item)
       }
       outQueue() {
           this.queue.shift() //把数组的第一个元素删除并返回该值
       }
       getHeader() {
           return this.queue[0]
       }
       getLength() {
           return this.queue.length
       }
       isEmpty() {
           return this.getLength() === 0
       }
   }
   ```

   循环队列

   ```javascript
   class SqQueue {
       constructor(length) {
           this.queue = new Array(length + 1)
           this.first = 0 //头
           this.last = 0 // 尾
           this.size = 0 // 大小
       }
       isEmpty() {
           return this.first === this.last
       }
       resize(length) {
           let q = new Array(length)
           for(let i = 0; i < length; i++) {
               q[i] = this.queue[(i + this.first) % this.queue.length]
           }
           this.queue = 0
           this.first = 0
           this.last = this.size
       }
       getLength() {
           return this.queue.length - 1
       }
       getHeader() {
           if(this.isEmpty()) {
               throw Error("Empty")
           }
           return this.queue[this.first]
       }
       enQueue(item) {
           //判断队尾+1是否为队头，后面取余是为了防止数组越界
           if(this.first === (this.last + 1) % this.queue.length) {
               this.resize(this.getLength() * 2 - 1)
           }
           this.queue[this.last] = item
           this.size ++
           this.last = (this.last + 1) % this.queue.length
       }
       outQueue() {
           // 判空
           let r = this.queue[this.first]
           this.queue[this.first] = null
           this.first = (this.first + 1) % this.queue.length
           this.size --
           // 判断当前队列大小是否过小
           // 为了保证不浪费空间，在队列空间等于总长度四分之一时
           // 且不为 2 时缩小总长度为当前的一半
           if (this.size === this.getLength() / 4 && this.getLength() / 2 !== 0) {
             this.resize(this.getLength() / 2)
           }
           return r
       }
   }
   ```

3. 单向链表

   ```javascript
   class Node {
       constructor(v, next) {
           this.value = v
           this.next = next
       }
   }
   class LinkList {
       constructor() {
           this.size = 0
           this.VitualHeader = new Node(null, null) //虚拟头部
       }
       find(header, index, currentIndex) {
           if(index === currentIndex) return header
           return this.find(header.next, index, currentIndex + 1)
       }
       addNode(v, index) {
           this.checkIndex(index)
           // 当往链表末尾插入时，prev.next 为空
           // 其他情况时，因为要插入节点，所以插入的节点
           // 的 next 应该是 prev.next
           // 然后设置 prev.next 为插入的节点
           let prev = this.find(this.dummyNode, index, 0)
           prev.next = new Node(v, prev.next)
           this.size++
           return prev.next
     	}
     	insertNode(v, index) {
     	  	return this.addNode(v, index)
     	}
    	addToFirst(v) {
     	 	return this.addNode(v, 0)
     	}
     	addToLast(v) {
       	return this.addNode(v, this.size)
     	}
     	removeNode(index, isLast) {
       	this.checkIndex(index)
       	index = isLast ? index - 1 : index
       	let prev = this.find(this.dummyNode, index, 0)
       	let node = prev.next
       	prev.next = node.next
       	node.next = null
       	this.size--
       	return node
     	}
     	removeFirstNode() {
       	return this.removeNode(0)
     	}
     	removeLastNode() {
       	return this.removeNode(this.size, true)
     	}
     	checkIndex(index) {
       	if (index < 0 || index > this.size) throw Error('Index error')
     	}
     	getNode(index) {
       	this.checkIndex(index)
       	if (this.isEmpty()) return
       	return this.find(this.dummyNode, index, 0).next
     	}
     	isEmpty() {
       	return this.size === 0
     	}
     	getSize() {
       	return this.size
     	}
   }
   ```

4. `AVL`树

   我们知道，二叉搜索树并不是严格的O(logN)，在业务中受限制，AVL树改进了二分搜索树，`AVL`树中任意节点的左右子树高度差都不大于1，保证了时间复杂度是严格的O(logN)，基于此，对 AVL 树增加或删除节点时可能需要旋转树来达到高度的平衡。

5. Trie

   又称字典树，是一种有序树，用于保存关联数组，其中的键通常是字符串。

   ![img](https://user-gold-cdn.xitu.io/2018/6/9/163e1d2f6cec3348?imageslim)

   其有以下特点：

   - 根节点代表空字符串，每个节点都有 N（假如搜索英文字符，就有 26 条） 条链接，每条链接代表一个字符
   - 节点不存储字符，只有路径才存储，这点和其他的树结构不同
   - 从根节点开始到任意一个节点，将沿途经过的字符连接起来就是该节点对应的字符串

6. 堆
7. 图

