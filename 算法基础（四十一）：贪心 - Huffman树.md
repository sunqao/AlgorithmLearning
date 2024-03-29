# 算法基础（四十一）：贪心 - Huffman树

Huffman树是数据结构中一个非常经典的知识，同时它的构造过程是非常典型的贪心策略，本文主要来证明这个贪心策略为什么是正确的，关于构造的细节这里不再赘述；不过首先，我们还是先明确一些概念：

**权值：**

指的是树的每个结点对应的值，在最开始的时候是这些需要合并的结点的值，如下图所示，树的权值是树的根节点的权值：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215213005190.png" alt="image-20231215213005190" style="zoom:67%;" />

**合并代价：**

合并两个子树的代价是子树的根节点的权值和，在最开始合并的时候，比如合并上图中的`A, B`结点，代价就是`9`，合并合并所有子树的代价之和就是合并这颗树的代价，同时也是这个树的所有非叶子结点的结点权值之和，注意根节点的权值是所有叶子结点的权值之和，不管怎么合并根节点的权值是不变的

**带权路径长度:**

从树的根节点到结点`A`的路径长度 * 结点`A`的权值即为结点`A`的带权路径长度，一颗树的带权路径长度就是这颗树的所有叶子结点的带权路径长度之和，同时也是从所有零散的结点合并成这颗树的代价，上图的五个结点合并成一颗Huffman树如下图所示：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215214428777.png" alt="image-20231215214428777" style="zoom:67%;" />

这五个结点构成的带权路径长度最小的树称为Huffman树，也叫做最优二叉树，当然树的形态不唯一，下面来证明这个Huffman树为什么是最优的，也就是按照每次合并两个根节点权值最小子树的贪心策略为什么最后的代价是最小的

**证明：**

首先我们假设最后的树的形态是这样的，当然其他的形态也可以，不同的合并策略形成不同的树的形态，五个叶子结点是最开始的五个结点，这个树的构造过程也就是子树合并的过程

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215215107146.png" alt="image-20231215215107146" style="zoom: 50%;" />

以上面的图为例，因为最开始只有五个叶子结点，所以它从叶子结点开始的合并构造过程一定是下面这样的，合并结点构造树的过程一定是从最深的叶子结点开始：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215220127617.png" alt="image-20231215220127617" style="zoom: 33%;" />

`A1 ~ E1`是五个结点的位置，五个结点不同的排列方式构成了不同的合并方案，也构成了合并成最后这颗树不同的代价，假设这些不同的合并方案构成了一个集合，这个集合按照第一次是否合并最小权值的两个结点可划分为两个不同的子集，如下图所示，左侧代表第一次合并两个最小权值的结点的方案，右边是第一次不合并两个最小权值结点的两个方案：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215221113902.png" alt="image-20231215221113902" style="zoom:50%;" />

如果我们能证明最优方案一定在左侧集合中，那么每次合并完了之后会得到一个新的子树，将这个子树的根结点看作一个新的结点，与剩下的结点进行合并，这又是一个类似的子过程，并且这个子过程的所有合并方案与第一次合并最小权值结点的方案一一对应（上图集合左侧），假设f(n)表示初始合并方案的最小代价，f(n - 1)表示合并最小权值后子过程的最小代价，两者关系如下图所示：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/v2-af8c7623d2ecbb482ce03be44c69b96a_1440w.png" alt="img" style="zoom:50%;" />

两者就方案步骤来讲是一一对应完全等价的，只是代价不同，而由于是一个类似的子过程，所以最优方案仍然在每次合并两个最小权值结点的方案子集合中出现，这样递归下去就可以计算得到总的最优合并代价了

**下证：最优方案一定在左侧集合中**

对于第一次合并的时候不是两个最小权值结点的方案，假设第一次合并的是`A C`，最后计算整个树的代价的时候是`W1 = A*i + C*i + ... + B*k + ...`，其中`k <= i`，以上图中树的形状为例：`W1 = A*4 + C*4 + ... + B*k + ...`，`k < 4`

当然也可能是下图中的树的形状：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215225136985.png" alt="image-20231215225136985" style="zoom:50%;" />

假设第一次合并，那么代价就是：`W1 = A*3 + C*3 + ... + B*3 + ...`，`k = 3`，不过不管什么方案，第一次合并构造树一定是从最深的叶结点开始的。而且一定有：`W1 = A*i + C*i + ... + B*k + ...`，其中`k <= i`

对于这个集合右侧的方案，我们单单交换`B C`两个结点的位置得到一个集合左侧新的方案代价：`W2 = A*i + B*i + ... + C*k + ...`，`W2`与`W1`相比只有`B C`的位置不同其他结点权值完全一样，而又由于`B`的权值小于`C`，所以有`W1 - W2 = iC - iB + kB - kC = (i - k)(C - B) >= 0`，因此对于集合右侧的任意一个方案，总能找到集合左侧一个代价小于等于它的方案，所以最优方案一定出现在左侧集合中，所以合并结点，或者说构造Huffman树时每次合并最小权值的两个子树（结点）得到的一定是一颗最优二叉树（合并代价最小的树）

证毕。

**一个例题：**

![image-20231215225659567](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215225659567.png)

![image-20231215225709112](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20231215225709112.png)

**代码实现：**

```cpp
#include<iostream>
#include<algorithm>
#include<queue>
using namespace std;


int main(){
    int n;
    cin >> n;
    priority_queue<int, vector<int>, greater<int>> heap;//定义一个优先队列
    
    while(n --){
        int x;
        cin >> x;
        heap.push(x);//读入n个果子插入到堆里
    }
    
    int res = 0;
    
    while(heap.size() > 1){
        //取出最小权值的两个结点
        int x = heap.top();heap.pop();
        int y = heap.top();heap.pop();
        
        res += (x + y);
        heap.push(x + y);//加入到堆中
       // cout << x << "  " << y << endl;
    }
    
    cout << res;
    return 0;
    
}

```

