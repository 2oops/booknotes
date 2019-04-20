# Javascript

排序二叉树

应用：百度搜索等

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



