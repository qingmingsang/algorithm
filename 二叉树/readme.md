# 二叉树以及二叉搜索树

@1

树相关的概念：
- 子树：由节点和他的后代构成，如上图标示处。
- 深度：节点的深度取决于它祖节点数量，比如节点5有2个祖节点，他的深度为2。
- 高度：树的高度取决于所有节点深度的最大值。

## 二叉树
二叉树中的节点最多只能有2个子节点，一个是左侧子节点，一个是右侧子节点，这样定义的好处是有利于写出更高效的插入，查找，删除节点的算法。
@3

- 根节点：二叉树最顶层的节点
- 分支节点：除了根节点以外且拥有叶子节点
- 叶子节点：除了自身，没有其他子节点

在二叉树中，我们常常还会用父节点和子节点来描述，比如图中2为6和3的父节点，反之6和3是2子节点

二叉树的三个性质:
- 在二叉树的第i层上，至多有`2^i-1`个节点
    - `i=1`时，只有一个根节点，`2^(i-1) = 2^0 = 1`
- 深度为k的二叉树至多有2^k-1个节点
    - `i=2`时，`2^k-1 = 2^2 - 1 = 3`个节点
- 对任何一棵二叉树T，如果总结点数为n0，度为2(子树数目为2)的节点数为n2,则`n0=n2+1`

树和二叉树的三个主要差别
- 树的节点个数至少为1，而二叉树的节点个数可以为0
- 树中节点的最大度数(节点数量)没有限制,而二叉树的节点的最大度数为2
- 树的节点没有左右之分，而二叉树的节点有左右之分

## 二叉搜索树
二叉搜索树是二叉树的一种，它只允许你在左侧子节点存储比父节点小的值，但在右侧节点存储比父节点大的值。

@2


## 二叉搜索树实现
创建BinarySearchTree类
```
function BinarySearchTree(){
    // 用于创建节点的类
    let Node = function(key) {
        this.key = key;
        this.left = null;
        this.right = null;
    }
    // 根节点
    let root = null;
}
```
插入一个键
```
    this.insert = function(key) {
        let newNode = new Node(key);
        root === null ? (root = newNode) : (insertNode(root, newNode))
    }
```
向树中插入一个新的节点主要有以下三部分：
1.创建新节点的Node类实例 --> 2.判断插入操作是否为根节点,是根节点就将其指向根节点 --> 3.将节点加入非根节点的其他位置。
```
function insertNode(node, newNode){
    if(newNode.key < node.key) {
        node.left === null ? (node.left = newNode) : (insertNode(node.left, newNode))
    }else {
        node.right === null ? (node.right = newNode) : (insertNode(node.right, newNode))
    }
}

let tree = new BinarySearchTree();
tree.insert(20);
tree.insert(21);
tree.insert(520);
tree.insert(521);
```
插入的结构会按照二叉搜索树的规则去插入，结构类似于上文的第一个树图。

## 树的遍历
访问树的所有节点有三种遍历方式：中序，先序和后序。
- 中序遍历：以从最小到最大的顺序访问所有节点
- 先序遍历：以优先于后代节点的顺序访问每个节点
- 后序遍历：先访问节点的后代节点再访问节点本身

### 中序遍历
使用中序遍历可以实现对树进行从小到大排序的功能。
```
this.inOrderTraverse = function(cb){
    inOrderTraverseNode(root, cb);
}

// 辅助函数
function inOrderTraverseNode(node, cb){
    if(node !== null){
        inOrderTraverseNode(node.left, cb);
        cb(node.key);
        inOrderTraverseNode(node.right, cb);
    }
}
```

### 先序遍历
使用先序遍历可以实现结构化输出的功能。
```
// 先序遍历 --- 优先于后代节点的顺序访问每个节点
   this.preOrderTraverse = function(cb) {
     preOrderTraverseNode(root, cb);
   }

   // 先序遍历辅助方法
   function preOrderTraverseNode(node, cb) {
     if(node !== null) {
       cb(node.key);
       preOrderTraverseNode(node.left, cb);
       preOrderTraverseNode(node.right, cb);
     }
   }
```

### 后序遍历
后序遍历可以用于计算有层级关系的所有元素的大小。
```
// 后续遍历 --- 先访问后代节点，再访问节点本身
   this.postOrderTraverse = function(cb) {
     postOrderTraverseNode(root, cb);
   }

   // 后续遍历辅助方法
   function postOrderTraverseNode(node, cb) {
     if(node !== null){
       postOrderTraverseNode(node.left, cb);
       postOrderTraverseNode(node.right, cb);
       cb(node.key);
     }
   }
```

## 搜索树中的值

### 最小值
最小值通过定义可以知道即是左侧树的最底端的节点
```
// 最小值
   this.min = function(){
     return minNode(root)
   }

    function minNode(node) {
     if(node) {
       while(node && node.left !== null){
         node = node.left;
       }
       return node.key
     }
     return null
   }
```

###  最大值
```
// 最大值
   this.max = function() {
     return maxNode(root)
   }

   function maxNode(node) {
     if(node){
       while(node && node.right !== null){
         node = node.right;
       }
       return node.key
     }
     return null
   }
```

### 搜索一个特定的值
```
// 搜索树中某个值
this.search = function(key) {
    return searchNode(root, key)
}

// 搜索辅助方法
function searchNode(node, key){
    if(node === null) {
        return false
    }
    if(key < node.key) {
        return searchNode(node.left, key)
    } else if(key > node.key) {
        return searchNode(node.right, key)
    }else {
        return true
    }
}
```

### 移除一个节点
```
this.remove = function(key){
    root = removeNode(root, key);
}

// 发现最小节点
function findMinNode(node) {
    if(node) {
    while(node && node.left !== null){
        node = node.left;
    }
    return node
    }
    return null
}

// 移除节点辅助方法
function removeNode(node, key) {
    if(node === null) {
    return null
    }

    if(key < node.key){
    node.left = removeNode(node.left, key);
    return node
    } else if( key > node.key){
    node.right = removeNode(node.right, key);
    return node
    } else {
    // 一个页节点
    if(node.left === null && node.right === null) {
        node = null;
        return node
    }

    // 只有一个子节点的节点
    if(node.left === null) {
        node = node.right;
        return node
    }else if(node.right === null) {
        node = node.left;
        return node
    }

    // 有两个子节点的节点
    let aux = findMinNode(node.right);
    node.key = aux.key;
    node.right = removeNode(node.right, aux.key);
    return node
    }
}
```
删除节点需要考虑的情况比较多，这里我们会使用和min类似的实现去写一个发现最小节点的函数，当要删除的节点有两个子节点时，我们要将当前要删除的节点替换为子节点中最大的一个节点的值，然后将这个子节点删除。


















