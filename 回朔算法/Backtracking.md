# 概念
回溯是一种穷举，但与brute force有一些区别，回溯带了两点脑子的，并不多，brute force一点也没带。
* 第一点脑子是回溯知道回头；相反如果是brute force,发现走不通立刻跳下山摔死，换第二条命从头换一条路走。
* 第二点脑子是回溯知道剪枝；如果有一条岔路上放了一坨屎，那这条路我们不走，就可以少走很多不必要走的路。

还有一些爱混淆的概念：递归，回溯，DFS。

回溯是一种找路方法，搜索的时候走不通就回头换路接着走，直到走通了或者发现此山根本不通。

DFS是一种开路策略，就是一条道先走到头，再往回走一步换一条路走到头，这也是回溯用到的策略。在树和图上回溯时人们叫它DFS。

递归是一种行为，回溯和递归如出一辙，都是一言不合就回到来时的路，所以一般回溯用递归实现；当然也可以不用，用栈。

以下以回溯统称，因为这个词听上去很文雅。

# 识别回溯
判断回溯很简单，拿到一个问题，你感觉如果不穷举一下就没法知道答案，那就可以开始回溯了。

一般回溯的问题有三种：
* Find a path to success 有没有解
* Find all paths to success 求所有解
  * 求所有解的个数
  * 求所有解的具体信息    
* Find the best path to success 求最优解

理解回溯：给一堆选择, 必须从里面选一个. 选完之后我又有了新的一组选择. 
这个过程一遍又一遍地重复，直到你达到一个最终状态。如果你的选择顺序做得很好，那么你的最终状态就是目标状态；如果你没有，那就不是目标状态。

回溯可以抽象为一棵树，我们的目标可以是找这个树有没有good leaf，也可以是问有多少个good leaf，也可以是找这些good leaf都在哪，也可以问哪个good leaf最好，分别对应上面所说回溯的问题分类。

good leaf都在leaf上。good leaf是我们的goal state，leaf node是final state，是解空间的边界。

对于第一类问题(问有没有解)，基本都是长着个样子的，理解了它，其他类别迎刃而解：
```java
boolean solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node, return true
        else return false
    } else {
        for each child c of n {
            if solve(c) succeeds, return true
        }
        return false
    }
}
```
算法是用布尔函数来表示的。这对于理解算法是至关重要的。如果 `solve(n)`为真，那就意味着 节点 n 是解的一部分--也就是说，节点 n 是从根到目标节点的路径上的节点之一。我们说n是可解。如果 `solve(n)`是否的，那么就不存在包括n到任何目标节点的路径。

关于回溯的三种问题，模板略有不同，
* 第一种，返回值是true/false。
* 第二种，求个数，设全局counter，返回值是void；求所有解信息，设result，返回值void。
* 第三种，设个全局变量best，返回值是void。

第一种
```java
boolean solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node, return true
        else return false
    } else {
        for each child c of n {
            if solve(c) succeeds, return true
        }
        return false
    }
}
```

第二种：
```java
void solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node, count++, return;
        else return
    } else {
        for each child c of n {
            solve(c)
        }
    }
}
```

第三种：
```java
void solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node, update best result, return;
        else return
    } else {
        for each child c of n {
            solve(c)
        }
    }
}
```

[Backtracking - David Matuszek](https://www.cis.upenn.edu/~matuszek/cit594-2012/Pages/backtracking.html)

