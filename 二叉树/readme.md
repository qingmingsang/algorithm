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

二叉树分为完全二叉树(complete binary tree)和满二叉树(full binary tree)
- 满二叉树：一棵深度为k且有`2^k - 1`个节点的二叉树称为满二叉树
- 完全二叉树：完全二叉树是指最后一层左边是满的，右边可能满也可能不满，然后其余层都是满的二叉树称为完全二叉树(满二叉树也是一种完全二叉树)
@4


用一个数组来表示二叉树的结构，将一组数组从根节点开始从上到下，从左到右依次填入到一棵完全二叉树中，如下图所示
@5
通过上图我们可以分析得到数组表示的完全二叉树拥有以下几个性质：
- `left = index * 2 + 1`，例如：根节点的下标为0，则左节点的值为下标`array[0＊2+1]=1`
- `right = index * 2 + 2`，例如：根节点的下标为0，则右节点的值为下标`array[0＊2+2]=2`
- `序数 >= floor(N/2)`都是叶子节点，例如：`floor(9/2) = 4`，则从下标4开始的值都为叶子节点

## 二叉堆
二叉堆由一棵完全二叉树来表示其结构，用一个数组来表示，但一个二叉堆需要满足如下性质：
- 二叉堆的父节点的键值总是大于或等于(小于或等于)任何一个子节点的键值
- 当父节点的键值大于或等于(小于或等于)它的每一个子节点的键值时，称为最大堆（最小堆）

@6

从上图可以看出：
- 左图：父节点总是大于或等于其子节点，所以满足了二叉堆的性质，
- 右图：分支节点7作为2和12的父节点并没有满足其性质(大于或等于子节点)。

## 二叉搜索树
二叉搜索树满足以下的几个性质：
- 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
- 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值；
- 任意节点的左、右子树也需要满足左边小右边大的性质

一组数据：12,4,18,1,8,16,20
由下图可以看出，左边的图满足了二叉树的性质，它的每个左子节点都小于父节点，右子节点大于其父节点，同时左子树的节点都小于根节点，右子树的节点都大于根节点

@7

二叉搜索树主要的几个操作：
- 查找（search）
- 插入（insert）
- 遍历（transverse）

### 二叉树搜索树的链式存储结构
二叉搜索树的节点通常包含4个域，数据元素，分别指向其左，右节点的指针和一个指向父节点的指针所构成，一般把这种存储结构称为三叉链表。
@8


## 二叉搜索树实现
### 初始化一个二叉搜索树
在二叉搜索树中我们会维护一个root指针，这个就相当于链表中的head指针，在没有任何节点插入的时候它指向空，在有节点插入以后它指向根节点。
```
      // 节点的构造函数
      var Node = function (key) {
        this.key = key; // 节点的值
        this.left = null; // 左子树节点
        this.right = null; // 右子树节点
      }

      // 根节点
      var root = null;
```


插入一个键
```
      // 插入节点的实例方法
      this.insert = function (key) {
        var thisNode = new Node(key);

        // 根节点赋值
        if (root === null) {
          root = thisNode;
        } else {
          // 插入子节点
          insertNode(root, thisNode);
        }
      }
```
向树中插入一个新的节点主要有以下三部分：
1.创建新节点的Node类实例 --> 2.判断插入操作是否为根节点,是根节点就将其指向根节点 --> 3.将节点加入非根节点的其他位置。
```
      // 插入root的子节点
      var insertNode = function (parentNode, childNode) {
        // 左子树构造
        if (childNode.key < parentNode.key) {
          // 为空时赋值，否则继续延伸
          if (parentNode.left === null) {
            parentNode.left = childNode;
          } else {
            insertNode(parentNode.left, childNode);
          }
        } else { // 右子树的构造 childNode.key > parentNode.key
          if (parentNode.right === null) {
            parentNode.right = childNode;
          } else {
            insertNode(parentNode.right, childNode);
          }
        }

      }


    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    var binaryTree = new BinaryTree();
    nodes.forEach(key => {
      binaryTree.insert(key);
    });
```

## 树的遍历
- 中序遍历(inorder)：以从最小到最大的顺序访问所有节点
    - 先遍历左节点，再遍历自己，最后遍历右节点，输出的刚好是有序的列表
- 前序遍历(preorder)：以优先于后代节点的顺序访问每个节点
    - 先自己，再遍历左节点，最后遍历右节点
- 后序遍历(postorder)：先访问节点的后代节点再访问节点本身
    - 先左节点，再右节点，最后自己

最常用的一般是中序遍历，因为中序遍历可以得到一个已经排好序的列表，这也是为什么会用二叉搜索树排序的原因

### 中序遍历
使用中序遍历可以实现对树进行从小到大排序的功能。
```
      // 中序遍历的实例方法
      this.inOrderTraverse = function(node, printLog) {
        if (node !== null) {
          // 优先去找左子树，然后中间节点，最后右子树
          this.inOrderTraverse(node.left, printLog);
          printLog(node.key);
          this.inOrderTraverse(node.right, printLog);
        }
      }
    }


    // 节点数组
    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    // 打印方法
    var printLog = key => console.log(key);
    var binaryTree = new BinaryTree();
    nodes.forEach( key => {
      binaryTree.insert(key);
    });
    var root = binaryTree.getRoot();
    console.log(root);
    binaryTree.inOrderTraverse(root, printLog);
```
解析：从根节点(‘8’)作为入口，查看该节点的左子树，如果有，则进入该子树(‘3’)，查看’3’有无左子树，有则进入到’3’的左子树’1’，‘1’没有左子树，则输出该节点的值，为 1。‘1’没有右子树，则返回上一层’3’，输出 3。查看’3’的右子树，有则进入’5’，‘5’有左子树，则进入到’4’，‘4’为叶子节点，所以直接输出 4。
返回上一层’5’，并输出 5 。然后进入’5’的右子树’6’，‘6’为叶子节点直接输出 6。然后返回’5’，‘5’已输出继续返回上一层’3’，‘3’也输出了，返回到’8’，然后输出 8 。'8’的右子树同理。
则顺序为1、3、4、5、6、8、10、13、14 为升序


### 前序遍历
使用前序遍历可以实现结构化输出的功能。
高效率复制二叉树。
```
// 前序遍历的实例方法
      this.preTraverse = function(node, printLog) {
        if (node !== null) {
          // 优先输出中间节点，然后寻找左子树，最后右子树
          printLog(node.key);
          this.preTraverse(node.left, printLog);
          this.preTraverse(node.right, printLog);
        }
      }
    }


    // 节点数组
    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    // 打印方法
    var printLog = key => console.log(key);
    var binaryTree = new BinaryTree();
    nodes.forEach( key => {
      binaryTree.insert(key);
    });
    var root = binaryTree.getRoot();
    console.log(root);
    // 中序遍历
    // binaryTree.inOrderTraverse(root, printLog);

    // 前序遍历
    binaryTree.preTraverse(root, printLog);
```
解析：从根节点’8’进入并打印，然后进去左子树’3’并打印,再进入’3’的左子树’1’并打印,‘1’为叶子节点返回到上一层’3’，进入右子树’5’并打印，然后进入’5’的左子树’4’并打印，‘4’为叶子节点返回上一层’5’，进去右子树’6’并打印，然后逐一返回至根节点遍历根节点的右子树…
输出结果为：8、3、1、5、4、6、10、14、13

### 后序遍历
后序遍历可以用于计算有层级关系的所有元素的大小。
首先寻找左节点最深处的并打印，然后右子树并打印，最后中间节点。
```
	  // 后序遍历的实例方法
      this.postTraverse = function(node, printLog) {
        if (node !== null) {
          // 优先输出左子树，然后寻找右子树，最后中间节点
          this.postTraverse(node.left, printLog);
          this.postTraverse(node.right, printLog);
          printLog(node.key);
        }
      }
    }


    // 节点数组
    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    // 打印方法
    var printLog = key => console.log(key);
    var binaryTree = new BinaryTree();
    nodes.forEach( key => {
      binaryTree.insert(key);
    });
    var root = binaryTree.getRoot();
    console.log(root);
    // 中序遍历
    // binaryTree.inOrderTraverse(root, printLog);

    // 前序遍历
    // binaryTree.preTraverse(root, printLog);
    
    //后序遍历
    binaryTree.postTraverse(root, printLog);
```
输出结果为：1、4、6、5、3、13、14、10、8


## 查询节点
### 最小值
从root节点开始寻找左子树，如果没有左子树则表明该节点为最小值。
```
	  // 查询最小值
      this.minNode = function(node) {
        if (node !== null) {
          while(node && node.left !== null) {
            node = node.left;
          }
          return node.key;
        }
        return null;
      }
```

###  最大值
从root节点开始寻找右子树，如果没有右子树则表明该节点为最大值。
```
	  // 查询最大值
      this.maxNode = function(node) {
        if (node !== null) {
          while(node && node.right !== null) {
            node = node.right;
          }
          return node.key;
        }
        return null;
      }
```

### 搜索一个特定的值
```
      // 查找指定值
      this.searchNode = function(node, key) {
        if (node === null) {
          return false;
        }
        // 要查找的值小于当前节点的值，用左子树继续查找
        if (key < node.key) {
          return this.searchNode(node.left, key);
        } else if (key > node.key) { // 要查找的值大于当前节点的值，用右子树继续查找
          return this.searchNode(node.right, key);
        } else {
          return true;
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

## es5完整实现
```
    // 二叉树构造函数
    function BinaryTree() {
      // 节点的构造函数
      var Node = function (key) {
        this.key = key; // 节点的值
        this.left = null; // 左子树节点
        this.right = null; // 右子树节点
      }
      // 根节点
      var root = null;
      // 插入root的子节点
      var insertNode = function (parentNode, childNode) {
        // 左子树构造
        if (childNode.key < parentNode.key) {
          // 为空时赋值，否则继续延伸
          if (parentNode.left === null) {
            parentNode.left = childNode;
          } else {
            insertNode(parentNode.left, childNode);
          }
        } else { // 右子树的构造 childNode.key > parentNode.key
          if (parentNode.right === null) {
            parentNode.right = childNode;
          } else {
            insertNode(parentNode.right, childNode);
          }
        }
      }
      // 获取根节点
      this.getRoot = function () {
        return root;
      }
      // 插入节点的实例方法
      this.insert = function (key) {
        var thisNode = new Node(key);
        // 根节点赋值
        if (root === null) {
          root = thisNode;
        } else {
          // 插入子节点
          insertNode(root, thisNode);
        }
      }
      // 中序遍历的实例方法
      this.inOrderTraverse = function (node, printLog) {
        if (node !== null) {
          // 优先去找左子树，然后中间节点，最后右子树
          this.inOrderTraverse(node.left, printLog);
          printLog(node.key);
          this.inOrderTraverse(node.right, printLog);
        }
      }
      // 前序遍历的实例方法
      this.preTraverse = function (node, printLog) {
        if (node !== null) {
          // 优先输出中间节点，然后寻找左子树，最后右子树
          printLog(node.key);
          this.preTraverse(node.left, printLog);
          this.preTraverse(node.right, printLog);
        }
      }
      // 后序遍历的实例方法
      this.postTraverse = function (node, printLog) {
        if (node !== null) {
          // 优先输出左子树，然后寻找右子树，最后中间节点
          this.postTraverse(node.left, printLog);
          this.postTraverse(node.right, printLog);
          printLog(node.key);
        }
      }
      // 查询最小值
      this.minNode = function (node) {
        if (node !== null) {
          while (node && node.left !== null) {
            node = node.left;
          }
          return node.key;
        }
        return null;
      }
      // 查询最大值
      this.maxNode = function (node) {
        if (node !== null) {
          while (node && node.right !== null) {
            node = node.right;
          }
          return node.key;
        }
        return null;
      }
      // 查找指定值
      this.searchNode = function (node, key) {
        if (node === null) {
          return false;
        }
        // 要查找的值小于当前节点的值，用左子树继续查找
        if (key < node.key) {
          return this.searchNode(node.left, key);
        } else if (key > node.key) { // 要查找的值大于当前节点的值，用右子树继续查找
          return this.searchNode(node.right, key);
        } else {
          return true;
        }
      }
    }

    // 节点数组
    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    // 打印方法
    var printLog = key => console.log(key);
    var binaryTree = new BinaryTree();
    nodes.forEach(key => {
      binaryTree.insert(key);
    });
    var root = binaryTree.getRoot();
    // 中序遍历
    // binaryTree.inOrderTraverse(root, printLog);
    // 前序遍历
    // binaryTree.preTraverse(root, printLog);
    //后序遍历
    // binaryTree.postTraverse(root, printLog);
    // 最小节点
    console.log('binaryTree.minNode(root): ', binaryTree.minNode(root));
    // 最大节点
    console.log('binaryTree.maxNode(root): ', binaryTree.maxNode(root));
    console.log('searchNode(root, 1): ', binaryTree.searchNode(root, 2));
```

## es6完整实现
```
    class BinaryNode {
      constructor(val) {
        this.value = val;
        this.left = null;
        this.right = null;
      }
    }
    class BinarySearchTree {
      constructor() {
        this.root = null;
        this.values = new Array();
      }

      /**
       * [insert 插入节点]
       * @param  {[type]} val [description]
       * @return {[type]}     [description]
       */
      insert(val) {
        this.values.push(val);
        let node = new BinaryNode(val);
        if (!this.root) {
          this.root = node;
        } else {
          this._insertNode(this.root, node);
        }
      }

      /**
       * [remove 移除指定值]
       * @param  {[*]} val [目标值]
       * @return {[type]}     [description]
       */
      remove(val) {
        this.root = this._removeNode(this.root, val);
      }

      /**
       * [search 检索]
       * @param  {[*]} val [被检索值]
       * @return {[Boolean]}     [表示是否存在]
       */
      search(val) {
        let values = this.inOrderTraverse();
        return values.includes(val);
      }

      /**
       * [min 返回最小值]
       * @return {[type]} [description]
       */
      min() {
        let values = this.inOrderTraverse();
        return values[0];
      }

      /**
       * [max 返回最大值]
       * @return {[type]} [description]
       */
      max() {
        let values = this.inOrderTraverse();
        return values[values.length - 1];
      }

      /**
       * [isEmpty 是否为空二叉树]
       * @return {Boolean}
       */
      isEmpty() {
        return this.root === null;
      }

      /**
       * [inOrderTraverse 中序遍历]
       * @return {[Array]} [description]
       */
      inOrderTraverse() {
        let result = new Array();
        this._inOrderTraverseNode(this.root, function (node) {
          result.push(node.value);
        })
        return result;
      }

      /**
       * [preOrderTraverse 先序遍历]
       * @return {[Array]} [description]
       */
      preOrderTraverse() {
        let result = new Array();
        this._preOrderTraverseNode(this.root, function (node) {
          result.push(node.value);
        })
        return result;
      }

      /**
       * [postOrderTraverse 后序遍历]
       * @return {[Array]} [description]
       */
      postOrderTraverse() {
        let result = new Array();
        this._postOrderTraverseNode(this.root, function (node) {
          result.push(node.value);
        })
        return result;
      }

      /**
       * [breadthFirstSearch 广度优先遍历]
       * @return {[Array]} [遍历后结果]
       */
      breadthFirstSearch() {
        let result = new Array();
        let content = new Array();
        let current_node = null;
        content.push(this.root);
        while (content.length) {
          current_node = content.shift();
          result.push(current_node.value);
          if (current_node.right) {
            content.push(current_node.right);
          }
          if (current_node.left) {
            content.push(current_node.left);
          }
        }
        return result;
      }

      /**
       * [_insertNode 在指定节点插入节点]
       * @param  {[BinaryNode]} node    [目标节点]
       * @param  {[BinaryNode]} newNode [待插入节点]
       */
      _insertNode(node, newNode) {
        if (node.value > newNode.value) {
          if (node.left) {
            this._insertNode(node.left, newNode);
          } else {
            node.left = newNode;
          }
        } else {
          if (node.right) {
            this._insertNode(node.right, newNode);
          } else {
            node.right = newNode;
          }
        }
      }

      /**
       * [_removeNode 移除节点递归]
       * @param  {[BinaryNode]} node [当前节点]
       * @param  {[*]} val  [要移的除节点值]
       * @return {[BinaryNode]}      [当前节点]
       */
      _removeNode(node, val) {
        if (node === null) {
          return node;
        }
        //递归寻找目标节点
        if (val < node.value) {
          this._removeNode(node.left, val);
          return node;
        }

        if (val > node.value) {
          this._removeNode(node.right, val);
          return node;
        }
        //找到目标节点
        if (val === node.value) {
          //为叶子节点
          if (node.left === null && node.right === null) {
            node = null;
            return node;
          }
          //只有一个子节点
          if (node.left === null) {
            node = node.right;
            return node;
          } else if (node.right === null) {
            node = node.left;
            return node;
          }
          //有两个子节点
          let min_node = this._findMinNode(node);
          node.value = min_node.value;
          node.right = this._removeNode(node.right, min_node.value);
          return node;
        }
      }

      /**
       * [_findMinNode 查找最小节点]
       * @param  {[BinaryNode]} node [当前节点]
       * @return {[BinaryNode]}      [最小的节点]
       */
      _findMinNode(node) {
        while (node && node.left) {
          node = node.left;
        }
        return node;
      }

      /**
       * [_inOrderTraverseNode 中序遍历递归]
       * @param  {[BinaryNode]}   node     [当前节点]
       * @param  {Function} callback [回调函数]
       * @return {[type]}            [description]
       */
      _inOrderTraverseNode(node, callback) {
        if (node) {
          this._inOrderTraverseNode(node.left, callback);
          callback(node);
          this._inOrderTraverseNode(node.right, callback);
        }
      }

      /**
       * [_preOrderTraverseNode 先序遍历递归]
       * @param  {[BinaryNode]}   node     [当前节点]
       * @param  {Function} callback [回调函数]
       * @return {[type]}            [description]
       */
      _preOrderTraverseNode(node, callback) {
        if (node) {
          callback(node);
          this._preOrderTraverseNode(node.left, callback);
          this._preOrderTraverseNode(node.right, callback);
        }
      }

      /**
       * [_postOrderTraverseNode 后序遍历递归]
       * @param  {[BinaryNode]}   node     [当前节点]
       * @param  {Function} callback [回调函数]
       * @return {[type]}            [description]
       */
      _postOrderTraverseNode(node, callback) {
        if (node) {
          this._postOrderTraverseNode(node.left, callback);
          this._postOrderTraverseNode(node.right, callback);
          callback(node);
        }
      }
    }
    // 节点数组
    var nodes = [8, 3, 10, 1, 5, 14, 4, 6, 13];
    var binaryTree = new BinarySearchTree();
    nodes.forEach(key => {
      binaryTree.insert(key);
    });
    console.log(binaryTree.root)
```


















