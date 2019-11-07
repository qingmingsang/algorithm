## 链表和数组
这里大致模拟一下数组的插入操作：
```
    function insert(arr, index, data) {
      for (let i = arr.length; i >index; i--) {
        arr[i] = arr[i - 1];
      }
      arr[index] = data;
    }
```
从上面的代码可以看出数组的插入以及删除都有可能会是一个`O(n)`的操作。从而就引出了链表这种数据结构，链表不要求逻辑上相邻的元素在物理位置上也相邻，因此它没有顺序存储结构所具有的缺点，当然它也失去了数组在一块连续空间内随机存取的优点。

# 单向链表

@1

单向链表的特点：
- 用一组任意的内存空间去存储数据元素（这里的内存空间可以是连续的，也可以是不连续的）
- 每个节点(node)都由数据本身和一个指向后续节点的指针组成
- 整个链表的存取必须从头指针开始，头指针指向第一个节点
- 最后一个节点的指针指向空（NULL）

链表中的几个主要操作
- 创建节点
- 插入节点
- 搜索/遍历节点
- 删除节点
- 合并

## 初始化节点

- 指针指向空
- 存储数据

```
    class Node {
        constructor(key) {
            this.next = null;
            this.key = key;
        }
    }
```

## 初始化单向链表
每个链表都有一个头指针，指向第一个节点，没节点则指向NULL
```
    class List {
        constructor() {
            this.head = null;
        }
    }
```

## 创建节点
```
    static createNode(key) {
        return new createNode(key);
    }
```
从创建一个链表 -> 创建一个节点 -> 将节点插入进链表中。


## 插入节点（插入到头节点之后）
插入操作只需要去调整节点的指针即可,两种情况：

- head没有指向任何节点，说明当前插入的节点是第一个
    - head指向新节点
    - 新节点的指针指向NULL
- head有指向的节点
    - head指向新的节点
    - 新节点的指针指向原本head所指向的节点

```
    insert(node) {
        // 如果head有指向的节点
        if(this.head){
           node.next = this.head;
        }else {
           node.next = null;
        }
        this.head = node;
    }
```

## 搜索节点
从head开始查找
找到节点中的key等于想要查找的key的时候，返回该节点
```
    find(key) {
        let node = this.head;
        while(node !== null && node.key !== key){
            node = node.next;
        }
        return node;
    }
```

## 删除节点
这里分三种情况：

- 所要删除的节点刚好是第一个，也就是head指向的节点
    - 将head指向所要删除节点的下一个节点(node.next)
- 要删除的节点为最后一个节点
    - 寻找到所要删除节点的上一个节点(prevNode)
    - 将prevNode中的指针指向NULL
- 在列表中间删除某个节点
    - 寻找到所要删除节点的上一个节点(prevNode)
    - 将prevNode中的指针指向当前要删除的这个节点的下一个节点

```
    delete(node) {
        // 第一种情况
        if(node === this.head){
            this.head = node.next;
            return;
        }
        
        // 查找所要删除节点的上一个节点
        let prevNode = this.head;
        while (prevNode.next !== node) {
            prevNode = prevNode.next;
        }
        
        // 第二种情况
        if(node.next === null) {
            prevNode.next = null;
        }
        
        // 第三种情况
        if(node.next) {
            prevNode.next = node.next;
        }
    }
```

完整的单向链表代码
```
class ListNode {
  constructor(key) {
    this.next = null;
    this.key = key;
  }
}

class List {
  constructor() {
    this.head = null;
    this.length = 0;
  }

  static createNode(key) {
    return new ListNode(key);
  }

  // 往头部插入数据
  insert(node) {
    // 如果head后面有指向的节点
    if (this.head) {
      node.next = this.head;
    } else {
      node.next = null;
    }
    this.head = node;
    this.length++;
  }

  find(key) {
    let node = this.head;
    while (node !== null && node.key !== key) {
      node = node.next;
    }
    return node;
  }

  delete(node) {
    if (this.length === 0) {
      throw 'node is undefined';
    }

    if (node === this.head) {
      this.head = node.next;
      this.length--;
      return;
    }

    let prevNode = this.head;

    while (prevNode.next !== node) {
      prevNode = prevNode.next;
    }

    if (node.next === null) {
      prevNode.next = null;
    }
    if (node.next) {
      prevNode.next = node.next;
    }
    this.length--;
  }
}
```

```
function LinkedList() {
  let Node = function (element) {
    this.element = element
    this.next = null
  }

  let length = 0
  let head = null

  this.append = function (element) { // 向链表尾部加一个元素
    let node = new Node(element)
    let current

    if (head === null) {
      head = node
    } else {
      current = head

      while (current.next) {
        current = current.next
      }

      current.next = node
    }

    length++
  }

  this.removeAt = function (position) { // 移除指定位置的元素
    if (position > -1 && position < length) {
      let current = head
      previous,
        index = 0

      if (position === 0) {
        head = current.next
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }

        previous.next = current.next
      }

      length--

      return current.element
    } else {
      return null
    }
  }

  this.insert = function (position, element) { // 在任意位置插入元素
    if (position >= 0 && position <= length) {
      let node = new Node(element),
        current = head,
        previous,
        index = 0

      if (position === 0) {
        node.next = current
        head = node
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }
        node.next = current
        previous.next = node
      }

      length++

      return true
    } else {
      return false
    }
  }

  this.toString = function () {
    let current = head,
      string = ''

    while (current) {
      string += current.element + (current.next ? ',' : '')
      current = current.next
    }
    return string
  }

  this.indexOf = function (element) {
    let current = head,
      index = 0

    while (current) {
      if (element === current.element) {
        return index
      }
      index++
      current = current.next
    }
    return -1
  }

  this.remove = function (element) {
    let index = this.indexOf(element)
    return this.removeAt(index)
  }

  this.isEmpty = function () {
    return length === 0
  }

  this.size = function () {
    return length
  }

  this.getHead = function () {
    return head
  }

}
```


# 双向链表
双向链表多了一个指向上一个节点的指针。

@2

## 初始化节点

- 指向前一个节点的指针
- 指向后一个节点的指针
- 节点数据

```
    class ListNode {
        this.prev = null;
        this.next = null;
        this.key = key;
    }
```

## 初始化双向链表
头指针指向NULL
```
    class List {
        constructor(){
            this.head = null;
        }
    }
```

## 创建节点
```
    static createNode(key){
        return new ListNode(key);
    }
```

## 插入节点（（插入到头节点之后）

- 看上图中head后面的第一个节点可以知道，该节点的prev指向NULL
- 节点的next指针指向后一个节点, 也就是当前头指针所指向的那个节点
- 如果head后有节点，那么原本head后的节点的prev指向新插入的这个节点(因为是双向的)
- 最后将head指向新的节点

```
    insert(node) {
        node.prev = null;
        node.next = this.head;
        if(this.head){
            this.head.prev = node;
        }
        this.head = node;
    }
```

## 搜索节点
这里和单向节点一样
```
  search(key) {
    let node = this.head;
    while (node !== null && node.key !== key) {
      node = node.next;
    }
    return node;
  }
```

## 删除节点
和之前单向链表一样，分三种情况去看：

- 删除的是第一个节点
    - head指向所要删除节点的下一个节点
    - 下一个节点的prev指针指向所要删除节点的上一个节点
- 删除的是中间的某个节点
    - 所要删除的前一个节点的next指向所要删除的下一个节点
    - 所要删除的下一个节点的prev指向所要删除的前一个节点
- 删除的是最后一个节点
    - 要删除的节点的上一个节点的next指向null（也就是指向删除节点的next所指的地址）

```
    delete(node) {
        const {prev,next} = node;
        delete node.prev;
        delete node.next;
        if(node === this.head){
            this.head = next;
        }
        if(next){
            next.prev = prev;
        }
        if(prev){
            prev.next = next;
        }
    }
```

双向链表完整代码
```
class ListNode {
  constructor(key) {
    // 指向前一个节点
    this.prev = null;
    // 指向后一个节点
    this.next = null;
    // 节点的数据(或者用于查找的键)
    this.key = key;
  }
}

/**
 * 双向链表
 */
class List {
  constructor() {
    this.head = null;
  }

  static createNode(key) {
    return new ListNode(key);
  }

  insert(node) {
    node.prev = null;
    node.next = this.head;
    if (this.head) {
      this.head.prev = node;
    }
    this.head = node;
  }

  search(key) {
    let node = this.head;
    while (node !== null && node.key !== key) {
      node = node.next;
    }
    return node;
  }

  delete(node) {
    const { prev, next } = node;
    delete node.prev;
    delete node.next;

    if (node === this.head) {
      this.head = next;
    }

    if (prev) {
      prev.next = next;
    }
    if (next) {
      next.prev = prev;
    }
  }
}
```

```
function DoubleLinkedList() {
  let Node = function (element) {
    this.element = element
    this.next = null
    this.prev = null
  }

  let length = 0
  let head = null
  let tail = null

  this.append = function (element) {
    let node = new Node(element),
      current

    if (head === null) {
      head = node
      tail = node
    } else {
      current = tail
      current.next = node
      node.prev = current
      tail = node
    }
    length++
  }

  this.insert = function (position, element) {
    if (position >= 0 && position <= length) {
      let node = new Node(element),
        current = head,
        previous,
        index = 0

      if (position === 0) {
        if (!head) {
          head = node
          tail = node
        } else {
          node.next = current
          current.prev = node
          head = node
        }
      } else if (position === length) {
        current = tail
        current.next = node
        node.prev = current
        tail = node
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }
        node.next = current
        previous.next = node

        current.prev = node
        node.prev = previous
      }

      length++

      return true
    } else {
      return false
    }
  }

  this.removeAt = function (position) {
    if (position > -1 && position < length) {
      let current = head,
        previous,
        index = 0

      if (position === 0) {
        head = current.next

        if (length === 1) {
          tail = null
        } else {
          head.prev = null
        }
      } else if (position === length - 1) {
        current = tail
        tail = current.prev
        tail.next = null
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }

        previous.next = current.next
        current.next.prev = previous
      }

      length--

      return current.element
    } else {
      return null
    }
  }

  // 其他方法实现和单向链表一样
}
```


# 循环链表
循环链表和单向链表相似，节点类型都是一样的。唯一的区别是，在创建循环链表时，让其头节点的 next 属性指向它本身，即链表的尾节点指向头节点，形成了一个循环链表。

@4

@5

单向循环链表代码
````
function CircularLinkedList() {
  const Node = function (element) {
    this.element = element
    this.next = null
  }
  let head = null
  let length = 0

  this.append = function (element) {
    let node = new Node(element)
    let current
    if (head === null) {
      head = node
    } else {
      current = head
      while (current.next !== head) {
        current = current.next
      }
      current.next = node
      node.next = head
      length++
    }
  }

  this.removeAt = function (position) {
    if (position > -1 && position < length) {
      let current = head,
        previous,
        index = 0

      if (position === 0) {
        while (current.next !== head) {
          current = current.next
        }
        head = head.next
        current.next = head
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }
        previous.next = current.next
      }
      length--
      return current.element
    } else {
      return null
    }
  }

  this.insert = function (position, element) {
    if (position >= 0 && position <= length) {
      let node = new Node(element),
        current = head,
        previous,
        index = 0

      if (position === 0) {
        node.next = current
        while (current.next !== head) {
          current = current.next
        }
        head = node
        current.next = head
      } else {
        while (index++ < position) {
          previous = current
          current = current.next
        }
        previous.next = node
        node.next = current

        if (node.next === null) {
          node.next = head
        }
      }

      length++
      return true
    } else {
      return false
    }
  }

  // 其他方法实现和单向链表一样
  
}
```