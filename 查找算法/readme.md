# 查找算法
查找（Searching）就是根据给定的某个值，在查找表中确定一个其关键字等于给定值的数据元素。

查找表（Search Table）：由同一类型的数据元素构成的集合

关键字（Key）：数据元素中某个数据项的值，又称为键值

主键（Primary Key）：可唯一的标识某个数据元素或记录的关键字

## 查找算法分类：
- 静态查找和动态查找:
    - 静态查找表（Static Search Table）：只做查找操作的查找表。
      1. 查询某个“特定的”数据元素是否在表中
      2. 检索某个“特定的”数据元素和各种属性
    - 动态查找表（Dynamic Search Table）：在查找中同时进行插入或删除等操作：
      1. 查找时插入数据
      2. 查找时删除数据
- 无序查找和有序查找:
    - 无序查找：被查找数列有序无序均可；
    - 有序查找：被查找数列必须为有序数列。

**平均查找长度（Average Search Length，ASL）**：
需和指定key进行比较的关键字的个数的期望值，称为查找算法在查找成功时的平均查找长度。
对于含有n个数据元素的查找表，查找成功的平均查找长度为：`ASL = Pi*Ci`的和。
- Pi：查找表中第i个数据元素的概率。
- Ci：找到第i个数据元素时已经比较过的次数。

## 顺序查找
说明：顺序查找适合于存储结构为顺序存储或链接存储的线性表。

基本思想：顺序查找也称为线形查找，属于无序查找算法。从数据结构线形表的一端开始，顺序扫描，依次将扫描到的结点关键字与给定值k相比较，若相等则表示查找成功；若扫描结束仍没有找到关键字等于k的结点，表示查找失败。

优缺点:
- 缺点：是当n 很大时，平均查找长度较大，效率低；
- 优点：是对表中数据元素的存储没有要求。另外，对于线性链表，只能进行顺序查找。

复杂度分析：
查找成功时的平均查找长度为：（假设每个数据元素的概率相等） `ASL = 1/n(1+2+3+…+n) = (n+1)/2 `;
当查找不成功时，需要n+1次比较，时间复杂度为`O(n)`;
所以，顺序查找的时间复杂度为`O(n)`。

C++实现
```
int SequenceSearch(int a[], int value, int n)
{
    int i;
    for(i=0; i<n; i++)
        if(a[i]==value)
            return i;
    return -1;
}
```
Python实现
```
# 最基础的遍历无序列表的查找算法

def sequential_search(lis, key):
  length = len(lis)
  for i in range(length):
    if lis[i] == key:
      return i
  return -1
 
LIST = [1, 5, 8, 123, 22, 54, 7, 99, 300, 222]
result = sequential_search(LIST, 123)
print(result)
```
js实现
```
    const SequenceSearch = (arr = [], value = 0, length = 0) => {
      for (let i = 0; i < length; i++) {
        if (arr[i] == value) {
          return i;
        }
      }
      return -1;
    }

    const SequenceSearch = (arr = [], value = 0) => {
      let result = -1;
      arr.forEach((v, k) => {
        if (v == value) {
          result = k;
        }
      })
      return result;
    }

    const SequenceSearch = (arr = [], value = 0) => {
      return arr.find((v, k) => {
        return v == value;
      }) || null;
    }
    //用js自带的方法反倒是最快的
    const SequenceSearch = (arr = [], value = 0) => {
      return arr.indexOf(value);
    }
```

## 二分查找
说明：元素必须是有序的，如果是无序的则要先进行排序操作。

基本思想：也称为是折半查找，属于有序查找算法。用给定值k先与中间结点的关键字比较，中间结点把线形表分成两个子表，若相等则查找成功；若不相等，再根据k与该中间结点关键字的比较结果确定下一步查找哪个子表，这样递归进行，直到查找到或查找结束发现表中没有这样的结点。

算法描述:
给予一个包含 n个带值元素的数组A 
1. 令 L为0 ， R为 n-1 ；
2. 如果`L>R`，则搜索以失败告终 ；
3. 令 m (中间值元素)为`(L+R)/2`；
4. 如果 `Am<T`，令 L为 `m + 1` 并回到步骤二 ；
5. 如果 `Am>T`，令 R为 `m - 1` 并回到步骤二；

复杂度分析：最坏情况下，关键词比较次数为`log2(n+1)`，且期望时间复杂度为`O(log2n)`；
空间复杂度：`O(1)`

注：折半查找的前提条件是需要有序表顺序存储，对于静态查找表，一次排序后不再变化，折半查找能得到不错的效率。但对于需要频繁执行插入或删除操作的数据集来说，维护有序的排序会带来不小的工作量，那就不建议使用。——《大话数据结构》

C++实现
```
//二分查找（折半查找），版本1
int BinarySearch1(int a[], int value, int n)
{
    int low, high, mid;
    low = 0;
    high = n-1;
    while(low<=high)
    {
        mid = (low+high)/2;
        if(a[mid]==value)
            return mid;
        if(a[mid]>value)
            high = mid-1;
        if(a[mid]<value)
            low = mid+1;
    }
    return -1;
}

//二分查找，递归版本
int BinarySearch2(int a[], int value, int low, int high)
{
    //查找失败的终止条件
    if(low>high){
        return -1;
    }
    int mid = low+(high-low)/2;
    if(a[mid]==value)
        return mid;
    if(a[mid]>value)
        return BinarySearch2(a, value, low, mid-1);
    if(a[mid]<value)
        return BinarySearch2(a, value, mid+1, high);
}
```
Python实现
```
# 针对有序查找表的二分查找算法
 
def binary_search(lis, key):
  low = 0
  high = len(lis) - 1
  time = 0
  while low <= high:
    time += 1
    mid = int((low + high) / 2)
    if key < lis[mid]:
      high = mid - 1
    elif key > lis[mid]:
      low = mid + 1
    else:
      # 打印折半的次数
      print("times: %s" % time)
      return mid
  print("times: %s" % time)
  return -1
 
LIST = [1, 5, 7, 8, 22, 54, 99, 123, 200, 222, 444]
result = binary_search(LIST, 99)
print(result)
```
JS实现
```
    const BinarySearch1 = (a = [], value = 0, length = 0) => {
      let low = 0;//低位索引
      let high = length - 1;//高位索引
      let mid = 0;//中间索引
      while (low <= high) {
        mid = parseInt((low + high) / 2);
        if (a[mid] == value) {
          return mid;
        }
        if (a[mid] > value) {
          high = mid - 1;//进入左子表
        } else {
          //a[mid] < value
          low = mid + 1;//进入右子表
        }
      }
      return -1;
    }

    const BinarySearch2 = (a = [], value = 0, low = 0, high = 0) => {
      if (low > high) {
        return -1;
      }
      let mid = parseInt(low + (high - low) / 2);
      if (a[mid] == value) {
        return mid;
      }
      if (a[mid] > value) {
        //进入左子表
        return BinarySearch2(a, value, low, mid - 1);
      }
      if (a[mid] < value) {
        //进入右子表
        return BinarySearch2(a, value, mid + 1, high);
      }
    }
```

## 插值查找
折半查找这种查找方式，不是自适应的（也就是说是傻瓜式的）。
二分查找中查找点计算如下：
`mid=(low+high)/2`, 即`mid=low+1/2*(high-low)`;
通过类比，可以将查找的点改进为如下：
`mid=low+(key-a[low])/(a[high]-a[low])*(high-low)`，
也就是将上述的比例参数`1/2`改进为自适应的，根据关键字在整个有序表中所处的位置，让mid值的变化更靠近关键字key，这样也就间接地减少了比较次数。

基本思想：基于二分查找算法，将查找点的选择改进为自适应选择，可以提高查找效率。当然，差值查找也属于有序查找。
注：对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。

复杂度分析：查找成功或者失败的时间复杂度均为`O(log2(log2n))`。
空间复杂度：O(1)。

C++实现
```
int InsertionSearch(int a[], int value, int low, int high)
{
    int mid = low+(value-a[low])/(a[high]-a[low])*(high-low);
    if (mid < low || mid > high) {
        return -1;
    }
    if(a[mid]==value)
        return mid;
    if(a[mid]>value)
        return InsertionSearch(a, value, low, mid-1);
    if(a[mid]<value)
        return InsertionSearch(a, value, mid+1, high);
}
```
Python实现
```
def binary_search(lis, key):
  low = 0
  high = len(lis) - 1
  time = 0
  while low < high:
    time += 1
    # 计算mid值是插值算法的核心代码
    mid = low + int((high - low) * (key - lis[low])/(lis[high] - lis[low]))
    print("mid=%s, low=%s, high=%s" % (mid, low, high))
    if key < lis[mid]:
      high = mid - 1
    elif key > lis[mid]:
      low = mid + 1
    else:
      # 打印查找的次数
      print("times: %s" % time)
      return mid
  print("times: %s" % time)
  return -1
 
LIST = [1, 5, 7, 8, 22, 54, 99, 123, 200, 222, 444]
result = binary_search(LIST, 66)
print(result)
```
JS实现
```
    const InsertionSearch = (a = [], value = 0, low = 0, high = 0) => {
      let mid = parseInt(low + (value - a[low]) / (a[high] - a[low]) * (high - low));
      if (mid < low || mid > high) {
        return -1;
      }
      if (a[mid] == value) {
        return mid;
      }
      if (a[mid] > value) {
        return InsertionSearch(a, value, low, mid - 1);
      }
      if (a[mid] < value) {
        return InsertionSearch(a, value, mid + 1, high);
      }
    }
```

## 斐波那契查找
斐波那契数列，又称黄金分割数列，指的是这样一个数列：1、1、2、3、5、8、13、21、····，在数学上，斐波那契被递归方法如下定义：`F(1)=1`，`F(2)=1`，`F(n)=f(n-1)+F(n-2) （n>=2）` 。该数列越往后相邻的两个数的比值越趋向于黄金比例值（0.618）。

随着斐波那契数列的递增，前后两个数的比值会越来越接近0.618，利用这个特性，可以将黄金比例运用到查找技术中。

0.618被公认为最具有审美意义的比例数字，这个数值的作用不仅仅体现在诸如绘画、雕塑、音乐、建筑等艺术领域，而且在管理、工程设计等方面也有着不可忽视的作用。因此被称为黄金分割。


@1

基本思想：也是二分查找的一种提升算法，通过运用黄金比例的概念在数列中选择查找点进行查找，提高查找效率。同样地，斐波那契查找也属于一种有序查找算法。
相对于折半查找，一般将待比较的key值与第`mid=（low+high）/2`位置的元素比较，比较结果分三种情况：
  1. 相等，mid位置的元素即为所求
  2. 大于，low=mid+1;
  3. 小于，high=mid-1。

斐波那契查找与折半查找很相似，他是根据斐波那契序列的特点对有序表进行分割的。他要求开始表中记录的个数为某个斐波那契数小1，即`n=F(k)-1`;

开始将k值与第`F(k-1)`位置的记录进行比较(及`mid=low+F(k-1)-1`),比较结果也分为三种:
  1. 相等，mid位置的元素即为所求
  2. 大于,`low=mid+1`,`k-=2`;
    说明：`low=mid+1`说明待查找的元素在`[mid+1,high]`范围内，`k-=2` 说明范围`[mid+1,high]`内的元素个数为`n-(F(k-1))= Fk-1-F(k-1)=Fk-F(k-1)-1=F(k-2)-1`个，所以可以递归的应用斐波那契查找。
  3. 小于，`high=mid-1`,`k-=1`。
    说明：`low=mid+1`说明待查找的元素在`[low,mid-1]`范围内，`k-=1` 说明范围`[low,mid-1]`内的元素个数为`F(k-1)-1`个，所以可以递归 的应用斐波那契查找。

复杂度分析：最坏情况下，时间复杂度为`O(log2n)`，且其期望复杂度也为`O(log2n)`。

C++实现
```
// 斐波那契查找.cpp 

#include "stdafx.h"
#include <memory>
#include  <iostream>
using namespace std;

const int max_size=20;//斐波那契数组的长度

/*构造一个斐波那契数组*/ 
void Fibonacci(int * F)
{
    F[0]=0;
    F[1]=1;
    for(int i=2;i<max_size;++i)
        F[i]=F[i-1]+F[i-2];
}

/*定义斐波那契查找法*/  
int FibonacciSearch(int *a, int n, int key)  //a为要查找的数组,n为要查找的数组长度,key为要查找的关键字
{
  int low=0;
  int high=n-1;
  
  int F[max_size];
  Fibonacci(F);//构造一个斐波那契数组F 

  int k=0;
  while(n>F[k]-1)//计算n位于斐波那契数列的位置
      ++k;

  int  * temp;//将数组a扩展到F[k]-1的长度
  temp=new int [F[k]-1];
  memcpy(temp,a,n*sizeof(int));

  for(int i=n;i<F[k]-1;++i)
     temp[i]=a[n-1];
  
  while(low<=high)
  {
    int mid=low+F[k-1]-1;
    if(key<temp[mid])
    {
      high=mid-1;
      k-=1;
    }
    else if(key>temp[mid])
    {
     low=mid+1;
     k-=2;
    }
    else
    {
       if(mid<n)
           return mid; //若相等则说明mid即为查找到的位置
       else
           return n-1; //若mid>=n则说明是扩展的数值,返回n-1
    }
  }  
  delete [] temp;
  return -1;
}

int main()
{
    int a[] = {0,16,24,35,47,59,62,73,88,99};
    int key=100;
    int index=FibonacciSearch(a,sizeof(a)/sizeof(int),key);
    cout<<key<<" is located at:"<<index;
    return 0;
}
```
Python实现
```
# 斐波那契查找算法
 
def fibonacci_search(lis, key):
  # 需要一个现成的斐波那契列表。其最大元素的值必须超过查找表中元素个数的数值。
  F = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144,
     233, 377, 610, 987, 1597, 2584, 4181, 6765,
     10946, 17711, 28657, 46368]
  low = 0
  high = len(lis) - 1
   
  # 为了使得查找表满足斐波那契特性，在表的最后添加几个同样的值
  # 这个值是原查找表的最后那个元素的值
  # 添加的个数由F[k]-1-high决定
  k = 0
  while high > F[k]-1:
    k += 1
  print(k)
  i = high
  while F[k]-1 > i:
    lis.append(lis[high])
    i += 1
  print(lis)
   
  # 算法主逻辑。time用于展示循环的次数。
  time = 0
  while low <= high:
    time += 1
    # 为了防止F列表下标溢出，设置if和else
    if k < 2:
      mid = low
    else:
      mid = low + F[k-1]-1
     
    print("low=%s, mid=%s, high=%s" % (low, mid, high))
    if key < lis[mid]:
      high = mid - 1
      k -= 1
    elif key > lis[mid]:
      low = mid + 1
      k -= 2
    else:
      if mid <= high:
        # 打印查找的次数
        print("times: %s" % time)
        return mid
      else:
        print("times: %s" % time)
        return high
  print("times: %s" % time)
  return -1
 

LIST = [1, 5, 7, 8, 22, 54, 99, 123, 200, 222, 444]
result = fibonacci_search(LIST, 444)
print(result)
```
JS实现
```
    const Fibonacci = (F = [], max_size = 0) => {
      /*构造一个斐波那契数组*/
      F[0] = 0;
      F[1] = 1;
      for (let i = 2; i < max_size; ++i) {
        F[i] = F[i - 1] + F[i - 2];
      }
    }

    /*定义斐波那契查找法*/
    const FibonacciSearch = (a = [], key = 0, n = 0) => {
      //a为要查找的数组,n为要查找的数组长度,key为要查找的关键字
      let low = 0;
      let high = n - 1;

      //构造一个斐波那契数组F 
      let F = new Array(n);
      Fibonacci(F, n);

      //计算n位于斐波那契数列的位置
      let k = 0;
      while (n > F[k] - 1) {
        ++k;
      }

      //将数组a扩展到F[k]-1的长度
      let temp = [].concat(a);
      for (let i = n; i < F[k] - 1; ++i) {
        temp[i] = a[n - 1];
      }

      while (low <= high) {
        let mid = low + F[k - 1] - 1;
        if (key < temp[mid]) {
          high = mid - 1;
          k -= 1;
        } else if (key > temp[mid]) {
          low = mid + 1;
          k -= 2;
        } else {
          if (mid < n) {
            return mid; //若相等则说明mid即为查找到的位置
          }
          return n - 1; //若mid>=n则说明是扩展的数值,返回n-1
        }
      }
      return -1;
    }
```

# 树表查找
## 最简单的树表查找算法——二叉树查找算法。
基本思想：二叉查找树是先对待查找的数据进行生成树，确保树的左分支的值小于右分支的值，然后在就行和每个节点的父节点比较大小，查找最适合的范围。 这个算法的查找效率很高，但是如果使用这种查找方法要首先创建树。 

二叉查找树（BinarySearch Tree，也叫二叉搜索树，或称二叉排序树Binary Sort Tree）或者是一棵空树，或者是具有下列性质的二叉树：
- 若任意节点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 任意节点的左、右子树也分别为二叉查找树。

二叉查找树性质：对二叉查找树进行中序遍历，即可得到有序的数列。

不同形态的二叉查找树如下图所示：
@2

更详细可以看这个[浅谈算法和数据结构: 七 二叉查找树](https://www.cnblogs.com/yangecnu/p/Introduce-Binary-Search-Tree.html)

复杂度分析：它和二分查找一样，插入和查找的时间复杂度均为`O(logn)`，但是在最坏的情况下仍然会有`O(n)`的时间复杂度。原因在于插入和删除元素的时候，树没有保持平衡（比如，我们查找上图（b）中的“93”，我们需要进行n次查找操作）。我们追求的是在最坏的情况下仍然有较好的时间复杂度，这就是平衡查找树设计的初衷。

下图为二叉树查找和顺序查找以及二分查找性能的对比图：
@3

二叉树查找 Python实现
```

class BSTNode:
    """
    定义一个二叉树节点类。
    以讨论算法为主，忽略了一些诸如对数据类型进行判断的问题。
    """
    def __init__(self, data, left=None, right=None):
        """
        初始化
        :param data: 节点储存的数据
        :param left: 节点左子树
        :param right: 节点右子树
        """
        self.data = data
        self.left = left
        self.right = right


class BinarySortTree:
    """
    基于BSTNode类的二叉查找树。维护一个根节点的指针。
    """
    def __init__(self):
        self._root = None

    def is_empty(self):
        return self._root is None

    def search(self, key):
        """
        关键码检索
        :param key: 关键码
        :return: 查询节点或None
        """
        bt = self._root
        while bt:
            entry = bt.data
            if key < entry:
                bt = bt.left
            elif key > entry:
                bt = bt.right
            else:
                return entry
        return None

    def insert(self, key):
        """
        插入操作
        :param key:关键码 
        :return: 布尔值
        """
        bt = self._root
        if not bt:
            self._root = BSTNode(key)
            return
        while True:
            entry = bt.data
            if key < entry:
                if bt.left is None:
                    bt.left = BSTNode(key)
                    return
                bt = bt.left
            elif key > entry:
                if bt.right is None:
                    bt.right = BSTNode(key)
                    return
                bt = bt.right
            else:
                bt.data = key
                return

    def delete(self, key):
        """
        二叉查找树最复杂的方法
        :param key: 关键码
        :return: 布尔值
        """
        p, q = None, self._root     # 维持p为q的父节点，用于后面的链接操作
        if not q:
            print("空树！")
            return
        while q and q.data != key:
            p = q
            if key < q.data:
                q = q.left
            else:
                q = q.right
            if not q:               # 当树中没有关键码key时，结束退出。
                return
        # 上面已将找到了要删除的节点，用q引用。而p则是q的父节点或者None（q为根节点时）。
        if not q.left:
            if p is None:
                self._root = q.right
            elif q is p.left:
                p.left = q.right
            else:
                p.right = q.right
            return
        # 查找节点q的左子树的最右节点，将q的右子树链接为该节点的右子树
        # 该方法可能会增大树的深度，效率并不算高。可以设计其它的方法。
        r = q.left
        while r.right:
            r = r.right
        r.right = q.right
        if p is None:
            self._root = q.left
        elif p.left is q:
            p.left = q.left
        else:
            p.right = q.left

    def __iter__(self):
        """
        实现二叉树的中序遍历算法,
        展示我们创建的二叉查找树.
        直接使用python内置的列表作为一个栈。
        :return: data
        """
        stack = []
        node = self._root
        while node or stack:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            yield node.data
            node = node.right


lis = [62, 58, 88, 48, 73, 99, 35, 51, 93, 29, 37, 49, 56, 36, 50]
bs_tree = BinarySortTree()
for i in range(len(lis)):
    bs_tree.insert(lis[i])
# bs_tree.insert(100)
bs_tree.delete(58)
for i in bs_tree:
    print(i, end=" ")
# print("\n", bs_tree.search(4))
```
JS实现
```
// 二叉查找树类
function BinarySearchTree() {
    // 用于实例化节点的类
    var Node = function(key){
        this.key = key; // 节点的健值
        this.left = null; // 指向左节点的指针
        this.right = null; // 指向右节点的指针
    };
    var root = null; // 将根节点置为null
}

this.inOrderTraverse = function(callback){
    // callback用于对遍历到的节点做操作
    inOrderTraverseNode(root, callback);
};
var inOrderTraverseNode = function (node, callback) {
    // 遍历到node为null为止
    if (node !== null) {
        // 优先遍历左边节点，保证从小到大遍历
        inOrderTraverseNode(node.left, callback);
        // 处理当前的节点
        callback(node.key);
        // 遍历右侧节点
        inOrderTraverseNode(node.right, callback);
    }
};

this.preOrderTraverse = function(callback){
    // 同样的，callback用于对遍历到的节点做操作
    preOrderTraverseNode(root, callback);
};
var preOrderTraverseNode = function (node, callback) {
    // 遍历到node为null为止
    if (node !== null) {
        callback(node.key); // 先处理当前节点
        preOrderTraverseNode(node.left, callback); // 再继续遍历左子节点
        preOrderTraverseNode(node.right, callback); // 最后遍历右子节点
    }
};

this.postOrderTraverse = function(callback){
    postOrderTraverseNode(root, callback);
};
var postOrderTraverseNode = function (node, callback) {
    if (node !== null) {
        postOrderTraverseNode(node.left, callback); //{1}
        postOrderTraverseNode(node.right, callback); //{2}
        callback(node.key); //{3}
    }
};

this.search = function(key, node){
    // 同样的，search方法允许在子树中查找值
    node = node || root;
    return searchNode(key, node);
};
var searchNode = function(key, node){
    // 如果node是null，说明树中没有要查找的值，返回false
    if (node === null){
        return false;
    }
    if (key < node.key){
        // 如果要查找的值小于该节点，继续递归遍历其左侧节点
        return searchNode(node.left, key);
    } else if (key > node.key){
        // 如果要查找的值大于该节点，继续递归遍历其右侧节点
        return searchNode(node.right, key);
    } else {
        // 如果要查找的值等于该节点，说明查找成功，返回改节点
        return node;
    }
};
```

基于二叉查找树进行优化，进而可以得到其他的树表查找算法，如平衡树、红黑树等高效算法。

## 平衡查找树之2-3查找树（2-3 Tree）
2-3查找树定义：和二叉树不一样，2-3树运行每个节点保存1个或者两个的值。对于普通的2节点(2-node)，他保存1个key和左右两个自己点。
对应3节点(3-node)，保存两个Key，2-3查找树的定义如下：
- 要么为空，要么：
    1. 对于2节点，该节点保存一个key及对应value，以及两个指向左右节点的节点，左节点也是一个2-3节点，所有的值都比key要小，右节点也是一个2-3节点，所有的值比key要大。
    2. 对于3节点，该节点保存两个key及对应value，以及三个指向左中右的节点。左节点也是一个2-3节点，所有的值均比两个key中的最小的key还要小；中间节点也是一个2-3节点，中间节点的key值在两个跟节点key值之间；右节点也是一个2-3节点，节点的所有key值比两个key中的最大的key还要大。

@4

2-3查找树的性质：
- 如果中序遍历2-3查找树，就可以得到排好序的序列；
- 在一个完全平衡的2-3查找树中，根节点到每一个为空节点的距离都相同。（这也是平衡树中“平衡”一词的概念，根节点到叶节点的最长距离对应于查找算法的最坏情况，而平衡树中根节点到叶节点的距离都一样，最坏情况也具有对数复杂度。）

@5

复杂度分析：
2-3树的查找效率与树的高度是息息相关的。
- 在最坏的情况下，也就是所有的节点都是2-node节点，查找效率为`lgN`
- 在最好的情况下，所有的节点都是3-node节点，查找效率为`log3N`约等于`0.631lgN`

距离来说，对于1百万个节点的2-3树，树的高度为12-20之间，对于10亿个节点的2-3树，树的高度为18-30之间。
对于插入来说，只需要常数次操作即可完成，因为他只需要修改与该节点关联的节点即可，不需要检查其他节点，所以效率和查找类似。
下面是2-3查找树的效率：
@6

Python实现
```
class Node(object):
    def __init__(self,key):
        self.key1=key
        self.key2=None
        self.left=None
        self.middle=None
        self.right=None
    def isLeaf(self):
        return self.left is None and self.middle is None and self.right is None
    def isFull(self):
        return self.key2 is not None
    def hasKey(self,key):
        if (self.key1==key) or (self.key2 is not None and self.key2==key):
            return True
        else:
            return False
    def getChild(self,key):
        if key<self.key1:
            return self.left
        elif self.key2 is None:
            return self.middle
        elif key<self.key2:
            return self.middle
        else:
            return self.right
            
class two_three_Tree(object):
    def __init__(self):
        self.root=None
    def get(self,key):
        if self.root is None:
            return None
        else:
            return self._get(self.root,key)
    def _get(self,node,key):
        if node is None:
            return None
        elif node.hasKey(key):
            return node
        else:
            child=node.getChild(key)
            return self._get(child,key)
    def put(self,key):
        if self.root is None:
            self.root=Node(key)
        else:
            pKey,pRef=self._put(self.root,key)
            if pKey is not None:
                newnode=Node(pKey)
                newnode.left=self.root
                newnode.middle=pRef
                self.root=newnode
    def _put(self,node,key):
        if node.hasKey(key):
            return None,None
        elif node.isLeaf():
            return self._addtoNode(node,key,None)
        else:
            child=node.getChild(key)
            pKey,pRef=self._put(child,key)
            if pKey is None:
                return None,None
            else:
                return self._addtoNode(node,pKey,pRef)
             
         
    def _addtoNode(self,node,key,pRef):
        if node.isFull():
            return self._splitNode(node,key,pRef)
        else:
            if key<node.key1:
                node.key2=node.key1
                node.key1=key
                if pRef is not None:
                    node.right=node.middle
                    node.middle=pRef
            else:
                node.key2=key
                if pRef is not None:
                    node.right=Pref
            return None,None
    def _splitNode(self,node,key,pRef):
        newnode=Node(None)
        if key<node.key1:
            pKey=node.key1
            node.key1=key
            newnode.key1=node.key2
            if pRef is not None:
                newnode.left=node.middle
                newnode.middle=node.right
                node.middle=pRef
        elif key<node.key2:
            pKey=key
            newnode.key1=node.key2
            if pRef is not None:
                newnode.left=Pref
                newnode.middle=node.right
        else:
            pKey=node.key2
            newnode.key1=key
            if pRef is not None:
                newnode.left=node.right
                newnode.middle=pRef
        node.key2=None
        return pKey,newnode
```

## 平衡查找树之红黑树（Red-Black Tree）
2-3查找树能保证在插入元素之后能保持树的平衡状态，最坏情况下即所有的子节点都是2-node，树的高度为lgn，从而保证了最坏情况下的时间复杂度。但是2-3树实现起来比较复杂，于是就有了一种简单实现2-3树的数据结构，即红黑树（Red-Black Tree）。

基本思想：红黑树的思想就是对2-3查找树进行编码，尤其是对2-3查找树中的3-nodes节点添加额外的信息。红黑树中将节点之间的链接分为两种不同类型，红色链接，他用来链接两个2-nodes节点来表示一个3-nodes节点。黑色链接用来链接普通的2-3节点。特别的，使用红色链接的两个2-nodes来表示一个3-nodes节点，并且向左倾斜，即一个2-node是另一个2-node的左子节点。这种做法的好处是查找的时候不用做任何修改，和普通的二叉查找树相同。

@7

红黑树的定义：
红黑树是一种具有红色和黑色链接的平衡查找树，同时满足：
- 红色节点向左倾斜
- 一个节点不可能有两个红色链接
- 整个树完全黑色平衡，即从根节点到所以叶子结点的路径上，黑色链接的个数都相同。

下图可以看到红黑树其实是2-3树的另外一种表现形式：如果我们将红色的连线水平绘制，那么他链接的两个2-node节点就是2-3树中的一个3-node节点了。
@8

红黑树的性质：整个树完全黑色平衡，即从根节点到所以叶子结点的路径上，黑色链接的个数都相同（2-3树的第2）性质，从根节点到叶子节点的距离都相等）。

复杂度分析：最坏的情况就是，红黑树中除了最左侧路径全部是由3-node节点组成，即红黑相间的路径长度是全黑路径长度的2倍。

下图是一个典型的红黑树，从中可以看到最长的路径(红黑相间的路径)是最短路径的2倍：
@9

红黑树的平均高度大约为`logn`。
下图是红黑树在各种情况下的时间复杂度，可以看出红黑树是2-3查找树的一种实现，它能保证最坏情况下仍然具有对数的时间复杂度。
@10

Python实现
```
#红黑树
from random import randint

RED = 'red'
BLACK = 'black'

class RBT:
    def __init__(self):
       # self.items = []
        self.root = None
        self.zlist = []

    def LEFT_ROTATE(self, x):
        # x是一个RBTnode
        y = x.right
        if y is None:
            # 右节点为空，不旋转
            return
        else:
            beta = y.left
            x.right = beta
            if beta is not None:
                beta.parent = x

            p = x.parent
            y.parent = p
            if p is None:
                # x原来是root
                self.root = y
            elif x == p.left:
                p.left = y
            else:
                p.right = y
            y.left = x
            x.parent = y

    def RIGHT_ROTATE(self, y):
        # y是一个节点
        x = y.left
        if x is None:
            # 右节点为空，不旋转
            return
        else:
            beta = x.right
            y.left = beta
            if beta is not None:
                beta.parent = y

            p = y.parent
            x.parent = p
            if p is None:
                # y原来是root
                self.root = x
            elif y == p.left:
                p.left = x
            else:
                p.right = x
            x.right = y
            y.parent = x

    def INSERT(self, val):

        z = RBTnode(val)
        y = None
        x = self.root
        while x is not None:
            y = x
            if z.val < x.val:
                x = x.left
            else:
                x = x.right

        z.PAINT(RED)
        z.parent = y

        if y is None:
            # 插入z之前为空的RBT
            self.root = z
            self.INSERT_FIXUP(z)
            return

        if z.val < y.val:
            y.left = z
        else:
            y.right = z

        if y.color == RED:
            # z的父节点y为红色，需要fixup。
            # 如果z的父节点y为黑色，则不用调整
            self.INSERT_FIXUP(z)

        else:
            return

    def INSERT_FIXUP(self, z):
        # case 1:z为root节点
        if z.parent is None:
            z.PAINT(BLACK)
            self.root = z
            return

        # case 2:z的父节点为黑色
        if z.parent.color == BLACK:
            # 包括了z处于第二层的情况
            # 这里感觉不必要啊。。似乎z.parent为黑色则不会进入fixup阶段
            return

        # 下面的几种情况，都是z.parent.color == RED:
        # 节点y为z的uncle
        p = z.parent
        g = p.parent  # g为x的grandpa
        if g is None:
            return
            #   return 这里不能return的。。。
        if g.right == p:
            y = g.left
        else:
            y = g.right

        # case 3-0:z没有叔叔。即：y为NIL节点
        # 注意，此时z的父节点一定是RED
        if y == None:
            if z == p.right and p == p.parent.left:
                # 3-0-0:z为右儿子,且p为左儿子，则把p左旋
                # 转化为3-0-1或3-0-2的情况
                self.LEFT_ROTATE(p)
                p, z = z, p
                g = p.parent
            elif z == p.left and p == p.parent.right:
                self.RIGHT_ROTATE(p)
                p, z = z, p

            g.PAINT(RED)
            p.PAINT(BLACK)
            if p == g.left:
                # 3-0-1:p为g的左儿子
                self.RIGHT_ROTATE(g)
            else:
                # 3-0-2:p为g的右儿子
                self.LEFT_ROTATE(g)

            return

        # case 3-1:z有黑叔
        elif y.color == BLACK:
            if p.right == z and p.parent.left == p:
                # 3-1-0:z为右儿子,且p为左儿子,则左旋p
                # 转化为3-1-1或3-1-2
                self.LEFT_ROTATE(p)
                p, z = z, p
            elif p.left == z and p.parent.right == p:
                self.RIGHT_ROTATE(p)
                p, z = z, p

            p = z.parent
            g = p.parent

            p.PAINT(BLACK)
            g.PAINT(RED)
            if p == g.left:
                # 3-1-1:p为g的左儿子，则右旋g
                self.RIGHT_ROTATE(g)
            else:
                # 3-1-2:p为g的右儿子，则左旋g
                self.LEFT_ROTATE(g)

            return


        # case 3-2:z有红叔
        # 则涂黑父和叔，涂红爷，g作为新的z，递归调用
        else:
            y.PAINT(BLACK)
            p.PAINT(BLACK)
            g.PAINT(RED)
            new_z = g
            self.INSERT_FIXUP(new_z)

    def DELETE(self, val):
        curNode = self.root
        while curNode is not None:
            if val < curNode.val:
                curNode = curNode.left
            elif val > curNode.val:
                curNode = curNode.right
            else:
                # 找到了值为val的元素,正式开始删除

                if curNode.left is None and curNode.right is None:
                    # case1:curNode为叶子节点：直接删除即可
                    if curNode == self.root:
                        self.root = None
                    else:
                        p = curNode.parent
                        if curNode == p.left:
                            p.left = None
                        else:
                            p.right = None

                elif curNode.left is not None and curNode.right is not None:
                    sucNode = self.SUCCESOR(curNode)
                    curNode.val, sucNode.val  = sucNode.val, curNode.val
                    self.DELETE(sucNode.val)

                else:
                    p = curNode.parent
                    if curNode.left is None:
                        x = curNode.right
                    else:
                        x = curNode.left
                    if curNode == p.left:
                        p.left = x
                    else:
                        p.right = x
                    x.parent = p
                    if curNode.color == BLACK:
                        self.DELETE_FIXUP(x)

                curNode = None
        return False

    def DELETE_FIXUP(self, x):
        p = x.parent
        # w:x的兄弟结点
        if x == p.left:
            w = x.right
        else:
            w = x.left

        # case1:x的兄弟w是红色的
        if w.color == RED:
            p.PAINT(RED)
            w.PAINT(BLACK)
            if w == p.right:
                self.LEFT_ROTATE(p)
            else:
                self.RIGHT_ROTATE(p)

        if w.color == BLACK:
            # case2:x的兄弟w是黑色的，而且w的两个孩子都是黑色的
            if w.left.color == BLACK and w.right.color == BLACK:
                w.PAINT(RED)
                if p.color == BLACK:
                    return
                else:
                    p.color = BLACK
                    self.DELETE_FIXUP(p)

            # case3:x的兄弟w是黑色的，而且w的左儿子是红色的，右儿子是黑色的
            if w.left.color == RED and w.color == BLACK:
                w.left.PAINT(BLACK)
                w.PAINT(RED)
                self.RIGHT_ROTATE(w)

            # case4:x的兄弟w是黑色的，而且w的右儿子是红
            if w.right.color == RED:
                p.PAINT(BLACK)
                w.PAINT(RED)
                if w == p.right:
                    self.LEFT_ROTATE(p)
                else:
                    self.RIGHT_ROTATE(p)

    def SHOW(self):
        self.DISPLAY1(self.root)
        return self.zlist

    def DISPLAY1(self, node):
        if node is None:
            return
        self.DISPLAY1(node.left)
        self.zlist.append(node.val)
        self.DISPLAY1(node.right)

    def DISPLAY2(self, node):
        if node is None:
            return
        self.DISPLAY2(node.left)
        print(node.val)
        self.DISPLAY2(node.right)

    def DISPLAY3(self, node):
        if node is None:
            return
        self.DISPLAY3(node.left)
        self.DISPLAY3(node.right)
        print(node.val)

class RBTnode:
    '''红黑树的节点类型'''
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.parent = None

    def PAINT(self, color):
        self.color = color

def zuoxuan(b, c):
    a = b.parent
    a.left = c
    c.parent = a
    b.parent = c
    c.left = b

if __name__ == '__main__':
    rbt=RBT()
    b = []

    for i in range(100):
        m = randint(0, 500)
        rbt.INSERT(m)
        b.append(m)

    a = rbt.SHOW()
    b.sort()
    equal = True
    for i in range(100):
        if a[i] != b[i]:
            equal = False
            break

    if not equal:
        print('wrong')
    else:
        print('OK!')
```

红黑树这种数据结构应用十分广泛，在多种编程语言中被用作符号表的实现，如：
- Java中的java.util.TreeMap,java.util.TreeSet；
- C++ STL中的：map,multimap,multiset；
- .NET中的：SortedDictionary,SortedSet 等。


## B树和B+树（B Tree/B+ Tree）
平衡查找树中的2-3树以及其实现红黑树。2-3树种，一个节点最多有2个key，而红黑树则使用染色的方式来标识这两个key。

在计算机科学中，B树（B-tree）是一种树状数据结构，它能够存储数据、对其进行排序并允许以`O(log n)`的时间复杂度运行进行查找、顺序读取、插入和删除的数据结构。B树，概括来说是一个节点可以拥有多于2个子节点的二叉查找树。与自平衡二叉查找树不同，B树为系统最优化大块数据的读和写操作。B-tree算法减少定位记录时所经历的中间过程，从而加快存取速度。普遍运用在数据库和文件系统。

B树定义：
B树可以看作是对2-3查找树的一种扩展，即他允许每个节点有M-1个子节点。
- 根节点至少有两个子节点；
- 每个节点有M-1个key，并且以升序排列；
- 位于M-1和M key的子节点的值位于M-1 和M key对应的Value之间；
- 非叶子结点的关键字个数=指向儿子的指针个数-1；
- 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且K[i] ；
- 其它节点至少有M/2个子节点；
- 所有叶子结点位于同一层；

下图是一个`M=4` 阶的B树:
@11

可以看到B树是2-3树的一种扩展，他允许一个节点有多于2个的元素。B树的插入及平衡化操作和2-3树很相似，这里就不介绍了。
下面是往B树中依次插入
6 10 4 14 5 11 15 3 2 12 1 7 8 8 6 3 6 21 5 15 15 6 32 23 45 65 7 8 6 5 4
的演示动画：
@g1

B树算法思想:
B-树的搜索，从根结点开始，对结点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的儿子结点；重复，直到所对应的儿子指针为空，或已经是叶子结点；

B树的特性:
1. 关键字集合分布在整颗树中；
2. 任何一个关键字出现且只出现在一个结点中；
3. 搜索有可能在非叶子结点结束；
4. 其搜索性能等价于在关键字全集内做一次二分查找；
5. 自动层次控制；

由于限制了除根结点以外的非叶子结点，至少含有M/2个儿子，确保了结点的至少利用率，其最底搜索性能为`O(LogN)`

Python实现
```
# -*- coding: UTF-8 -*-
# B树查找

class BTree:  #B树
    def __init__(self,value):
        self.left=None
        self.data=value
        self.right=None

    def insertLeft(self,value):
        self.left=BTree(value)
        return self.left

    def insertRight(self,value):
        self.right=BTree(value)
        return self.right

    def show(self):
        print(self.data)


def inorder(node):  #中序遍历：先左子树，再根节点，再右子树
    if node.data:
        if node.left:
            inorder(node.left)
        node.show()
        if node.right:
            inorder(node.right)


def rinorder(node):  #倒中序遍历
    if node.data:
        if node.right:
            rinorder(node.right)
        node.show()
        if node.left:
            rinorder(node.left)

def insert(node,value):
    if value > node.data:
        if node.right:
            insert(node.right,value)
        else:
            node.insertRight(value)
    else:
        if node.left:
            insert(node.left,value)
        else:
            node.insertLeft(value)



l=[88,11,2,33,22,4,55,33,221,34]
Root=BTree(l[0])
node=Root
for i in range(1,len(l)):
    insert(Root,l[i])

print("中序遍历（从小到大排序 ）")
inorder(Root)
print("倒中序遍历（从大到小排序）")
rinorder(Root)
```


B+树定义：
B+树是对B树的一种变形树，它与B树的差异在于：
- 非叶子结点的子树指针与关键字个数相同,有k个子结点的结点必然有k个关键码；
- 非叶子结点的子树指针`P[i]`，指向关键字值属于`(K[i], K[i+1])`的子树
- 非叶结点仅具有索引作用，跟记录有关的信息均存放在叶结点中。
- B-树是开区间
- 为所有叶子结点增加一个链指针；
- 所有关键字都在叶子结点出现
- 树的所有叶结点构成一个有序链表，可以按照关键码排序的次序遍历全部记录。


如下图，是一个B+树:
@12

下图是B+树的插入动画：
@g2

B+树算法思想:
B+的搜索与B-树也基本相同，区别是B+树只有达到叶子结点才命中（B-树可以在非叶子结点命中），其性能也等价于在关键字全集做一次二分查找；

B+树的特性:
1. 所有关键字都出现在叶子结点的链表中（稠密索引），且链表中的关键字恰好是有序的；
2. 不可能在非叶子结点命中；
3. 非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层；
4. 更适合文件索引系统；

B和B+树的区别在于，B+树的非叶子结点只包含导航信息，不包含实际的值，所有的叶子结点和相连的节点使用链表相连，便于区间查找和遍历。

B+ 树的优点在于：
- 由于B+树在内部节点上不好含数据信息，因此在内存页中能够存放更多的key。 数据存放的更加紧密，具有更好的空间局部性。因此访问叶子几点上关联的数据也具有更好的缓存命中率。
- B+树的叶子结点都是相链的，因此对整棵树的便利只需要一次线性遍历叶子结点即可。而且由于数据顺序排列并且相连，所以便于区间查找和搜索。而B树则需要进行每一层的递归遍历。相邻的元素可能在内存中不相邻，所以缓存命中性没有B+树好。

但是B树也有优点，其优点在于，由于B树的每一个节点都包含key和value，因此经常访问的元素可能离根节点更近，因此访问也更迅速。

下面是B 树和B+树的区别图：
@13

B/B+树常用于文件系统和数据库系统中，它通过对每个节点存储个数的扩展，使得对连续的数据能够进行较快的定位和访问，能够有效减少查找时间，提高存储的空间局部性从而减少IO操作。它广泛用于文件系统及数据库中，如：
- Windows：HPFS文件系统；
- Mac：HFS，HFS+文件系统；
- Linux：ResiserFS，XFS，Ext3FS，JFS文件系统；
- 数据库：ORACLE，MYSQL，SQLSERVER等中。

有关B/B+树在数据库索引中的应用，请看张洋的[MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)这篇文章。

## 树表查找总结：
二叉查找树平均查找性能不错，为`O(logn)`，但是最坏情况会退化为`O(n)`。在二叉查找树的基础上进行优化，我们可以使用平衡查找树。平衡查找树中的2-3查找树，这种数据结构在插入之后能够进行自平衡操作，从而保证了树的高度在一定的范围内进而能够保证最坏情况下的时间复杂度。但是2-3查找树实现起来比较困难，红黑树是2-3树的一种简单高效的实现，他巧妙地使用颜色标记来替代2-3树中比较难处理的3-node节点问题。红黑树是一种比较高效的平衡查找树，应用非常广泛，很多编程语言的内部实现都或多或少的采用了红黑树。
除此之外，2-3查找树的另一个扩展——B/B+平衡树，在文件系统和数据库系统中有着广泛的应用。

# 分块查找
分块查找又称索引顺序查找，它是顺序查找的一种改进方法。

算法思想：将n个数据元素"按块有序"划分为m块（m ≤ n）。每一块中的结点不必有序，但块与块之间必须"按块有序"；即第1块中任一元素的关键字都必须小于第2块中任一元素的关键字；而第2块中任一元素又都必须小于第3块中的任一元素...

@15


算法流程:
1. 先选取各块中的最大关键字构成一个索引表；
2. 查找分两个部分：先对索引表进行二分查找或顺序查找，以确定待查记录在哪一块中；
3. 在已确定的块中用顺序法进行查找。

复杂度分析:时间复杂度:`O(log(m)+N/m)`

# 哈希查找
哈希表就是一种以键-值(key-indexed) 存储数据的结构，只要输入待查找的值即key，即可查找到其对应的值。
哈希表（Hash）:
我们使用一个下标范围比较大的数组来存储元素。可以设计一个函数（哈希函数， 也叫做散列函数），使得每个元素的关键字都与一个函数值（即数组下标）相对应，于是用这个数组单元来存储这个元素；也可以简单的理解为，按照关键字为每一个元素"分类"，然后将这个元素存储在相应"类"所对应的地方。但是，不能够保证每个元素的关键字与函数值是一一对应的，因此极有可能出现对于不同的元素，却计算出了相同的函数值，这样就产生了"冲突"，换句话说，就是把不同的元素分在了相同的"类"之中。后面我们将看到一种解决"冲突"的简便做法。
总的来说，"直接定址"与"解决冲突"是哈希表的两大特点。

哈希函数的规则是：通过某种转换关系，使关键字适度的分散到指定大小的的顺序结构中，越分散，则以后查找的时间复杂度越小，空间复杂度越高。

算法思想：哈希的思路很简单，如果所有的键都是整数，那么就可以使用一个简单的无序数组来实现：将键作为索引，值即为其对应的值，这样就可以快速访问任意键的值。这是对于简单的键的情况，我们将其扩展到可以处理更加复杂的类型的键。

算法流程：
1. 用给定的哈希函数构造哈希表；
2. 根据选择的冲突处理方法解决地址冲突；
    常见的解决冲突的方法：拉链法和线性探测法。详细的介绍可以参见：[浅谈算法和数据结构: 十一 哈希表](https://www.cnblogs.com/yangecnu/p/Introduce-Hashtable.html)。
3. 在哈希表的基础上执行哈希查找。

哈希表是一个在时间和空间上做出权衡的经典例子。如果没有内存限制，那么可以直接将键作为数组的索引。那么所有的查找时间复杂度为`O(1)`；如果没有时间限制，那么我们可以使用无序数组并进行顺序查找，这样只需要很少的内存。哈希表使用了适度的时间和空间来在这两个极端之间找到了平衡。只需要调整哈希函数算法即可在时间和空间上做出取舍。

复杂度分析：
单纯论查找复杂度：对于无冲突的Hash表而言，查找复杂度为`O(1)`（注意，在查找之前我们需要构建相应的Hash表）。

Python实现
```
# 忽略了对数据类型，元素溢出等问题的判断。
 
class HashTable:
  def __init__(self, size):
    self.elem = [None for i in range(size)] # 使用list数据结构作为哈希表元素保存方法
    self.count = size # 最大表长
 
  def hash(self, key):
    return key % self.count # 散列函数采用除留余数法
 
  def insert_hash(self, key):
    """插入关键字到哈希表内"""
    address = self.hash(key) # 求散列地址
    while self.elem[address]: # 当前位置已经有数据了，发生冲突。
      address = (address+1) % self.count # 线性探测下一地址是否可用
    self.elem[address] = key # 没有冲突则直接保存。
 
  def search_hash(self, key):
    """查找关键字，返回布尔值"""
    star = address = self.hash(key)
    while self.elem[address] != key:
      address = (address + 1) % self.count
      if not self.elem[address] or address == star: # 说明没找到或者循环到了开始的位置
        return False
    return True
 
 
if __name__ == '__main__':
  list_a = [12, 67, 56, 16, 25, 37, 22, 29, 15, 47, 48, 34]
  hash_table = HashTable(12)
  for i in list_a:
    hash_table.insert_hash(i)
 
  for i in hash_table.elem:
    if i:
      print((i, hash_table.elem.index(i)), end=" ")
  print("\n")
 
  print(hash_table.search_hash(15))
  print(hash_table.search_hash(33))
```

我们在实际编程中存储一个大规模的数据，最先想到的存储结构可能就是map，也就是我们常说的KV pair，经常使用Python可能更有这种体会。使用map的好处就是，我们在后续处理数据处理时，可以根据数据的key快速的查找到对应的value值。map的本质就是Hash表，那我们在获取了超高查找效率的基础上，我们付出了什么？
Hash是一种典型以空间换时间的算法，比如原来一个长度为100的数组，对其查找，只需要遍历且匹配相应记录即可，从空间复杂度上来看，假如数组存储的是byte类型数据，那么该数组占用100byte空间。现在我们采用Hash算法，我们前面说的Hash必须有一个规则，约束键与存储位置的关系，那么就需要一个固定长度的hash表，此时，仍然是100byte的数组，假设我们需要的100byte用来记录键与位置的关系，那么总的空间为200byte,而且用于记录规则的表大小会根据规则，大小可能是不定的。

Hash算法和其他查找算法的性能对比：
@14


# 广度优先搜索（BFS）
图的遍历一般有两种算法，即广度优先和深度优先。其中广度优先搜索算法会从指定的第一个顶点开始遍历图，先访问其所有的相邻点，就像一次访问图的一层。换句话说，就是先宽后深地访问顶点。
如图
@16

# 深度优先搜索（DFS）
深度优先搜索算法会从第一个指定的顶点开始遍历图，沿着路径直到这条路径最后一个顶点，接着原路回退并探索下一条路径。换句话说，它是先深度后广度地访问顶点。
如图
@17


# 其他的一些代码片段
```
    function findMin(arr) {
      var min = arr[0];
      for (var i = 1; i < arr.length; ++i) {
        if (arr[i] < min) {
          min = arr[i];
        }
      }
      return min;
    }

    function findMax(arr) {
      var max = arr[0];
      for (var i = 1; i < arr.length; ++i) {
        if (arr[i] > max) {
          max = arr[i];
        }
      }
      return max;
    }

    //二分法查找有序数组
    function binSearch(arr, data) {
      var upperBound = arr.length - 1;
      var lowerBound = 0;
      while (lowerBound <= upperBound) {
        var mid = Math.floor((upperBound + lowerBound) / 2);
        if (arr[mid] < data) {
          lowerBound = mid + 1;
        } else if (arr[mid] > data) {
          upperBound = mid - 1;
        } else {
          return mid;
        }
      }
      return -1;
    }
    //统计查找出的数据的个数
    function count(arr, data) {
      var count = 0;
      var position = binSearch(arr, data);
      if (position > -1) {
        ++count;
        for (var i = position - 1; i > 0; --i) {
          if (arr[i] == data) {
            ++count;
          } else {
            break;
          }
        }
        for (var i = position + 1; i < arr.length; ++i) {
          if (arr[i] == data) {
            ++count;
          } else {
            break;
          }
        }
      }
      return count;
    }


//实现队列
    function Queue() {
        var items = [];
        // 向队列尾部添加一个（或多个）新的项
        this.enqueue = function(element){
            items.push(element);
        };
        // 移除队列的第一（即排在队列最前面的）项，并返回被移除的元素
        this.dequeue = function(){
            return items.shift();
        };
        // 返回队列中第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动
        this.front = function(){
            return items[0];
        };
        // 如果队列中不包含任何元素，返回 true ，否则返回 false
        this.isEmpty = function(){
            return items.length == 0;
        };
        //  清空队列
        this.clear = function(){
            items = [];
        };
        // 返回队列包含的元素个数，与数组的 length 属性类似
        this.size = function(){
            return items.length;
        };
        this.print = function(){
            console.log(items.toString());
        };
    }
```
















