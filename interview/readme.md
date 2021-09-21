### Some knowledge about algorithm
I will put some links of blogs about the knowledge of each tag in leetcode here.

`像冒泡之类的各种基础算法，要用代码都能实现`

---

#### 栈
- [栈的java实现](https://www.cnblogs.com/fzz9/p/8167546.html#java%E5%AE%9E%E7%8E%B0)<br/>
- [单调栈问题（比当前元素小/大的前/后的一个元素）](https://blog.csdn.net/qq_17550379/article/details/86519771)<br/>
- [最小栈问题（常数时间内检索到最小元素的栈）](https://www.cnblogs.com/libai123456/p/10709310.html)<br/>
- [leetcode中关于栈和队列的题目的总结](https://zhuanlan.zhihu.com/p/61354685)<br/>
- [java官方stack的api](https://docs.oracle.com/javase/6/docs/api/java/util/Stack.html)<br/>
- 引入栈：`import java.util.Stack;`。java.util是java常用的工具包，链表等也可以从里面引入！。像`牛客网`这样要自己引包的一定要记住包！


#### 队列
- [leetcode中队列题讲解](https://blog.csdn.net/llz62378/article/details/78324379)
- [leetcode中关于栈和队列的题目的总结](https://zhuanlan.zhihu.com/p/61354685)<br/>
- [queue用java进行底层实现](https://www.cnblogs.com/liaohai/p/6523013.html)<br/>
- [java用类实现queue1](https://www.cnblogs.com/K-artorias/p/8143812.html)<br/>
- [java用类实现queue2](https://www.cnblogs.com/lemon-flm/p/7877898.html)<br/>
- [java用类实现queue2](https://www.runoob.com/java/data-queue.html)<br/>

#### 链表
- [链表的基础知识](https://blog.csdn.net/Shuffle_Ts/article/details/95055467)
- [链表的java底层实现，和一些操作](https://www.cnblogs.com/llfy/p/9395936.html)
- [Java官方列表类LinkedList解析](https://baijiahao.baidu.com/s?id=1598633392064956200&wfr=spider&for=pc)

#### 二分查找（binary search）
- 知识概要：
  - 时间复杂度o（logn）
  - 常用于待查数所在数组有序的时候
- 一些链接：
  - [二分查找的基础知识和题目范例](https://www.jianshu.com/p/3bdb7c29ab32)
  
#### 动态规划（dinamic programming）

- [链接：](https://www.bilibili.com/video/av16544031?from=search&seid=12793093025167879759)<br/>
  - 把一个问题化成他的子问题去解决（就像递归，如求斐波那契数列）
  - 动态规划常常就是求怎么得到最大值。
  - 有的时候递归很占内存，因为他会在不同地方重复计算某个被递归项(overlapping subproblems：重叠子问题）。所以我们需要把已经计算好的递归项保存起来（空间换时间），省的每次都算。<br/>
  - 抓住：选/不选 这个概念。一个情况下，选他的话，后续会有那些情况，不选他的话，后续哪些情况。这样，可以自底向上，先把最初始的情况选择了，然后保存选择，慢慢往上走，最后得到最高级那层时应该怎么选（比如视频的例子：选择工作项）<br/>
  
- [链接：](https://www.bilibili.com/video/av18512769/?spm_id_from=trigger_reload)<br/>
  - 递归写法看着直观，本质上也和动态规划类似。但是常常因为重叠子问题导致计算太慢。所以我们要使用一个数组保存已经计算的值，避免反复计算，从而实现最完美的动态规划。
  - 自顶向下的递归一般不叫动态规划（虽然up主用数组把值保存了，使得更接近动态规划了，因为动态规划的一个特点就是不必反复计算）。反而是用循环自底向上的算法才是正规的动态规划，因为底层的值都计算好了，算高层时需要的底层的值都已经有了，就都不用重复计算了。递归的出口反而是动态规划的入口。
  - 递归vs迭代：递归是函数调用自己（如斐波那契数列），迭代是本次迭代的运算结果成为下一轮迭代的初始值。
  - 贪心算法vs动态规划。贪心：目光短浅，结果不一定最优，当前状况下，每次都选取当前状态下最优的解。动态规划：结果最优，依据前一个状态，在状态改变的时候计算变迁。<br/>
  
- [动态规划以及例题，详细，共七集](https://www.bilibili.com/video/av95895415?p=1)<br/>

- backtracking
  - backtracking的经典例题是lc22，39，46。算法末班可以参考lc46的v1。
  - [这个人在lc中贴出了lc中backtracking题的汇总和解法，可以参考](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning))


#### 树
- [二叉树的前序中序后序遍历](https://blog.csdn.net/weixin_41316824/article/details/88721467)，还不懂可以去b站看视频讲解
- [树的数据结构知识概要](https://blog.csdn.net/wannuoge4766/article/details/83998377)
- [leetcode中关于树的题目](https://www.cnblogs.com/liuzhen1995/p/11921771.html)，这是第一部分，文章滑到结尾有第二和第三部分的链接
- [递归和迭代的区别](https://www.jianshu.com/p/32bcc45efd32)
- 平衡二叉树是指，在搭建二叉搜索树时，第一次出现一个节点的某边子树与另一边的子树的高度差超过1，则此时的树就已经不是平衡二叉（搜索）树了。要通过左旋或者右旋把它变成平衡的。这样在二分检索的时候，效率才高。<br/>
[戳我](https://baijiahao.baidu.com/s?id=1646617486319372351&wfr=spider&for=pc)
- 大佬语录：树问题中，不需要考虑整个树时怎样的，只需要考虑“在这个节点上应该发生什么我才能解决这个问题。”

#### 贪婪
- [贪心算法简介](https://www.cnblogs.com/aerer/p/9931068.html)
- [leetcode中贪心算法的应用](https://blog.csdn.net/qq_43699339/article/details/90146817)
- 思想：每次都选取当前最优，最后可以推导成整体最优时，才能使用贪心算法。

#### 双指针问题
> 我还没有写easy的题作为知识积累，所以这双指针问题的知识点必须重视
- [leetcode中双指针问题汇总](https://blog.csdn.net/pushup8/article/details/85071735)
  - 快慢指针，前后置镇，倒序指针
- 二分查找核心思想
  - 必须保证搜索空间在每轮迭代（interation）后都会减小
  - 当我们改变left和right时，我们必须保证target（存在的话）不会被意外的遗漏

#### 旋转数组的通用方法
- 代码：<br/>
```java
here give a common method to solve the image rotation problems.

/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/
void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```
- 例题见lc48


#### 字符串
- `substring（beginindex（含））`或`substring（beginindex（含），endindex（不含））`可以方便得提取子字符串。
