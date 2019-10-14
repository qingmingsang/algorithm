# 查找算法
## 查找算法分类：
- 静态查找和动态查找；(注：静态或者动态都是针对查找表而言的。动态表指查找表中有删除和插入操作的表。)
- 无序查找和有序查找。
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

复杂度分析：　查找成功时的平均查找长度为：（假设每个数据元素的概率相等） `ASL = 1/n(1+2+3+…+n) = (n+1)/2 `;
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
```

## 二分查找
说明：元素必须是有序的，如果是无序的则要先进行排序操作。

基本思想：也称为是折半查找，属于有序查找算法。用给定值k先与中间结点的关键字比较，中间结点把线形表分成两个子表，若相等则查找成功；若不相等，再根据k与该中间结点关键字的比较结果确定下一步查找哪个子表，这样递归进行，直到查找到或查找结束发现表中没有这样的结点。

复杂度分析：最坏情况下，关键词比较次数为`log2(n+1)`，且期望时间复杂度为`O(log2n)`；

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







