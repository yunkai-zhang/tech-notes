## 基础知识

#### 类型转换

1，包装类 基本数据类型 String 的转换：

- [1](https://blog.csdn.net/HY845638534/article/details/84669490)

## 栈队列

### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

方法一：双栈

#### 官方-思路和算法

维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

- 成员变量
  - 维护两个栈 stack1 和 stack2，其中 stack1 支持插入操作，stack2 支持删除操作

- 构造方法
  - 初始化 stack1 和 stack2 为空

- 插入元素

  插入元素对应方法 appendTail

  - stack1 直接插入元素

- 删除元素

  删除元素对应方法 deleteHead

  - 如果 stack2 为空，则将 stack1 里的所有元素弹出插入到 stack2 里

  - 如果 stack2 仍为空，则返回 -1，否则从 stack2 弹出一个元素并返回

#### 代码

```java
class CQueue {
    //两个栈，一个出栈，一个入栈
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;
    
    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        if(!stack2.isEmpty()){
            return stack2.pop();
        }else{
            while(!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
            return stack2.isEmpty() ? -1 : stack2.pop();
        }
    }
}
```

- 网友问：“这个方式为何只打败50%的提交？”。网友答：“因为Stack的实现是线程安全的，性能比LinkedList低”。

#### 复杂度分析

- 时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)O(1)。插入不多说，对于删除操作，虽然看起来是 O(n)O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)O(1)。

- 空间复杂度：O(n)O(n)。需要使用两个栈存储已有的元素。

#### 思考

- 20220302：
  - 不记得java中stack怎么实现
  - 知道用两个栈实现队列。但是针对删除，想的是“插入队列移动到删除队列，删除元素后，要把删除队列的所有元素移动回插入队列”，但是实际上不需要把删除队列的元素移动回去；
    - 正确思路就是如题解：删除队列负责删除，插入队列负责插入，删除队列空了才能把元素从插入队列移入删除队列，如果删除队列和插入队列同时为空则返回-1.

#### 再战

1,20220323，调试多次后成功：

```java
class CQueue {

    Stack stackIn=null;
    Stack stackOut=null;


    public CQueue() {
        stackIn=new Stack<>();
        stackOut=new Stack<>();
    }
    
        public void appendTail(int value) {
        //如果是传入integer类型，而非int的话，貌似会给integer再包裹一层，导致异常
        stackIn.push(value);
    }

    public int deleteHead() {
        //如果出栈不为空，直接pop一个就行。！！！包装类和基本数据类型的相互转换
        if(!stackOut.empty()){
            return (int) stackOut.pop();
        }
        //如果出栈为空，此时要删除节点,先就得把入栈的所有节点移动过来
        else{
            if(stackIn.empty()){
                //入栈也为空，说明整个逻辑队列为空，直接返回-1
                return -1;
            }else{
                //入栈不为空，把所有入栈元素转移到出栈；并弹出出栈的顶部。
                while(!stackIn.empty()){
                    stackOut.push(stackIn.pop());
                }
                return (int)stackOut.pop();
            }

        }

    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

2,20220323，改进代码：

```java
class CQueue {

    Stack <Integer> stackIn=null;
    Stack <Integer> stackOut=null;


    public CQueue() {
        stackIn=new Stack<>();
        stackOut=new Stack<>();
    }
    
        public void appendTail(int value) {
        //直接把int转化为Integer存入
        stackIn.push(Integer.valueOf(value));
    }

    public int deleteHead() {
        //如果出栈不为空，直接pop一个就行。！！！包装类和基本数据类型的相互转换
        if(!stackOut.empty()){
            return stackOut.pop().intValue();
        }
        //如果出栈为空，此时要删除节点,先就得把入栈的所有节点移动过来
        else{
            if(stackIn.empty()){
                //入栈也为空，说明整个逻辑队列为空，直接返回-1
                return -1;
            }else{
                //入栈不为空，把所有入栈元素转移到出栈；并弹出出栈的顶部。
                while(!stackIn.empty()){
                    stackOut.push(stackIn.pop());
                }
                //由于存的Integer对象，返回的时候要转化为int类型。
                return stackOut.pop().intValue();
            }

        }

    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

- 本代码因为主动包装int为Integer，性能更差了，但是更符合把对象存入栈的思想。
- `Stack <Integer> stackIn=new Stack<>();`，注意是在前面限定泛型的类型，后面的尖括号可留空由编译器自己推测；不正确指定泛型类型的话，会出现一些奇怪的包装错误，比如说期待包装成integer却包装为object。



### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

#### 首战告捷

```java
class MinStack {

    /** initialize your data structure here. */
    Stack<Integer> myStack=null;
    Stack<Integer> historicalMin=null;
    int min;
    public MinStack() {
        myStack=new Stack<>();
        historicalMin=new Stack<>();
        //不要忘记用引号结尾
        min=Integer.MAX_VALUE;
    }
    
    public void push(int x) {
        myStack.push(x);
        
        if(x<=min){
            min=x;
            historicalMin.push(min);
        }
    }
    
    public void pop() {
        int temp=myStack.pop();

        //不要误用=判断相等
        if(temp==min){
            historicalMin.pop();
            //如果两个栈都为空了，此时再直接peek会抛出异常；要判断hostoricalMin是否为空
            if(!historicalMin.empty()){
                min=historicalMin.peek();
            }else{
                min=Integer.MAX_VALUE;
            }
            
        }

    }
    
    public int top() {
        return myStack.peek();
    }
    
    public int min() {
        return min;

    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

- 执行后，据提示处理了几个语法错误；提交一次后，处理了一个逻辑漏洞；第二次提交完成。
- 待改进：
  - 读了题解后，我发现不该在hostoricalMin为空时，把intgermax当做min；而应该在push时，发现historicalMin为空就直接压入当前被push的元素，这样就不存在提前一步设置min的问题。
  - 基本数据类型min可以不设置，直接用stack.peek()就行。

---

#### 官方-解题思路
普通栈的 push() 和 pop() 函数的复杂度为 O(1)O(1) ；而获取栈最小值 min() 函数需要遍历整个栈，复杂度为 O(N)O(N) 。

本题难点： 将 min() 函数复杂度降为 O(1)O(1) ，可通过建立辅助栈实现；

- 数据栈 AA ： 栈 AA 用于存储所有元素，保证入栈 push() 函数、出栈 pop() 函数、获取栈顶 top() 函数的正常逻辑。
- 辅助栈 BB ： 栈 BB 中存储栈 AA 中所有 非严格降序 的元素，则栈 AA 中的最小元素始终对应栈 BB 的栈顶元素，即 min() 函数只需返回栈 BB 的栈顶元素即可。

因此，只需设法维护好 栈 BB 的元素，使其保持非严格降序，即可实现 min() 函数的 O(1)O(1) 复杂度。

#### 函数设计

- push(x) 函数： 重点为保持栈 BB 的元素是 非严格降序 的。

  - 将 xx 压入栈 AA （即 A.add(x) ）；

  - 若 ① 栈 BB 为空 或 ② xx 小于等于 栈 BB 的栈顶元素，则将 xx 压入栈 BB （即 B.add(x) ）。

- pop() 函数： 重点为保持栈 A, BA,B 的 元素一致性 。

  - 执行栈 AA 出栈（即 A.pop() ），将出栈元素记为 yy ；

  - 若 yy 等于栈 BB 的栈顶元素，则执行栈 B 出栈（即 B.pop() ）。

- top() 函数： 直接返回栈 AA 的栈顶元素即可，即返回 A.peek() 。

- min() 函数： 直接返回栈 BB 的栈顶元素即可，即返回 B.peek() 。

#### 复杂度分析

- 时间复杂度 O(1) ： push(), pop(), top(), min() 四个函数的时间复杂度均为常数级别。
- 空间复杂度 O(N)： 当共有 N 个待入栈元素时，辅助栈 BB 最差情况下存储 N 个元素，使用 O(N) 额外空间。

#### 代码
Java 代码中，由于 Stack 中存储的是 int 的包装类 Integer ，因此需要使用 equals() 代替 == 来比较值是否相等。

```java
class MinStack {
    Stack<Integer> A, B;
    public MinStack() {
        A = new Stack<>();
        B = new Stack<>();
    }
    public void push(int x) {
        A.add(x);
        if(B.empty() || B.peek() >= x)
            B.add(x);
    }
    public void pop() {
        if(A.pop().equals(B.peek()))
            B.pop();
    }
    public int top() {
        return A.peek();
    }
    public int min() {
        return B.peek();
    }
}

```



## 链表

### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    //记录最大递归深度，以便各层实现逆序存入数组
    int deepMax;
    int[] array;//array在递归最底层被建立
    public int[] reversePrint(ListNode head) {
        /*
        注意数组在java中是引用传递的：https://blog.csdn.net/qq_43555323/article/details/84783750;所以所有递归层共享数组，但是递归深度参数不同。
        所以可以考虑递归+引用传递解决
        */
        return digui(head,0+1);
    }
    //形参deep为本层的深度，也即以本层为最后节点的话，数组的长度-1。
    public int[] digui(ListNode head,int deep){
        //如果传入的头节点不为空，说明可以继续往下递归；小心不能用head.next判断链表尾部,因为不知道给的head是否为空。
        if(head!=null){
            //先递归调用获得数组
            array = digui(head.next,deep+1);
            //再把当前元素加入数组对应的位置；
            array[deepMax-deep]=head.val;

            //别忘了if和else都得有return，否则走if的递归无法返回
            return array;

        }
        //如果节点为空，则本层无节点；且制造数组，数组大小为本层深度-1
        else{
            deepMax=deep-1;
            return new int[deep-1];
        }

    }
}
```

- 我和一些网友理解：递归本质就是栈的使用，隐式使用了JVM栈
- 我这方法比官方还好，没有占用额外空间；时间复杂度一致；最高赞非官方解用的也是递归。

---

#### 官方-使用栈

栈的特点是后进先出，即最后压入栈的元素最先弹出。考虑到栈的这一特点，使用栈将链表元素顺序倒置。从链表的头节点开始，依次将每个节点压入栈内，然后依次弹出栈内的元素并存储到数组中。

- 创建一个栈，用于存储链表的节点
- 创建一个指针，初始时指向链表的头节点
- 当指针指向的元素非空时，重复下列操作：
  - 将指针指向的节点压入栈内
  - 将指针移到当前节点的下一个节点
- 获得栈的大小 size，创建一个数组 print，其大小为 size
- 创建下标并初始化 index = 0
- 重复 size 次下列操作：
  - 从栈内弹出一个节点，将该节点的值存到 print[index]
  - 将 index 的值加 1
- 返回 print

#### 代码

```java

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<ListNode> stack = new Stack<ListNode>();
        ListNode temp = head;
        while (temp != null) {
            stack.push(temp);
            temp = temp.next;
        }
        int size = stack.size();
        int[] print = new int[size];
        for (int i = 0; i < size; i++) {
            print[i] = stack.pop().val;
        }
        return print;
    }
}

```

- 这种倒着处理的，就可以想到栈 或递归！

#### 复杂性分析

时间复杂度：O(n)。正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。

空间复杂度：O(n)。额外使用一个栈存储链表中的每个节点。



### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

#### 首战寄

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    /*
    反转，一开始想到要用栈结构，如用递归解决；但是递归返回的头结点是原节点的末尾没错，可是想把当前节点放到逆链表的尾部的话
    却不能直接通过头节点直接放置，因为头节点的后续永远都是倒数第二个节点，要多次next才能放到尾部，很麻烦。

    换种思路，在每层递归时，交换节点指向；并记录逆链表的头结点即可。
     */
    ListNode resultHead,resultTail,temp; 
    public ListNode reverseList(ListNode head) {
        //resultTail保存尾节点，最后才能让尾节点指向null
        resultTail=head;
        //翻转链表
        recur(head);
        //才能让翻转链表的尾节点指向null
        resultTail.next=null;

        return resultHead;
    }
    //因为逆链表末尾需要为null，而不是两个节点组成小环，必须把递归提出来
    public void recur(ListNode head){
        //如果传入节点为空，说明是空链表，直接退出函数，让reverseList返回空的resultHead节点
        if(head==null){
            //注意void不能有返回值，可以用return是本层递归退栈
            return;
        }
        //如果当前传入节点不为空，说明至少有一个节点
        else{
            //如果下一节点为空，说明当前节点是逆链表的头，用resultHead引用它，保存好逆链表的头；因为对象给方法时给的是地址，所以不同层的递归修改是互相影响的。此时不需要交换本节点与下一节点（毕竟下一节点为空）的指向。
            if(head.next==null){
                resultHead=head;
            }
            //如果下一节点不为空，交换当前节点和下一节点的指向
            else{
                //交换指向；一定要用temp，否则链表会在head.next后断开
                temp.next=head
                ???要多个指针
                temp=head.next.next;
                head.next.next=head;
                //保证逆链表的尾结点为null
                //head.next=null;
            }

        }
    }
}
```

- 想想用递归单指针，但是逻辑上没理清；尝试递归+双指针时就放弃了

- 网友：链表题必须画图

#### 官方-方法一：迭代

![image-20220325152101750](lcof.assets/image-20220325152101750.png)

在遍历链表时，将当前节点的 \textit{next}next 指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

复杂度分析

- 时间复杂度：O(n)，其中 n 是链表的长度。需要遍历链表一次。
- 空间复杂度：O(1)。

#### 官方-方法二：递归

递归版本稍微复杂一些，其关键在于反向工作。假设链表的其余部分已经被反转，现在应该如何反转它前面的部分？

假设链表为：

![image-20220325152508307](lcof.assets/image-20220325152508307.png)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}

```

复杂度分析:

- 时间复杂度：O(n)，其中 n 是链表的长度。需要对链表的每个节点进行反转操作。

- 空间复杂度：O(n)，其中 n 是链表的长度。空间复杂度主要取决于递归调用的栈空间，最多为 n 层。

#### 即时再战-成功

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    ListNode newHead;
    public ListNode reverseList(ListNode head) {
        //设置递归返回条件
        if(head==null||head.next==null){
            return head;
        }else{//只有下一个节点不为空，就让下一个节点指向本节点，本节点指向空（保证尾节点指向了null）
            newHead=reverseList(head.next);
            //必须把反转链表放在递归后面，才能利用退栈时的逆序
            head.next.next=head;
            head.next=null;
            return newHead;

        }

    }
}
```

- 还是用的递归法



### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

#### 首战寄

不知道怎么写

----

#### 官方-解题思路

普通链表的节点定义如下：

```java
// Definition for a Node.
class Node {
    int val;
    Node next;
    public Node(int val) {
        this.val = val;
        this.next = null;
    }
}

```

本题链表的节点定义如下：

```java
// Definition for a Node.
class Node {
    int val;
    Node next, random;
    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}

```

给定链表的头节点 head ，复制普通链表很简单，只需遍历链表，每轮建立新节点 + 构建前驱节点 pre 和当前节点 node 的引用指向即可。

本题链表的节点新增了 random 指针，指向链表中的 任意节点 或者 null 。这个 random 指针意味着在复制过程中，除了构建前驱节点和当前节点的引用指向 pre.next ，还要构建前驱节点和其随机节点的引用指向 pre.random 。

本题难点： 在复制链表的过程中构建新链表各节点的 random 引用指向:

```java
class Solution {
    public Node copyRandomList(Node head) {
        Node cur = head;
        Node dum = new Node(0), pre = dum;
        while(cur != null) {
            Node node = new Node(cur.val); // 复制节点 cur
            pre.next = node;               // 新链表的 前驱节点 -> 当前节点
            // pre.random = "???";         // 新链表的 「 前驱节点 -> 当前节点 」 无法确定
            cur = cur.next;                // 遍历下一节点
            pre = node;                    // 保存当前新节点
        }
        return dum.next;
    }
}
```

> 本文介绍 「哈希表」 ，「拼接 + 拆分」 两种方法。哈希表方法比较直观；拼接 + 拆分方法的空间复杂度更低。

#### 官方-方法一：哈希表

利用哈希表的查询特点，考虑构建 原链表节点 和 新链表对应节点 的键值对映射关系，再遍历构建新链表各节点的 next 和 random 引用指向即可。

算法流程：

1. 若头节点 head 为空节点，直接返回 null ；
2. 初始化： 哈希表 dic ， 节点 cur 指向头节点；
3. 复制链表：
   1. 建立新节点，并向 dic 添加键值对 (原 cur 节点, 新 cur 节点） ；
   2. cur 遍历至原链表下一节点；
4. 构建新链表的引用指向：
   1. 构建新节点的 next 和 random 引用指向；
   2. cur 遍历至原链表下一节点；
5. 返回值： 新链表的头节点 dic[cur] ；

复杂度分析：

- 时间复杂度 O(N ： 两轮遍历链表，使用 O(N)O(N) 时间。
- 空间复杂度 O(N)： 哈希表 dic 使用线性大小的额外空间。

代码：

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null) return null;
        Node cur = head;
        Map<Node, Node> map = new HashMap<>();
        // 3. 复制各节点，并建立 “原节点 -> 新节点” 的 Map 映射
        while(cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        // 4. 构建新链表的 next 和 random 指向
        while(cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        // 5. 返回新链表的头节点
        return map.get(head);
    }
}
```

#### 官方-方法二：拼接 + 拆分

考虑构建 原节点 1 -> 新节点 1 -> 原节点 2 -> 新节点 2 -> …… 的拼接链表，如此便可在访问原节点的 random 指向节点的同时找到新对应新节点的 random 指向节点。

算法流程：

1. 复制各节点，构建拼接链表:

   ![image-20220325163531591](lcof.assets/image-20220325163531591.png)

2. 构建新链表各节点的 random 指向：
   - 当访问原节点 cur 的随机指向节点 cur.random 时，对应新节点 cur.next 的随机指向节点为 cur.random.next 。
3. 拆分原 / 新链表：
   - 设置 pre / cur 分别指向原 / 新链表头节点，遍历执行 pre.next = pre.next.next 和 cur.next = cur.next.next 将两链表拆分开。
4. 返回新链表的头节点 res 即可。

复杂度分析：

- 时间复杂度 O(N)： 三轮遍历链表，使用 O(N) 时间。
- 空间复杂度 O(1) ： 节点引用变量使用常数大小的额外空间。

代码：

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null) return null;
        Node cur = head;
        // 1. 复制各节点，并构建拼接链表
        while(cur != null) {
            Node tmp = new Node(cur.val);
            tmp.next = cur.next;
            cur.next = tmp;
            cur = tmp.next;
        }
        // 2. 构建各新节点的 random 指向
        cur = head;
        while(cur != null) {
            if(cur.random != null)
                cur.next.random = cur.random.next;
            cur = cur.next.next;
        }
        // 3. 拆分两链表
        cur = head.next;
        Node pre = head, res = head.next;
        while(cur.next != null) {
            pre.next = pre.next.next;
            cur.next = cur.next.next;
            pre = pre.next;
            cur = cur.next;
        }
        pre.next = null; // 单独处理原链表尾节点
        return res;      // 返回新链表头节点
    }
}
```

#### 20220325 半寄

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        //处理空节点
        if(head==null)
            return head;

        //把新链表的每个节点，跟在老链表的每个节点后面
        Node cur=head;
        while(cur!=null){
            Node temp=new Node(cur.val);
            temp.next=cur.next;
            cur.next=temp;
            cur=cur.next.next;
        }
        
        //根据老链表的ramdon，设置新链表的random
        cur=head;
        while(cur!=null){
            //有些节点指向random，所以要小心random.next时发生空指针异常。不给新链表的random赋值的话，random就是默认的null。
            if(cur.random!=null)
                cur.next.random=cur.random.next;
            cur=cur.next.next;
        }

        //拆分两个链表
        cur=head;
        Node result=cur.next;
        while(cur!=null){
            //必须要临时辅助节点记录截断处
            Node temp=cur.next;
            cur.next=cur.next.next; 
            //！！！链表中小心深度next；本处可能temp为新链表的最后一个节点了，两重next会报错；推荐像标答那样，以后者为next终止条件
            if(temp.next!=null)
                temp.next=temp.next.next;
            cur=cur.next;
        }

        return result;
        
    }
}
```

- 思路有，但是在处理最后拆分的时候，没注意好next的深度。
  - 官方方法二拆分时，cur.next != null可以保证还有两个节点没拆分，但是会导致新旧两个链表最后一个节点黏在一起的情况，所以最后要单独让原链表的尾部为null。
  - 我的拆分时，不需要最后处理最后节点粘在一起的情况，但是每次迭代都要if判断比较耗时；
  - 总之缝合链表拆分时，以粘合处的后者为判断离开的标志为好，不容易空指针异常。



## 字符串

### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

#### 首战告捷

```java
class Solution {
    public String replaceSpace(String s) {
        
        String result="";

        //对字符串的每个字符进行处理。字符串长度用length()，数组长度用length。？？？size的大小包含空格吗
        for(int i=0;i<s.length();i++){
            //获取字符串形式表示的当前字符
            String temp=String.valueOf(s.charAt(i));
            //对空格进行替换。注意字符串不能用==判断地址相等推出内容相等，因为不同地址存的字符串的内容可能相等；而得用equals才能判断内容是否相等，String重写了equals。
            if(temp.equals(" ")){
                result+="%20";
            }else{
                result+=temp;
            }
        }

        return result;
    }
}
```

- 注意：字符串判断内容相等要用equals

- charAt可以得到字符串某位置的字符；String.valueOf()可以把字符转化为字符串存入堆中的字符串常量池。

----

#### 官方-遍历添加

在 Python 和 Java 等语言中，字符串都被设计成「不可变」的类型，即无法直接修改字符串的某一位字符，需要新建一个字符串实现。

算法流程：

1. 初始化一个 list (Python) / StringBuilder (Java) ，记为 res ；
2. 遍历列表 s 中的每个字符 c ：
   - 当 c 为空格时：向 res 后添加字符串 "%20" ；
   - 当 c 不为空格时：向 res 后添加字符 c ；
3. 将列表 res 转化为字符串并返回。

复杂度分析：

- 时间复杂度 O(N) ： 遍历使用 O(N) ，每轮添加（修改）字符操作使用 O(1)；
- 空间复杂度 O(N) ： Python 新建的 list 和 Java 新建的 StringBuilder 都使用了线性大小的额外空间。

```java

class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder();
        for(Character c : s.toCharArray())
        {
            if(c == ' ') res.append("%20");
            else res.append(c);
        }
        return res.toString();
    }
}

```



#### 官方-字符数组

由于每次替换从 1 个字符变成 3 个字符，使用字符数组可方便地进行替换。建立字符数组地长度为 s 的长度的 3 倍，这样可保证字符数组可以容纳所有替换后的字符。算法：

- 获得 s 的长度 length
- 创建字符数组 array，其长度为 length * 3
- 初始化 size 为 0，size 表示替换后的字符串的长度
- 从左到右遍历字符串 s
  - 获得 s 的当前字符 c
  - 如果字符 c 是空格，则令 array[size] = '%'，array[size + 1] = '2'，array[size + 2] = '0'，并将 size 的值加 3
  - 如果字符 c 不是空格，则令 array[size] = c，并将 size 的值加 1
- 遍历结束之后，size 的值等于替换后的字符串的长度，从 array 的前 size 个字符创建新字符串，并返回新字符串

代码：

```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
}

```

- 网友：这样不行吗😂`return s.replace(" ","%20");`?
  - 网友答：不行，这个只能替换第一个，应该用replaceAll
    - 网友反驳：不是哇，这个replace方法确实是替换了呀，你再去翻翻看文档
    - 网友：replace和replaceAll的区别是后者支持正则但是前者不支持，但是它们都是全部替换的

复杂性分析:

- 时间复杂度：O(n)。遍历字符串 `s` 一遍。
- 空间复杂度：O(n)。额外创建字符数组，长度为 `s` 的长度的 3 倍。



### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

#### 首战告捷

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        //保存前若干位字符
        String head="";
        //保存后若干位字符
        String tail="";

        //把前n位字符存入head
        for(int i=0;i<n;i++){
            head+=String.valueOf(s.charAt(i));
        }

        //把原字符串后面的部分取出来
        for(int i=n;i<s.length();i++){
            tail+=String.valueOf(s.charAt(i));
        }

        return tail+head;

    }
}
```

- “把字符串拆成单个字符，再把单个字符转化成单字符字符串，再操作字符串”；这种方法很容易，但是时间和空间复杂度比较大。



#### 官方-方法一：字符串切片

解题思路：

- 本题做法较多，本文主要介绍 “字符串切片” ， “列表遍历拼接” ， “字符串遍历拼接” 三种方法。
  由于本题的多解法涉及到了 字符串为不可变对象 的相关概念，导致效率区别较大。因此，单列一节 三种方法的效率分析 ，望对各位有所帮助。

方法一：字符串切片

- 应用字符串切片函数，可方便实现左旋转字符串。

- 获取字符串 s[n:]s[n:] 切片和 s[:n]s[:n] 切片，使用 "++" 运算符拼接并返回即可。

复杂度分析：

- 时间复杂度 O(N) ： 其中 N 为字符串 s的长度，字符串切片函数为线性时间复杂度（参考资料）；
- 空间复杂度 O(N)： 两个字符串切片的总长度为 N。

代码：

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        return s.substring(n, s.length()) + s.substring(0, n);
    }
}

```

- substring左包右不包



#### 官方-方法二：列表遍历拼接
若面试规定不允许使用 切片函数 ，则使用此方法。

算法流程：

1. 新建一个 list(Python)、StringBuilder(Java) ，记为 resres ；

2. 先向 resres 添加 “第 n + 1n+1 位至末位的字符” ；
3. 再向 resres 添加 “首位至第 nn 位的字符” ；
4. 将 resres 转化为字符串并返回。

复杂度分析：

- 时间复杂度 O(N)： 线性遍历 s并添加，使用线性时间；
- 空间复杂度 O(N)： 新建的辅助 res使用 O(N)大小的额外空间。

代码：

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder res = new StringBuilder();
        for(int i = n; i < s.length(); i++)
            res.append(s.charAt(i));
        for(int i = 0; i < n; i++)
            res.append(s.charAt(i));
        return res.toString();
    }
}
```

- 注意：StirngBuilder实例化的对象最后要用toString方法把自己转换成字符串。
- StirngBuilder可以直接append字符



#### 官方-方法三：字符串遍历拼接
若规定 Python 不能使用 join() 函数，或规定 Java 只能用 String ，则使用此方法。

此方法与 方法二 思路一致，区别是使用字符串代替列表。

复杂度分析：

- 时间复杂度 O(N)： 线性遍历 s并添加，使用线性时间；
- 空间复杂度 O(N) ： 假设循环过程中内存会被及时回收，内存中至少同时存在长度为 N和 N-1的两个字符串（新建长度为 N的 res需要使用前一个长度 N-1的 res），因此至少使用 O(N)的额外空间。

代码：

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        String res = "";
        for(int i = n; i < s.length(); i++)
            res += s.charAt(i);
        for(int i = 0; i < n; i++)
            res += s.charAt(i);
        return res;
    }
}
```

- 这个就是我首战的做法；不过我还把charAt转换成Stirng后，再用+=。



#### 三种官方算法的区别

详细分析请参考 Efficient String Concatenation in Python 。

以上三种方法的空间使用如下图所示。
以 Python 为例开展三种方法的效率测试，结论同样适用于 Java 等其他语言。

![Picture4.png](lcof.assets/ef68413b3366b97af3ed76037c6a9d1e40ac09c74fd6e5cb6d5173cbd7116beb-Picture4.png)



## 查找算法（简单）

### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

#### 首战告捷

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        /*
        "长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内",就是明示要用数组内容与数组下标关联。
         */

        //doc数组的每一位，记载了nums数组中各元素出现的频率；doc的下标和nums的值对应。数组中初始为全0。
        int[] doc=new int[nums.length];

        //遍历nums，nums中出现的值对应doc数组的下标，给对应下标存的值+1来记录频次。
        for(int i=0;i<nums.length;i++){
           doc[nums[i]]++; 
        }

        //把重复数字里，最小的那个返回
        for(int i=0;i<doc.length;i++){
            if(doc[i]>1){
                return i;
            }
        }

        //如果上面for循环没有返回，则说明数组中没有重复数字；返回-1表示报错。
        return -1;

    }
}
```

- 我这种方法，如果nums的内容很大，可能会超过int的范围，这样来说还是官方的更安全
  - int的取值范围为： -2^31——2^31-1，即-2147483648——2147483647。
  - 不过本题中的”限制“说2 <= n <= 100000，那么不会击穿。做题一定要看示例下的**“提示”**或**”限制“**

#### 官方-方法一：遍历数组
由于只需要找出数组中任意一个重复的数字，因此遍历数组，遇到重复的数字即返回。为了判断一个数字是否重复遇到，使用集合存储已经遇到的数字，如果遇到的一个数字已经在集合中，则当前的数字是重复数字。算法：

- 初始化集合为空集合，重复的数字 repeat = -1
- 遍历数组中的每个元素：
  - 将该元素加入集合中，判断是否添加成功
    - 如果添加失败，说明该元素已经在集合中，因此该元素是重复元素，将该元素的值赋给 repeat，并结束遍历
- 返回 repeat

代码：

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
```

- **利用了hashSet判断重复；同理可以利用hashmap判断个数**。

复杂性分析：

- 时间复杂度：O(n)。
  - 遍历数组一遍。使用哈希集合（HashSet），添加元素的时间复杂度为 O(1)，故总的时间复杂度是 O(n)。
- 空间复杂度：O(n)。不重复的每个元素都可能存入集合，因此占用 O(n)额外空间。



### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

#### 首战告捷-hash法

```java
class Solution {
    public int search(int[] nums, int target) {
        /*
        因为10的9次方，小于2*10的9次次方，所以nums存值的范围在整数范围内，但是用数组会有连续空间，浪费；用hashMap吧
        */

        //注意左边指定泛型的具体类型
        Map<Integer,Integer> map=new HashMap<>();

        //便利给定的数组
        for(int i=0;i<nums.length;i++){
            //如果已仅存在hash映射，给次数加一
            if(map.containsKey(nums[i])){
                map.put(nums[i],map.get(nums[i])+1);

            }else{//否则建立针对某数字和其数字的hash映射
                map.put(nums[i],1);
            }
        }

        //处理次数为0的请款
        return map.containsKey(target)?map.get(target):0;

    }
}
```

- 本题条件是“排序数组”，这个方法没用上这个条件；排序数组应该用双指针解决。

#### 首战告捷-双指针

```java
class Solution {
    public int search(int[] nums, int target) {
        //看到排序数组，所以目标数字是连续的；用双指针

        //处理特殊情况：空数组情况，否则nums[right]会超过数组边界
        if(nums.length==0){
            return 0;
        }

        //初始化双指针为数组两端
        int left=0;
        int right=nums.length-1;

        //只要两个指针没碰到目标数就一直循环
        while(true){
            if(nums[left]!=target){
                left++;
            }
            if(nums[right]!=target){
                right--;
            }

            //当left>right，或nums[left]==nums[right]!=target时直接返回0；当nums[left]==nums[right]==target时返回right-left+1
            if(left>right||((nums[left]==nums[right])&&(nums[right]!=target))){
                return 0;
            }else if((nums[left]==nums[right])&&(nums[right]==target)){
                return right-left+1;
            }
        }

    }
}
```

- 我：
  - 数组问题，小心自定义i时，在特殊边界条件下，i超出边界
  - 变量要一致，比如left不要写着写着就写成l。
- 网友：面试官出这题的话肯定是想让你写二分的啦 其他没用
  - 网友：我第一想法也是二分，但是我发现题目没给数组长度，求个长度不就已经 O(n)了吗
    - 网友反驳： 像java这种语言，数组的长度是编译的时候就生成好的，不需要实时算的；nums.length直接是数组长度，这是数组本身的属性
- 本题双指针，不如官方的2分效率高。

性能：

![image-20220326170601282](lcof.assets/image-20220326170601282.png)

#### 官方-二分法

解题思路：

排序数组中的搜索问题，首先想到 **二分法** 解决。

![image-20220326172735012](lcof.assets/image-20220326172735012.png)

算法解析：

![image-20220326172816408](lcof.assets/image-20220326172816408.png)

效率优化：

![image-20220326172840370](lcof.assets/image-20220326172840370.png)

复杂度分析：

- 时间复杂度 O(log N)： 二分法为对数级别复杂度。
- 空间复杂度 O(1) ： 几个变量使用常数大小的额外空间。

代码：

```java
class Solution {
    public int search(int[] nums, int target) {
        // 搜索右边界 right
        int i = 0, j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] <= target) i = m + 1;
            else j = m - 1;
        }
        int right = i;
        // 若数组中无 target ，则提前返回
        if(j >= 0 && nums[j] != target) return 0;
        // 搜索左边界 right
        i = 0; j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] < target) i = m + 1;
            else j = m - 1;
        }
        int left = j;
        return right - left - 1;
    }
}
```

- 可将 nums[m] = targetnums[m]=target 情况合并至其他两种情况中。



### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

#### 首战告捷

```java
class Solution {
    public int missingNumber(int[] nums) {
        /*
        可以二分查找，“l-r”不等于“nums[l]-nums[r]”的话，则确实的数字在lr之间。
        但是纯双指针会跳过目标值，使目标值不再lr之间；因为双指针规则中“好像”说l和r在修改时不能和m重合，只能越过。

        我先用on遍历的方法做了
        */

        //缺失值在[0,n-2]时
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=i)
                return i;
        }
        //缺失值是n-1时
        return nums.length;

    }
}
```

效果还行：

![image-20220327154126719](lcof.assets/image-20220327154126719.png)



#### 官方-二分法

解题思路：

![image-20220327155156544](lcof.assets/image-20220327155156544.png)

算法解析：

![image-20220327155218162](lcof.assets/image-20220327155218162.png)

复杂度分析：

![image-20220327155245698](lcof.assets/image-20220327155245698.png)

代码：

```java
class Solution {
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] == m) i = m + 1;
            else j = m - 1;
        }
        return i;
    }
}
```

- 我和官方和高赞网友：“**排序数组中的搜索问题，首先想到 二分法 解决**。”get一项新技能。

- 我：本题解的视频不错，很清晰，可以看看
- 我：官方算法在l==r的时候不退出，让它两再移动一步从而交叉夹住目标值，不错！



## 查找算法（中等）

### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

#### 首战寄

#### 官方-二叉搜索树法

由于给定的二维数组具备每行从左到右递增以及每列从上到下递增的特点，当访问到一个元素时，可以排除数组中的部分元素。

从二维数组的右上角开始查找。如果当前元素等于目标值，则返回 true。如果当前元素大于目标值，则移到左边一列。如果当前元素小于目标值，则移到下边一行。

可以证明这种方法不会错过目标值。如果当前元素大于目标值，说明当前元素的下边的所有元素都一定大于目标值，因此往下查找不可能找到目标值，往左查找可能找到目标值。如果当前元素小于目标值，说明当前元素的左边的所有元素都一定小于目标值，因此往左查找不可能找到目标值，往下查找可能找到目标值。

- 若数组为空，返回 false
- 初始化行下标为 0，列下标为二维数组的列数减 1
- 重复下列步骤，直到行下标或列下标超出边界
  - 获得当前下标位置的元素 num
  - 如果 num 和 target 相等，返回 true
  - 如果 num 大于 target，列下标减 1
  - 如果 num 小于 target，行下标加 1
- 循环体执行完毕仍未找到元素等于 target ，说明不存在这样的元素，返回 false`

代码：

```java

class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int row = 0, column = columns - 1;
        while (row < rows && column >= 0) {
            int num = matrix[row][column];
            if (num == target) {
                return true;
            } else if (num > target) {
                column--;
            } else {
                row++;
            }
        }
        return false;
    }
}

```

复杂度分析:

- 时间复杂度：O(n+m)。访问到的下标的行最多增加 n 次，列最多减少 m 次，因此循环体最多执行 n + m 次。
- 空间复杂度：O(1)。



#### 20220327成功

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {

        //处理空数组情况
        if(matrix.length==0){
            return false;
        }

        //处理非空数组，从数组的左下角开始做二叉搜索树式探索
        //初始化行列坐标；共n行m列
        int n=matrix.length-1,m=0,maxM=matrix[0].length-1;

        while(n>=0&&m<=maxM){
            if(matrix[n][m]<target){
                m++;
            }else if(matrix[n][m]>target){
                n--;
            }else{
                return true;
            }
        }

        return false;

    }
}
```



### [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

#### 首战半寄

```java
class Solution {
    public int minArray(int[] numbers) {

        //题目说n不为负数，但是numbers[n]可能是负数；那么这个last取0就会导致元素一旦有负数，就提前退出for循环
        //int last=0;
        int last=Integer.MIN_VALUE;
        for(int i=0;i<numbers.length;i++){
            //侦查到断崖就返回
            if(numbers[i]<last){
                return numbers[i];
            }
            last=numbers[i];
        }

        //没侦查到断崖，可能是数字全相同；或者经历了n次旋转成了没旋转前的升序状态：返回第一个数
        return numbers[0];
    }
}
```

- 思路有，但是把last设置为0忽略了数组元素为负数的情况，导致程序一碰到负数元素就以为碰到了断崖就退出循环并返回错误的值了；把last初始值换成最小整数就过了。
- 我反思：既然都线性遍历了，不如干脆用later记录碰到过的最小值，甚至都不用查看断崖；我相当于相同的时间复杂度能解决的问题，用更复杂的方式解决了，减分项。

#### 官方-二分查找

解题思路：

![image-20220327224636236](lcof.assets/image-20220327224636236.png)

算法流程：

![image-20220327224718445](lcof.assets/image-20220327224718445.png)

- 我：j=j-1后，最小值还在ij之间，因为：
  - 如果j是最小值，那么原本numsm==numsj，让j--后，至少m位置有最小值
  - j--相当于是为重复元素去重。

正确性证明：

![image-20220327224758158](lcof.assets/image-20220327224758158.png)

补充思考： 为什么本题二分法不用 nums[m]*n**u**m**s*[*m*] 和 nums[i]*n**u**m**s*[*i*] 作比较？

![image-20220327224829837](lcof.assets/image-20220327224829837.png)

复杂度分析：

![image-20220327224853507](lcof.assets/image-20220327224853507.png)

代码：

```java
class Solution {
    public int minArray(int[] numbers) {
        int i = 0, j = numbers.length - 1;
        while (i < j) {
            int m = (i + j) / 2;
            if (numbers[m] > numbers[j]) i = m + 1;
            else if (numbers[m] < numbers[j]) j = m;
            else j--;
        }
        return numbers[i];
    }
}
```

- 网友：为什么官方的二分法的题解很多都是写的`low + (high - low) / 2 `而不是 `(high + low) / 2`
  - 网友答：防止溢出吧。

### [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

#### 首战告捷

```java
class Solution {
    public char firstUniqChar(String s) {
        /*
        第一次重复。没有说有序，使用hashMap判断重复
         */
        Map<Character,Integer> map=new HashMap<>();
        //先做一次遍历，记录下各字符出现的频度
        for(int i=0;i<s.length();i++){
            char tempCh=s.charAt(i);
            if(map.containsKey(tempCh)){
                map.put(tempCh,map.get(tempCh)+1);
            }else{
                //基本数据类型会自动包装拆包
                map.put(tempCh,1);
            }
        }
        //查找第一个只出现一次的字符
        for(int i=0;i<s.length();i++){
            char tempCh=s.charAt(i);
            if(map.get(tempCh)==1){
                return tempCh;
            }
        }

        //一直没return 说明不存在字符
        return ' ';
    }
}
```

#### 网友1-数组记录

```java
public char firstUniqChar(String s) {
    if (s.equals("")) return ' ';
    //创建‘a'-'z'的字典
    int[] target = new int[26];
    //第一次遍历，将字符统计到字典数组
    for (int i = 0; i < s.length(); i++) {
        target[s.charAt(i) - 'a']++;
    }
    //第二次遍历，从字典数组获取次数
    for (int i = 0; i < s.length(); i++) {
        if (target[s.charAt(i) - 'a'] == 1) return s.charAt(i);
    }

    return ' ';
}
```

- 题干说`s 只包含小写字母`，我做题时认为，除了英文的26个字母可能还有别的小写字母，就用了hashMap；如果这里的小写字母只指英文26字母的话，网友1的做法确实更快更好；但是我看官方也没有使用26字母的特性，保险起见，还是别用网友1的方法。

#### 官方方法1-hash表

本题考察 哈希表 的使用，本文介绍 哈希表 和 有序哈希表 两种解法。其中，在字符串长度较大、重复字符很多时，“有序哈希表” 解法理论上效率更高。

1. 遍历字符串 `s` ，使用哈希表统计 “各字符数量是否 > 1>1 ”。
2. 再遍历字符串 `s` ，在哈希表中找到首个 “数量为 11 的字符”，并返回。

![Picture1.png](lcof.assets/ed093aabc9195caff6d088454eaebe3cad875e8ca4a643c004ef25e4e5e9e174-Picture1.png)

算法流程：

- 初始化： 字典 (Python)、HashMap(Java)、map(C++)，记为 dic ；
- 字符统计： 遍历字符串 s 中的每个字符 c ；
  - 若 dic 中 不包含 键(key) c ：则向 dic 中添加键值对 (c, True) ，代表字符 c 的数量为 11 ；
  - 若 dic 中 包含 键(key) c ：则修改键 c 的键值对为 (c, False) ，代表字符 c 的数量 > 1>1 。
- 查找数量为 11 的字符： 遍历字符串 s 中的每个字符 c ；
  - 若 dic中键 c 对应的值为 True ：，则返回 c 。
- 返回 ' ' ，代表字符串无数量为 11 的字符。

复杂度分析：

- 时间复杂度 O(N)： N为字符串 s 的长度；需遍历 s 两轮，使用 O(N)；HashMap 查找操作的复杂度为 O(1) ；
- 空间复杂度 O(1) ： 由于题目指出 s 只包含小写字母，因此最多有 26 个不同字符，HashMap 存储需占用 O(26) = O(1)的额外空间。

代码：

```java
class Solution {
    public char firstUniqChar(String s) {
        HashMap<Character, Boolean> dic = new HashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(char c : sc)
            if(dic.get(c)) return c;
        return ' ';
    }
}
```

- 我：这个解法和我的很像，只是它没有存储s中字符出现的具体数目，而是存储是否超过1个（布尔值）
  - 网友针对我的写法的评价：我觉得 Map 结构的 Value 使用 Boolean 类型是一个更好的选择，原因如下：
    1. 一个字母出现的次数大于 1 次就不符合要求了，这个时候使用 Fasle 标记状态相对于 Integer 的不断递增更合理，也更省空间。
    2. 布尔值可以用来判断，可以简化代码逻辑。

#### 官方方法2-有序hash表法

在哈希表的基础上，有序哈希表中的键值对是 **按照插入顺序排序** 的。基于此，可通过遍历有序哈希表，实现搜索首个 “数量为 11 的字符”。

哈希表是 **去重** 的，即哈希表中键值对数量 ≤ 字符串 s 的长度。因此，相比于方法一，方法二减少了第二轮遍历的循环次数。当字符串很长（重复字符很多）时，方法二则效率更高。

复杂度分析：

- 时间和空间复杂度均与 “方法一” 相同，而具体分析：方法一 需遍历 s 两轮；方法二 遍历 s 一轮，遍历 dic 一轮（ dic 的长度不大于 26 ）。

代码：

```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Boolean> dic = new LinkedHashMap<>();
        char[] sc = s.toCharArray();
        for(char c : sc)
            dic.put(c, !dic.containsKey(c));
        for(Map.Entry<Character, Boolean> d : dic.entrySet()){
           if(d.getValue()) return d.getKey();
        }
        return ' ';
    }
}
```

- Java 使用 LinkedHashMap 实现有序哈希表。

- 字符串转换为字符数组：`s.toCharArray();`

- [为什么要使用Map.Entry](https://www.cnblogs.com/a198720/articles/4212034.html)



## 搜索与回溯算法(简单)

### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

#### 首战半寄

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] levelOrder(TreeNode root) {
        /*
        从上往下，从左往右，这是bfs的特点。使用队列实现bfs：https://www.runoob.com/java/data-queue.html
        Queue接口要用linkedlist实现。
         */
        //处理特殊情况
        if(root==null)return new int[0];

         //存储结果
         ArrayList<Integer> arrayList=new ArrayList<>();
         //初始化队列
         Queue <TreeNode> queue=new LinkedList<>();
         queue.offer(root);

        //把头节点加入结果数组列表，并把左右子节点加入队列
         while(!queue.isEmpty()){
             TreeNode temp=queue.poll();
             arrayList.add(temp.val);
            if(temp.left!=null)
                queue.offer(temp.left);
            if(temp.right!=null)
                queue.offer(temp.right);
         }

         //把integer数组列表转换成int数组，这里只能for循环。https://blog.csdn.net/huanghanqian/article/details/73920439
         int[] result=new int[arrayList.size()];
         for(int i=0;i<arrayList.size();i++){
             //arraylist不能数组式访问，要用get，不能用arraylist[i]形式访问
             result[i]=arrayList.get(i).intValue();
         }
         return result;

    }
}
```

- 思路有，但是queue的使用，arraylist转array都是借助网络了，中间运行了几次。

- 我推测：单个int Integer互相转换可以自动装箱拆箱；但是int[]和Integer[]互相转换就不能直接装箱拆箱，而要对一个个元素手动/自动装箱拆箱；
  - 如本题目所示`result[i]=arrayList.get(i).intValue();`是手动拆箱，`result[i]=arrayList.get(i);`是自动拆箱，亲测都可以提交成功。

#### 官方-bfs

解题思路：

- 题目要求的二叉树的 **从上至下** 打印（即按层打印），又称为二叉树的 **广度优先搜索**（BFS）。
- BFS 通常借助 **队列** 的先入先出特性来实现。

![Picture0.png](lcof.assets/f824fdd8052ae4ee657365c98633480caf03c60e42e4661797618e318baf8664-Picture0.png)

算法流程：

1. 特例处理： 当树的根节点为空，则直接返回空列表 [] ；
2. 初始化： 打印结果列表 res = [] ，包含根节点的队列 queue = [root] ；
3. BFS 循环： 当队列 queue 为空时跳出；
   1. 出队： 队首元素出队，记为 node；
   2. 打印： 将 node.val 添加至列表 tmp 尾部；
   3. 添加子节点： 若 node 的左（右）子节点不为空，则将左（右）子节点加入队列 queue ；
4. 返回值： 返回打印结果列表 res 即可。

复杂度分析：

- 时间复杂度 O(N)O(N) ： NN 为二叉树的节点数量，即 BFS 需循环 NN 次。
- 空间复杂度 O(N)O(N) ： 最差情况下，即当树为平衡二叉树时，最多有 N/2N/2 个树节点同时在 queue 中，使用 O(N)O(N) 大小的额外空间。

代码：

```java
class Solution {
    public int[] levelOrder(TreeNode root) {
        if(root == null) return new int[0];
        Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
        ArrayList<Integer> ans = new ArrayList<>();
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            ans.add(node.val);
            if(node.left != null) queue.add(node.left);
            if(node.right != null) queue.add(node.right);
        }
        int[] res = new int[ans.size()];
        for(int i = 0; i < ans.size(); i++)
            res[i] = ans.get(i);
        return res;
    }
}
```

- 我：和我的代码基本一样。

### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        /*
        "I. 从上到下打印二叉树 类似，关键是怎么判断一层的范围
        打算使用两个队列，cur和next
         */
         //处理特殊情况
         if(root==null) return new ArrayList<>();

        Queue<TreeNode> cur =new LinkedList<>();
        Queue<TreeNode> next =new LinkedList<>();
        List<List<Integer>> result=new ArrayList<>();//ArrayList底层是数组，查询快、增删慢；LinkedList底层是链表，查询慢、增删快。

        cur.offer(root);
        do{
            //用于存储接下来的链表中的节点值
            List<Integer> tempList=new ArrayList<>();
            while(!cur.isEmpty()){
            //处理当前队列中的每个节点
            TreeNode temp=cur.poll();
            tempList.add(temp.val);
            if(temp.left!=null) next.offer(temp.left);
            if(temp.right!=null) next.offer(temp.right);
            }
            //把一个队列处理完后得到的列表，存入结果列表
            result.add(tempList);

            //cur处理完了，交换cur和next；让cur变成存了节点的队列，next变成空队列
            Queue<TreeNode> tempQueue=cur;
            cur=next;
            next=tempQueue;

        }while(!cur.isEmpty());//这里判断当前队列为空，实际上就是判断上一轮队列处理后，还有没有节点要处理

        return result;

    }
}
```

- 思路和知识点都知道，有些小语法错误，借助“执行代码”改了下，就成功了。

#### 官方-BFS

解题思路：

> 建议先做 面试题32 - I. 从上到下打印二叉树 再做此题，两题仅有微小区别，即本题需将 **每一层打印到一行** 。

I. 按层打印： 题目要求的二叉树的 从上至下 打印（即按层打印），又称为二叉树的 广度优先搜索（BFS）。BFS 通常借助 队列 的先入先出特性来实现。

II. 每层打印到一行： 将本层全部节点打印到一行，并将下一层全部节点加入队列，以此类推，即可分为多行打印。

![Picture1.png](lcof.assets/59e0600588ffdc2f34b4b563193b56c1f678743637e2754e2a9be0e7facc5d48-Picture1.png)

算法流程：

1. 特例处理： 当根节点为空，则返回空列表 [] ；
2. 初始化： 打印结果列表 res = [] ，包含根节点的队列 queue = [root] ；
3. BFS 循环： 当队列 queue 为空时跳出；
   1. 新建一个临时列表 tmp ，用于存储当前层打印结果；
   2. 当前层打印循环： 循环次数为当前层节点数（即队列 queue 长度）；
      1. 出队： 队首元素出队，记为 node；
      2. 打印： 将 node.val 添加至 tmp 尾部；
      3. 添加子节点： 若 node 的左（右）子节点不为空，则将左（右）子节点加入队列 queue ；
   3. 将当前层结果 tmp 添加入 res 。
4. 返回值： 返回打印结果列表 res 即可。

复杂度分析：

- 时间复杂度 O(N) ： N为二叉树的节点数量，即 BFS 需循环 N 次。
- 空间复杂度 O(N) ： 最差情况下，即当树为平衡二叉树时，最多有 N/2 个树节点同时在 queue 中，使用 O(N)大小的额外空间。

代码：

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}

```

- 网友：还在思考怎么分层打印，博主这个len(queue)的操作太6了
  - 我理解：这说的len(queue)就是i = queue.size()



### [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

#### 首战半寄

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        /*
        总是使用deque不会错，它也代替了Stack：https://blog.csdn.net/devnn/article/details/82716447
         */

         //创建双端队列;默认节点last进first出;注意用LinkedList实现Deque！！！
         Deque <TreeNode> deque=new LinkedList<>();
         Deque <TreeNode> deque2=new LinkedList<>();
         if(root!=null) deque.offerLast(root);

         List<List<Integer>> resultList=new ArrayList<>();

         //决定是否从左(first)往右（last）打印节点;第一行从左往右，所以初始true
         boolean left2right=true;

         while(!deque.isEmpty()){
             List<Integer> tempList=new ArrayList();
             //不能在for中判断size，因为加了子节点size就变了
             int dequeSize=deque.size();
             for(int i=0;i<dequeSize;i++){
                 //从左往右打印节点，就先放左子节点进队列(默认都从尾部入)
                if(left2right){
                    TreeNode temp=deque.pollLast();
                    tempList.add(temp.val);
                    //!!!千万不要把null存入队列，它会占用队列的一个位置，且null.val null.left都会导致错误
                    if(temp.left!=null)deque2.offerLast(temp.left);
                    if(temp.right!=null)deque2.offerLast(temp.right);

                }else{//从右往左打印节点，就先放右子节点进队列(默认都从尾部入)
                    TreeNode temp=deque.pollLast();
                    tempList.add(temp.val);
                    if(temp.right!=null)deque2.offerLast(temp.right);
                    if(temp.left!=null)deque2.offerLast(temp.left);

                }
             }
             //更改走向标志，换一个方向的子节点先入队列
             left2right=!left2right;
             resultList.add(tempList);

             Deque <TreeNode> tempDeque=deque;
             deque=deque2;
             deque2=tempDeque;
         }

         return resultList;
    }
}
```

- 做的时候各种bug，和逻辑小错误，最后用idea一下下debug弄出来了
- 应该是用来官方方法2，但是确实绕。

#### 官方1-层序遍历 + 双端队列

思想：

- 利用双端队列的两端皆可添加元素的特性，设打印列表（双端队列） tmp ，并规定：
  - 奇数层 则添加至 tmp 尾部 ，
  - 偶数层 则添加至 tmp 头部 。

算法流程：

1. 特例处理： 当树的根节点为空，则直接返回空列表 [] ；
2. 初始化： 打印结果空列表 res ，包含根节点的双端队列 deque ；
3. BFS 循环： 当 deque 为空时跳出；
   1. 新建列表 tmp ，用于临时存储当前层打印结果；
   2. 当前层打印循环： 循环次数为当前层节点数（即 deque 长度）；
      1. 出队： 队首元素出队，记为 node；
      2. 打印： 若为奇数层，将 node.val 添加至 tmp 尾部；否则，添加至 tmp 头部；
      3. 添加子节点： 若 node 的左（右）子节点不为空，则加入 deque ；
   3. 将当前层结果 tmp 转化为 list 并添加入 res ；
4. 返回值： 返回打印结果列表 res 即可；

复杂度分析：

- 时间复杂度 O(N) ： N为二叉树的节点数量，即 BFS 需循环 N次，占用 O(N) ；双端队列的队首和队尾的添加和删除操作的时间复杂度均为 O(1) 。
- 空间复杂度 O(N) ： 最差情况下，即当树为满二叉树时，最多有 N/2 个树节点 同时 在 deque 中，使用 O(N)大小的额外空间。

代码：

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            LinkedList<Integer> tmp = new LinkedList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                if(res.size() % 2 == 0) tmp.addLast(node.val); // 偶数层 -> 队列头部
                else tmp.addFirst(node.val); // 奇数层 -> 队列尾部
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}
```

- Java 中将链表 LinkedList 作为双端队列使用，LinkedList 可以用addLast和addFirst从链状列表的两端插入。
- 注意：我` for(int i = 0; i <queue.size(); i++) `，会导致往队列加元素后队列长度变化，导致的无法按层输出。但是题解中` for(int i = queue.size(); i > 0; i--)`的写法，就保证一层级的节点数不会被后续加入队列的节点所影响。

#### 官方2-层序遍历 + 双端队列（奇偶层逻辑分离）
- 方法一代码简短、容易实现；但需要判断每个节点的所在层奇偶性，即冗余了 NN 次判断。
- 通过将奇偶层逻辑拆分，可以消除冗余的判断。

算法流程：

![image-20220329165225545](lcof.assets/image-20220329165225545.png)

- 与方法一对比，仅 BFS 循环不同。

- BFS 循环： 循环打印奇 / 偶数层，当 deque 为空时跳出；
  1. 打印奇数层： 从左向右 打印，先左后右 加入下层节点；
  2. 若 deque 为空，说明向下无偶数层，则跳出；
  3. 打印偶数层： 从右向左 打印，先右后左 加入下层节点；

复杂度分析：

- 时间复杂度 O(N) ： 同方法一。
- 空间复杂度 O(N)： 同方法一。

代码：

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Deque<TreeNode> deque = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) deque.add(root);
        while(!deque.isEmpty()) {
            // 打印奇数层
            List<Integer> tmp = new ArrayList<>();
            for(int i = deque.size(); i > 0; i--) {
                // 从左向右打印
                TreeNode node = deque.removeFirst();
                tmp.add(node.val);
                // 先左后右加入下层节点
                if(node.left != null) deque.addLast(node.left);
                if(node.right != null) deque.addLast(node.right);
            }
            res.add(tmp);
            if(deque.isEmpty()) break; // 若为空则提前跳出
            // 打印偶数层
            tmp = new ArrayList<>();
            for(int i = deque.size(); i > 0; i--) {
                // 从右向左打印
                TreeNode node = deque.removeLast();
                tmp.add(node.val);
                // 先右后左加入下层节点
                if(node.right != null) deque.addFirst(node.right);
                if(node.left != null) deque.addFirst(node.left);
            }
            res.add(tmp);
        }
        return res;
    }
}

```

- 因为奇数层时从first拿，从last加；偶数层是从last拿，从first加；两者分别从两端拿。所以可以处理完奇数层后紧接着就处理偶数层。
- 本题，奇数层从双端队列的左边拿数，偶数层从双端队列的右边拿数。





### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

#### 首战寄

- 思路比较混乱，担心自己的算法会造成节点的反复判断，就放弃了

#### 官方-递归

解题思路：

![image-20220329184634598](lcof.assets/image-20220329184634598.png)

算法流程：

![image-20220329184701480](lcof.assets/image-20220329184701480.png)

复杂度分析：

![image-20220329185817677](lcof.assets/image-20220329185817677.png)

代码：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        //如果左右节点有空的，则说明传了空树进来，按照题目说明，空树不参与字数判断，直接返回false。
        if (A == null||B == null ) {
            return false;
        }

        //左右节点都不为空，如果当前节点相等，就继续判断当前两个节点的左右子树是否还符合子结构
        if (A.val == B.val && (helper(A.left, B.left) && helper(A.right, B.right))) {
            return true;
        }

        //如果以当前节点为根的树不符合，就判断A的左子树是否包含B，或A的右子树是否包含B
        return isSubStructure(A.left, B) || isSubStructure(A.right, B);
    }

    private boolean helper(TreeNode root1, TreeNode root2) {
        //必须要先判断root2，再判断root1；这样如题干中B树的4号节点的右子树为空，但是A树的4号节点的右子树不为空，先判断B树为空才能返回true。
        if (root2 == null) {
            return true;
        }
        if (root1 == null) {
            return false;
        }
        //如果树的路径上的节点相同，继续往下判断
        if (root1.val == root2.val) {
            return helper(root1.left, root2.left) && helper(root1.right, root2.right);
        } else {
            return false;
        }
    }
}
```

- helper专门用来判断左右子树。



### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        /*
        交换每个节点的左右子节点，即得到镜像
         */
         
         //如果原树为空，返回null
         if(root==null)return null;
        //如果原树不为空，创造新树的根节点，并递归构建新树的左右子节点。
         TreeNode result=new TreeNode(root.val);
         result.left=recur(root.right);
         result.right=recur(root.left);

        //返回新树
         return result;
    }
    public TreeNode recur(TreeNode root){
        if(root==null) return null;
        
        //设置好当前节点和左右子节点
        TreeNode curNode=new TreeNode(root.val);
        curNode.left=recur(root.right);
        curNode.right=recur(root.left);

        //把当前节点返回给上一层递归
        return curNode;
    }
}
```

#### 官方-递归

思路与算法:

- 这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子节点先开始翻转得到镜像。如果当前遍历到的节点root 的左右两棵子树都已经翻转得到镜像，那么我们只需要交换两棵子树的位置，即可得到以root 为根节点的整棵子树的镜像。

代码：

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode left = mirrorTree(root.left);
        TreeNode right = mirrorTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```

- 官方思路和我一样，不过我自建了一个recur函数，让recur专门负责子节点的处理，让逻辑更清晰。

复杂度分析:

- 时间复杂度：O(N)，其中 N  为二叉树节点的数目。我们会遍历二叉树中的每一个节点，对每个节点而言，我们在常数时间内交换其两棵子树。

- 空间复杂度：O(N) 。使用的空间由递归栈的深度决定，它等于当前节点在二叉树中的高度。在平均情况下，二叉树的高度与节点个数为对数关系，即 O(logN)。而在最坏情况下，树形成链状，空间复杂度为 O(N)。

### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

#### 首战寄

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        /*
        deque和queue用linkedlist实现是可以存储null的（官方不建议），这样可以用bfs构建队列，从两端拿节点，只要不同就返回false
         */
        if(root==null)return true;

        Deque<Integer> deque=new LinkedList<>();
        deque.addLast(root.val);
        while(!deque.isEmpty()){
            
            for
        }

    }
}
```

- 没有好方法，本来想着用deque从两端往中间拿数来判断对称，但是同时从两端同时拿数的话又无法正常形成下一层队列。

#### 官方-递归

解题思路：

![image-20220330162905068](lcof.assets/image-20220330162905068.png)

算法流程：

![image-20220330162925469](lcof.assets/image-20220330162925469.png)

复杂度分析：

![image-20220330162944480](lcof.assets/image-20220330162944480.png)

<img src="https://pic.leetcode-cn.com/88916808515487aac3ca24f9c55cbbdf6514f012eea04ec46cc2cc26acf9c4eb-Picture12.png" alt="Picture12.png" style="zoom:25%;" />

代码：

细节版：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return check(root.left, root.right); // 检查root的左右子树
    }

    private boolean check(TreeNode a, TreeNode b) { 
        if (a == null && b == null) return true; // 都为空, 对称
        if (a != null && b == null) return false;
        if (a == null && b != null) return false; // 只有一边为空, 不对称
        if (a.val != b.val) return false; // 都不为空, 但值不相等

        boolean outside = check(a.left, b.right); // 比较a, b两棵树的外侧
        boolean inside = check(a.right, b.left); // 比较a, b两颗树的内测
        return outside && inside; // a, b内外侧都相等才对称
    }
}

```

简写版：

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null ? true : recur(root.left, root.right);
    }
    boolean recur(TreeNode L, TreeNode R) {
        if(L == null && R == null) return true;
        if(L == null || R == null || L.val != R.val) return false;
        return recur(L.left, R.right) && recur(L.right, R.left);
    }
}

```

#### 即时再战成功

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        /*对称比较，抓住树的外侧和内侧 */
        if(root==null)return true;

        //原树根节点不为空的话，比较左右子树是否对称
        return recur(root.left,root.right);
    }
    public boolean recur(TreeNode leftTree,TreeNode rightTree){
        //如果传入的两个节点都为null，说明遍历到本路劲的末尾都没false，返回代表本路劲的true
        if(leftTree==null&&rightTree==null) return true;
        //如果传入的两个树根节点的值不相同，或左右根节点有一个为空另一个不是，则说明传入的两个树不对称；注意判断null在取val之前做，否则可能空指针异常,这就要利用||的“得到true就停止判断的特性”
        if((leftTree==null&&rightTree!=null)||(leftTree!=null&&rightTree==null)||(leftTree.val!=rightTree.val))return false;

        //如果上面情况都没发生，说明传入的两个树的根节点相同，递归判断这链各个树是否为对称的。
        return recur(leftTree.left,rightTree.right)&&recur(leftTree.right,rightTree.left);
    }

}
```

## 动态规划(简单)

### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

#### 首战寄

先尝试直接递归，n==43时会超时，因为执行每一个fib都会一直递归到0或1，但是比如`return (fib(n-1)+fib(n-2))%1000000007;`计算了左边的fib(49)后，计算右边的fib(48)时还递归到底，所以最好把计算过的结果保存！保存计算结果应该是动态规划区别于递归的一个特征。

```java
class Solution {
    public int fib(int n) {
        //设定返回条件
        if(n==0) {
            return 0;
        }else if(n==1) {
            return 1;
        }

        return (fib(n-1)+fib(n-2))%1000000007;
    }
}
```

- 但是想不起来怎么保存结果了

#### 官方-动态规划

解题思路：

![image-20220330174419199](lcof.assets/image-20220330174419199.png)

![image-20220330174432153](lcof.assets/image-20220330174432153.png)

动态规划解析：

![image-20220330174451619](lcof.assets/image-20220330174451619.png)

空间复杂度优化：

![image-20220330174517988](lcof.assets/image-20220330174517988.png)

循环求余法 处理大数：

![image-20220330174550027](lcof.assets/image-20220330174550027.png)

复杂度分析：

![image-20220330174615495](lcof.assets/image-20220330174615495.png)

代码：

```java
class Solution {
    public int fib(int n) {
        int a = 0, b = 1, sum;
        for(int i = 0; i < n; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
}
```

#### 即时再战半寄

```java
class Solution {
    public int fib(int n) {
        /*
        动态规划，当前值只与前两个值有关；让值从小往大加即可
         */

        //先返回特殊值；这样下面的for循环才能安全地只接收n>=2的情况
        if(n==0||n==1)return n;

        int pre2=0,pre1=1;

        //循环当前的i为谁，执行完当前循环，得到的sum即为fib(i)
        for(int i=2;i<=n;i++){

            int tempSum=(pre2%1000000007+pre1%1000000007)%1000000007;
            pre2=pre1;
            pre1=tempSum;
        }

        //最后一次循环得到的sum即为fib(n)，这个sum被复制给pre1了
        return pre1;

    }
}
```

- 第一次提交的时候，忽略了处理n==1和0的情况；for循环无法处理n<2的情况，所以要做面对n==1和0的情况时的特殊处理。如上处理后就提交成功了。



### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

#### 首战告捷

```java
class Solution {
    public int numWays(int n) {
        /*
        跳上台阶的跳法，至只与前两步有关系:跳到当前台阶的方法数==跳到prev1（前一个台阶）的方法数+跳到prev2的方法数。
        因为跳到当前台阶只有两种可能：从prev1跳1个台阶上来，或从prev2一跃跳两个台阶上来
         */

         if(n==0||n==1)return 1;
         if(n==2)return 2;

        //从第三级台阶开始动态规划
         int prev1=2,prev2=1;
         //当前循环为i，则执行完当前循环时的sum为跳到第i级台阶的跳法数
         for(int i=3;i<=n;i++){
             int tempSum=(prev1%1000000007+prev2%1000000007)%1000000007;
             prev2=prev1;
             prev1=tempSum;
         }

         return prev1;

    }
}
```

#### 官方-动态规划

解题思路：

![image-20220330180140749](lcof.assets/image-20220330180140749.png)

- 本题和斐波那契数列问题基本一模一样。
- 注意大佬评语：此类求 ***多少种可能性*** 的题目一般都有 **递推性质** ，即 f(n)*f*(*n*) 和 f(n-1)*f*(*n*−1)…f(1)*f*(1) 之间是有联系的。

复杂度分析：

![image-20220330180256791](lcof.assets/image-20220330180256791.png)

代码：

```java
class Solution {
    public int numWays(int n) {
        int a = 1, b = 1, sum;
        for(int i = 0; i < n; i++){
            sum = (a + b) % 1000000007;
            a = b;
            b = sum;
        }
        return a;
    }
}
```



### [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

#### 首战告捷

```java
class Solution {
    public int maxProfit(int[] prices) {
        /*记录历史最大利润，和最低价格的股票 */
        
        //处理特殊情况
        if(prices.length==0)return 0;
        
        int lowP=prices[0];
        int benefit=0;
        for(int i=0;i<prices.length;i++){
            //时刻更新最大收益
            if((prices[i]-lowP)>benefit){
                benefit=prices[i]-lowP;
            }
            //只要碰见更小的price就保存，因为未来碰见相同的高值时，记录下更小的lowP可以带来更大的收入。
            if(prices[i]<lowP){
                lowP=prices[i];
            }
        }
        
        return benefit;

    }
}
```

#### 官方-动态规划

解题思路：

![image-20220330192323464](lcof.assets/image-20220330192323464.png)

动态规划解析：

![image-20220330192357464](lcof.assets/image-20220330192357464.png)

效率优化：

![image-20220330192427858](lcof.assets/image-20220330192427858.png)

复杂度分析：

![image-20220330192446488](lcof.assets/image-20220330192446488.png)

代码：

```java
class Solution {
    public int maxProfit(int[] prices) {
        int cost = Integer.MAX_VALUE, profit = 0;
        for(int price : prices) {
            cost = Math.min(cost, price);
            profit =Math.min(profit, price - cost);
        }
        return profit;
    }
}
```

- 注意这里java工具类方法`Math.min`和`Math.min`的用法。



## 动态规划(中等)

### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

#### 首战寄

#### 官方-动态规划

解题思路：

![image-20220331195848158](lcof.assets/image-20220331195848158.png)

动态规划解析：

![image-20220331195938823](lcof.assets/image-20220331195938823.png)

空间复杂度降低：

![image-20220331200048428](lcof.assets/image-20220331200048428.png)

复杂度分析：

![image-20220331200117017](lcof.assets/image-20220331200117017.png)

代码：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = nums[0];
        for(int i = 1; i < nums.length; i++) {
            nums[i] += Math.max(nums[i - 1], 0);
            res = Math.max(res, nums[i]);
        }
        return res;
    }
}
```

网友补充：感谢分享，思路很清晰，只是有一点建议。 因为有的时候，题目要求可能不能修改原有数组，考虑到在dp列表中，dp[i]只和dp[i-1]有关,所以用两个参数存储循环过程中的dp[i]和dp[i-1]的值即可，空间复杂度也为O(1)。 代码如下：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        int former = 0;//用于记录dp[i-1]的值，对于dp[0]而言，其前面的dp[-1]=0
        int cur = nums[0];//用于记录dp[i]的值
        for(int num:nums){
            cur = num;
            if(former>0) cur +=former;
            if(cur>max) max = cur;
            former=cur;
        }
        return max;
    }
}
```



#### 即时再战成功

```java
class Solution {
    public int maxSubArray(int[] nums) {
        /* 
        dpi表示已i号元素结尾的最大子数组，那么dpi=Max(dp[i-1]+numsi,numsi),所以dp[i-1]<0的话只能拖后腿就截掉
        因为最长子数组是连续的，所以要以i为结尾的数组做dp数组
        */
        //处理特殊情况
        if(nums.length==0)return 0;
        if(nums.length==1)return nums[0];

        //从数组第二个元素开始就dpi
        int curMaxSum=nums[0],historyMaxSum=curMaxSum;
        for(int i=1;i<nums.length;i++){
            if(curMaxSum<0){
                curMaxSum=nums[i];
            }else{
                curMaxSum+=nums[i];
            }

            //判断当前的最大值是否比之前所有记载过的最大值都大，都大的话更新历史最大值
            if(curMaxSum>historyMaxSum){
                historyMaxSum=curMaxSum;
            }
        }

        return historyMaxSum;

    }
}
```



### [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

#### 首战告捷

```java
class Solution {
    public int maxValue(int[][] grid) {
        /*
        覆写二维数组，在相应位置写上“以左上角出发到当前位置能拿到的最大礼物”，这个礼物值只与上一节点和左一节点有关。

        为了空间复杂度，直接在二维数组上记录
         */
         //处理特殊情况;注意要先判断行不为0，否则先判断列不为0的话可能出现空指针异常
         if(grid.length==0||grid[0].length==0)return 0;

         //先处理二维数组的行
         for(int i=0;i<grid.length;i++){
             //再在二维数组的某个行中依次处理各个列元素
             for(int j=0;j<grid[0].length;j++){
                 //获得数组当前元素，从左or从上来，哪个拿更多礼物
                 int tempLeft=0,tempUp=0;
                if(i>0){//如果i大于0，说明不在第一行，那么可以获得grid[i-1][j]位置存放的最大礼物
                    tempUp=grid[i-1][j];
                }
                if(j>0){//如果j大于0，说明不在第一列，那么可以获得grid[i][j-1]位置存放的最大礼物
                    tempLeft=grid[i][j-1];
                }

                //把grid[i][j]覆盖写为走当当前位置能拿到得最多礼物。
                grid[i][j]=Math.max(tempLeft,tempUp)+grid[i][j];
             }

         }

         return grid[grid.length-1][grid[0].length-1];

    }
}
```

- 看了官方解析，我得解法就是官方的待优化版，可以先初始化“第一行 第一列”，这样就避免每次判断当前元素是不是在二维数组的边缘。

#### 官方-动态规划

解题思路：

![image-20220401152228155](lcof.assets/image-20220401152228155.png)

动态规划解析：

![image-20220401152255940](lcof.assets/image-20220401152255940.png)

空间复杂度优化：

![image-20220401152320811](lcof.assets/image-20220401152320811.png)

复杂度分析：

![image-20220401152346735](lcof.assets/image-20220401152346735.png)

代码：

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(i == 0 && j == 0) continue;
                if(i == 0) grid[i][j] += grid[i][j - 1] ;
                else if(j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j]);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```

以上代码逻辑清晰，和转移方程直接对应，但仍可提升效率：当 gridgrid 矩阵很大时， i = 0i=0 或 j = 0j=0 的情况仅占极少数，相当循环每轮都冗余了一次判断。因此，可先初始化矩阵第一行和第一列，再开始遍历递推:

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int j = 1; j < n; j++) // 初始化第一行
            grid[0][j] += grid[0][j - 1];
        for(int i = 1; i < m; i++) // 初始化第一列
            grid[i][0] += grid[i - 1][0];
        for(int i = 1; i < m; i++)
            for(int j = 1; j < n; j++) 
                grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j]);
        return grid[m - 1][n - 1];
    }
}

```



### [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

#### 首战寄

#### 大佬-适合新手理解

思路：

此题总结规律和青蛙跳台阶有点相似（没刷到的可以先刷青蛙跳台阶），青蛙跳台阶是只能跳一步或者两步，所以最后只有两种情况：


    1. 跳到n-1阶，然后再跳1
    2. 跳到n-2阶,然后再跳2
    所以f(n) = f(n-1)+f(n-2)
此题关注的是最大能翻译的数字为25，说明情况为要不一次性翻译一个数字，要不一次性翻译两个数字（不可能三个数字，三个数字肯定大于25了），和青蛙跳只能跳一步或两步类似


    假设dp(i)为长度为i的数字的翻译总数
        a.如果最后两位数字大于10且小于25，他才能翻译两个数字，他就和青蛙跳类似有两种情况：
             1.dp(i-1),然后最后一个数字单独翻译
             2.dp(i-2),然后最后两个数字一起翻译
             所以dp(i) = dp(i-1) + dp(i-2);
        b.如果最后两位数字比10小，比25大，那么他最后一个数字只能单独翻译，所以只有一种情况：
             1.dp(i-1),然后最后一个数字单独翻译,所以dp(i) = dp(i-1);
代码(最好理解的写法)

```java
class Solution {
    public int translateNum(int num) {
        //dp[i-2]
        int a = 1;
        //dp[i-1]
        int b = 1;
        //dp[i]
        int c = 1;
        String s = String.valueOf(num);
        for (int i = 1; i < s.length(); i++) {
            //最后两个数字大于10小于25
            if ((s.charAt(i-1) == '1') || ( s.charAt(i-1) == '2' && s.charAt(i) <= '5')) {
                c = b + a;
                //更新新的dp[i-2]
                a = b;
                //更新新的dp[i-1]
                b = c;
            }else {
                c = b;
                //由于新的dp[i-1]值没变，所以只更新新的dp[i-2]
                a = b;
            }
        }
        return c;

    }
}
```

- 注意：字符‘1’ ‘2’ ‘3’ ‘4’ ‘5’，，也是ascii递增的，他们可以用符号比大小。

#### 官方1-动态规划+字符串遍历

解题思路：

根据题意，可按照下图的思路，总结出 “递推公式” （即转移方程）。
因此，此题可用动态规划解决，以下按照流程解题。

![Picture1.png](lcof.assets/e231fde16304948251633cfc65d04396f117239ea2d13896b1d2678de9067b42-Picture1.png)

动态规划解析：

![image-20220401172718039](lcof.assets/image-20220401172718039.png)

字符串遍历做法：

![image-20220401172829205](lcof.assets/image-20220401172829205.png)

复杂度分析：

![image-20220401172857171](lcof.assets/image-20220401172857171.png)

代码：

```java
class Solution {
    public int translateNum(int num) {
        String s = String.valueOf(num);
        int a = 1, b = 1;
        for(int i = 2; i <= s.length(); i++) {
            String tmp = s.substring(i - 2, i);
            int c = tmp.compareTo("10") >= 0 && tmp.compareTo("25") <= 0 ? a + b : a;
            b = a;
            a = c;
        }
        return a;
    }
}
```

高赞网友补充：动态规划思想，按照大佬的思路，使用的数组（感觉使用a和b不太直观）

```java
class Solution {
    public int translateNum(int num) {
        String s = String.valueOf(num);
        int[] dp = new int[s.length()+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= s.length(); i ++){
            String temp = s.substring(i-2, i);
            if(temp.compareTo("10") >= 0 && temp.compareTo("25") <= 0)
                dp[i] = dp[i-1] + dp[i-2];
            else
                dp[i] = dp[i-1];
        }
        return dp[s.length()];
    }
}
```

![image-20220401173035518](lcof.assets/image-20220401173035518.png)

```java
class Solution {
    public int translateNum(int num) {
        String s = String.valueOf(num);
        int a = 1, b = 1;
        for(int i = s.length() - 2; i > -1; i--) {
            String tmp = s.substring(i, i + 2);
            int c = tmp.compareTo("10") >= 0 && tmp.compareTo("25") <= 0 ? a + b : a;
            b = a;
            a = c;
        }
        return a;
    }
}
```

#### 官方2-动态规划+数字求余

上述方法1虽然已经节省了 dp列表的空间占用，但字符串 s 仍使用了 O(N)大小的额外空间。

空间复杂度优化：

![image-20220401173303141](lcof.assets/image-20220401173303141.png)

复杂度分析：

![image-20220401173327733](lcof.assets/image-20220401173327733.png)

代码：

```java
class Solution {
    public int translateNum(int num) {
        int a = 1, b = 1, x, y = num % 10;
        while(num != 0) {
            num /= 10;
            x = num % 10;
            int tmp = 10 * x + y;
            int c = (tmp >= 10 && tmp <= 25) ? a + b : a;
            b = a;
            a = c;
            y = x;
        }
        return a;
    }
}
```

高赞网友补充：大佬，直接对100取余可以简化代码:

```java
class Solution {
    public int translateNum(int num) {
        int m = 1, n = 1;
        // 至少是一个两位数才需要考虑能不能组合
        while (num > 9) { 
            int t = (num % 100 < 26 && num % 100 > 9) ? m + n : n;
            m = n;
            n = t;
            num /= 10;
        }
        return n;
    }
}
```

#### 及时再战成功

```java
class Solution {
    public int translateNum(int num) {

        /**
        数字翻译成字符串是对称的，所以从右往左看；以nums[i-1]为首位的数字，可能收到nums[i]和nums[i+1]的影响
         */
        //初始化，把num看做nums数组的话，表示以nums[len-1]和nums[len]为终点的的翻译有几种情况，虽然初始情况下dpLater2对应的nums[len]不存在，但是可以通过dp[len-2]和dp[len-1]推出dp[len].
        int dpLater1=1,dpLater2=1;

        //使用对100求余数，得到最接近左遍历终点的两个数字构成的数
        while(num>9){
            int tempFocus=num%100;
            int dpCur;
            //数字在[10,25]有两种翻译方法
            if(tempFocus>=10&&tempFocus<=25){
                dpCur=dpLater1+dpLater2;
            }else{
                dpCur=dpLater1;
            }

            //往左继续处理
            dpLater2=dpLater1;
            dpLater1=dpCur;
            num/=10;
        }

        //最后的dpCur给了dpLater1
        return dpLater1;

    }
}
```

自画思路图：

![image-20220401181306013](lcof.assets/image-20220401181306013.png)

- 即最后的12，如果整体翻译，就是以4为左结尾的翻译可能数（12    4，共一种情况）；如果拆开翻译，即是以2为结尾的返回数（1   2   4，1    24，  共两种情况）；又因为既能翻译为1又能翻译为12，所以是两种翻译情况的种数的和。
  - 比如324，最后分析32，不能作为翻译单元，所以只能拆开3和2；有3   24，3    2    4两种情况，即相当于以2结尾的翻译可能数。

- 其实核心思想就是，从右往左处理时，最后的翻译单元是1 还是12；最后的翻译单元是1则对应一个1xxxx翻译，最后的翻译单元是12则对应一个12xxxx翻译，xxxx区域相当于就是动态规划的子问题，共两种情况。如果数字更长的话，也是同理。

### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

#### 首战寄

#### 官方-解题思路

解题思路：

![image-20220402102952931](lcof.assets/image-20220402102952931.png)

动态规划解析：

![image-20220402103030643](lcof.assets/image-20220402103030643.png)

空间复杂度优化：

![image-20220402103106620](lcof.assets/image-20220402103106620.png)

#### 官方方法1-动态规划 + 哈希表

![image-20220402103323735](lcof.assets/image-20220402103323735.png)

复杂度分析：

![image-20220402103346795](lcof.assets/image-20220402103346795.png)

代码：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> dic = new HashMap<>();
        int res = 0, tmp = 0;
        for(int j = 0; j < s.length(); j++) {
            int i = dic.getOrDefault(s.charAt(j), -1); // 获取索引 i
            dic.put(s.charAt(j), j); // 更新哈希表
            tmp = tmp < j - i ? tmp + 1 : j - i; // dp[j - 1] -> dp[j]
            res = Math.max(res, tmp); // max(dp[j - 1], dp[j])
        }
        return res;
    }
}
```

- Java 的  getOrDefault(key, default)， 代表当哈希表包含键 key时返回对应 value ，不包含时返回默认值 default 。

- 我：dpj的值由dpj-1和i共同决定；如果i在dpj-1所代表的字符串中，则dpj的长度为j-i；如果i不在dpj-1所代表的字符串中，则dpj的长度为(dpj-1)+1。

#### 官方方法2-动态规划 + 线性遍历

思路：

![image-20220402104704664](lcof.assets/image-20220402104704664.png)

代码：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> dic = new HashMap<>();
        int res = 0, tmp = 0;
        for(int j = 0; j < s.length(); j++) {
            int i = j - 1;
            while(i >= 0 && s.charAt(i) != s.charAt(j)) i--; // 线性查找 i
            tmp = tmp < j - i ? tmp + 1 : j - i; // dp[j - 1] -> dp[j]
            res = Math.max(res, tmp); // max(dp[j - 1], dp[j])
        }
        return res;
    }
}

```

- 这种往左遍历增大了时间复杂度为on2，不可取，不建议

#### 官方方法3-双指针 + 哈希表

思路：

![image-20220402104914233](lcof.assets/image-20220402104914233.png)

代码：

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> dic = new HashMap<>();
        int i = -1, res = 0;
        for(int j = 0; j < s.length(); j++) {
            if(dic.containsKey(s.charAt(j)))
                i = Math.max(i, dic.get(s.charAt(j))); // 更新左指针 i
            dic.put(s.charAt(j), j); // 哈希表记录
            res = Math.max(res, j - i); // 更新结果；j - i即为以i为右指针的最长字符串的长度，其要与历史最大值比较来更新历史最大值。
        }
        return res;
    }
}

```

- 网友问：left = Math.max(left, hashMap.get(ch) + 1);为什么取最大值

  - 网友答：

  - 因为需要保证的是`[left,right]`没有重复值。而你的left如果不取最大值的话，是有可能往前退，就无法保证这个条件了。

    比如`abba` , 到第二个`a`的时候，这时候上一轮的第二个b完成，此时`left=2` ，而`a`上一次出现的地方是0。

    如果你直接`left = hashMap.get(ch) + 1` ,此时 left = 1 ,范围为`[left,right] = [1,3]` 而此时`[1,2] = bb` ，b已经重复了。本质就是你这个字母上次出现的地方到当前再次出现的位置，这两者间可能存在重复的值

#### 即时再战半寄

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        /**
        从左往右遍历，ij分别为左右重复元素，dpj由dpj-1和i共同决定
         */

        //存储字符元素-位置
        Map<Character,Integer> map=new HashMap<>();
        //j位置为当前元素，i为当前元素之前的重复元素的位置
        int i=-1,dpPre=0,result=0;
        for(int j=0;j<s.length();j++){
            //先获取当前元素的上一个重复元素的index，如果没出现过则返回-1；
            i=map.getOrDefault(s.charAt(j),-1);

            //当前元素为右尾的子串的长度，由dpPre和ij关系共同决定。注意dpPre==j-i时j的上一个重复元素在dpPre对应的子串的首位，所以在dpPre对应的子串内，所以不能用dpPre+1作为新值。
            int dpCur=(dpPre>=j-i?j-i:dpPre+1);
            //比较以当前元素为结尾的子串的最大长度和历史上记录的最大长度，更新历史最大长度
            result=Math.max(result,dpCur);

            //更新s.charAt(j)字符的位置为当前位置
            map.put(s.charAt(j),j);
            //不要忘记更新dpPre，否则dpPre永远是0
            dpPre=dpCur;
        }

        return result;

    }
}
```

- 能理顺动态规划的思路，但是写代码的时候忘记更新dpPre，且一直没排查出来；最后用idea debug才发现。**涉及到pre的一定要注意pre是否需要被更新**！

## 双指针(简单)

### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        //处理特殊情况。习惯性的做一下null处理，避免恶心人的案例。
        if(head==null)return null;
        
        //创建引用当前节点的引用
        ListNode cur=head;
        
        //由于总是删除下一节点，所以先处理头节点;因为头节点不能被通过前节点删除。
        if(head.val==val){
            head=head.next;
            return head;
        }

        //由于总是判断删除下一个节点，所以判断的是next不为空；本while循环不会循环完退出，只会在循环中找到了要删除的节点并退出
        while(cur.next!=null){
            //判断当前节点的下一个节点要不要被删掉；由于程序最迟在cur为倒数第二个节点时能判断到倒数第一个节点要被删掉，并且while的条件保证了下一节点不为空，所以其实不用担心cur.next==null即cur为结尾指针造成的空指针异常。
            if(cur.next.val==val){
                //即使是删掉最后一个节点，cur为倒数第二个节点，cur.next.next也为null不会空指针异常
                cur.next=cur.next.next;
                return head;
            }
            //while不要忘记移动cur
            cur=cur.next;
        }
        //正常来说，这个return是永远不会使用的。
        return null;
    }
}
```

- 没用到双指针，就时间复杂度on解决了

#### 官方-双指针

其实官方的双指针就是我的做法吗，只不过多此一举用了两个指针。

解题思路：

![image-20220403171506936](lcof.assets/image-20220403171506936.png)

算法流程：

![image-20220403171539038](lcof.assets/image-20220403171539038.png)

复杂度分析：

![image-20220403171557465](lcof.assets/image-20220403171557465.png)

代码：

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head.val == val) return head.next;
        ListNode pre = head, cur = head.next;
        while(cur != null && cur.val != val) {
            pre = cur;
            cur = cur.next;
        }
        if(cur != null) pre.next = cur.next;
        return head;
    }
}
```

- 我：有个网友评价“单指针”就够了，做法就是我的做法。



### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

#### 首战告捷--递归法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    //定义全局变量
    ListNode result;
    public ListNode getKthFromEnd(ListNode head, int k) {
        /*
        倒数，想到栈：先把节点全部入栈，出栈的时候计数，到指定节点都出栈的时候，返回那个节点。
        栈可以通过递归实现，计数可以通过全局变量
         */

        //执行递归，以保存目标节点
        recur(head,k);

        return result;

    }
    public int recur(ListNode head,int k){
        //设定递归触底条件，哪个递归层拿到的return与k相同，则说明应返回当前节点
        if(head==null) return 1;

        //如果下一层递归返回的当前层的倒数层数为k，说明当前节点就是目标节点；
        int kthFromEnd=recur(head.next,k);
        if(kthFromEnd==k){
            result=head;
        }

        //给退栈时的各层返回他们的倒数的层数
        return kthFromEnd+1;
    }
}
```

- 问问问：我n层递归，空间复杂度是不是就是on了，那性能就不如官方的了。

#### 官方--双指针

解题思路：

![image-20220403175326006](lcof.assets/image-20220403175326006.png)

算法流程：

![image-20220403175352955](lcof.assets/image-20220403175352955.png)

复杂度分析：

![image-20220403175419032](lcof.assets/image-20220403175419032.png)

代码：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode former = head, latter = head;
        for(int i = 0; i < k; i++)
            former = former.next;
        while(former != null) {
            former = former.next;
            latter = latter.next;
        }
        return latter;
    }
}
```

- 本题没有 k*k* 大于链表长度的 case ，因此不用考虑越界问题。

- 我：看来双指针可以这样控制距离，牛逼！



### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //处理特殊情况
        if(l1==null)return l2;
        if(l2==null)return l1;
        //能走到这一步说明两个链表的头结点都不为空，不会空指针；保存初始头结点为两个链表头结点里小的那个；必须保存了头结点，后面while中才能有head.next的方式赋值，才能连接起链表。
        ListNode head;
        if(l1.val<=l2.val){
            head=l1;
            //l1的头结点已经被加入结果链表了，所以往下移动一位看下一个节点
            l1=l1.next;
        }else{
            head=l2;
            l2=l2.next;
        }
        ListNode headCur=head;

        //建立双指针分别指向两个链表，合并
        ListNode pl1=l1;
        ListNode pl2=l2;
        //把两个节点中较小的节点加入结果列表head
        while(pl1!=null&&pl2!=null){   
            if(pl1.val<=pl2.val){
                headCur.next=pl1;
                headCur=headCur.next;
                pl1=pl1.next;
            }else{
                headCur.next=pl2;
                headCur=headCur.next;
                pl2=pl2.next;
            }
        }

        //一个队列遍历完毕后，把另一个队列的剩余部分直接加入结果链表
        if(pl1!=null){
            headCur.next=pl1;
        }else{
            headCur.next=pl2;
        }


        return head;
    }
}
```

- 为了结果链表的第一个节点，我的操作比较复杂，实际上可以像官方答案一样，使用一个伪头节点即可。

#### 官方-双指针+伪头结点

解题思路：

![image-20220403192034915](lcof.assets/image-20220403192034915.png)

算法流程：

![image-20220403192122384](lcof.assets/image-20220403192122384.png)

复杂度分析：

![image-20220403192147988](lcof.assets/image-20220403192147988.png)

代码：

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dum = new ListNode(0), cur = dum;
        while(l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            }
            else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 != null ? l1 : l2;
        return dum.next;
    }
}

```

- 这个伪头结点的思路确实不错！



### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

#### 首战寄

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        /**
         链表判断环，判断相交，就考虑双指针
         */
        ListNode la=headA;
        ListNode lb=headB;

        //使用两个指针分别遍历两个链表，当发现指向同一节点时返回该节点；以节点的地址为判断节点相等的标准，因为节点的值可重复见（示例）
        while((la!=null)&&(lb!=null)){
            System.out.println("la:"+la);
            System.out.println("lb:"+lb);
            if(la==lb){
                return la;
            }
            //while始终记住要移动指针
            la=la.next;
            lb=lb.next;
        }

        //经过遍历都没找到公共节点，说明无公共节点
        return null;
    }
}
```

- 忽略了公共节点前，两个链表的前部长度不一致的问题；比如公共部分有c1 c2的话，头部长的链表的指针走到c1，头部短的链表的指针走到c2，两个指针总是有前后差距错开了。



#### 官方-双指针+浪漫相遇

解题思路：

我们使用两个指针 node1，node2 分别指向两个链表 headA，headB 的头结点，然后同时分别逐结点遍历，当 node1 到达链表 headA 的末尾时，重新定位到链表 headB 的头结点；当 node2 到达链表 headB 的末尾时，重新定位到链表 headA 的头结点。

这样，当它们相遇时，所指向的结点就是第一个公共结点。

复杂度分析：

![image-20220404150150252](lcof.assets/image-20220404150150252.png)

代码：

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode PA = headA;
        ListNode PB = headB;
        while (PA != PB) {
            PA = PA == null ? headB : PA.next;
            PB = PB == null ? headA : PB.next;
        }
        return PA;
    }
}
```

#### 即时再战成功

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {

        /**
        小心两个链表的头部的长度是不一致的，需要让两个指针都走一遍两个列表；这样要不走完都没相遇，要不在走对方列表的时候相遇
         */

        ListNode ha=headA;
        ListNode hb=headB;

        /**
        遍历情况分类讨论：
        1，如果两条链表一样长：如果有公共节点说明头部也一样长，那么指针在遍历各自的第一条链表时就能找到交点；如果没有公共节点，则两个指针会同时走到链表末尾都为null，触发while的退出，即可返回null即无交点。
        2，如果两条链表不一样长：那么要处理节点前后距离差的问题，如果有交点，那么指针会把两个链表都走一遍，走到第二个链表的时候会在交点出相交；如果两个链表没有交点的话，指针也会把两个链表都走一遍，两个指针也会同时从两个链表的尾部退出

        注意由于列表长度可能不等，所以要两个指针都为null即都到了链表结尾（要不数组长度相等一遍过无交点，要不数组长度不等两遍过无交点），才允许退出；只一个指针为null的话，可能另一个指针还在遍历第一个链表，不能允许退出。
         */
        while(ha!=hb){
            //如果当前节点遍历到第一条链表的末尾，另一个节点还在遍历链表时，让他遍历第二条链表；没有交点的话第二条链表的结尾两个指针都会指向null，退出while
            ha=(ha==null?headB:ha.next);
            hb=(hb==null?headA:hb.next);
        }

        //ha==hb且ha!=null退出的话，ha==hb==交点；ha==hb==null退出的话，也返回null
        return ha;
        
    }
}
```



### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

#### 首战告捷

```java
class Solution {
    public int[] exchange(int[] nums) {
        /**
        双指针，l指向奇数段的右部右一位，r指向偶数段左部左一位
        只移动一个指针，本例移动奇指针，发现偶数就给偶指针交换，偶指针再左移动一位，奇指针重新判断；发现奇数则寄指针右移一位；两指针重合时退出，重合位置的奇偶性无所谓，毕竟位于奇数段偶数段的交点，是谁都可以。
         */

        //处理特殊情况，测试案例[]纯纯乌鱼子
        if(nums.length==0)
        return nums;

         int l=0,r=nums.length-1;
         while(l!=r){
             //左指针发现偶数
             if(nums[l]%2==0){
                 //交换左右指针指向的数组元素
                 int temp=nums[r];
                 nums[r]=nums[l];
                 nums[l]=temp;
                //移动右指针
                r--;
             }else{//左指针发现奇数，左指针右移一位
                l++;
             }
             //如果左指针发现偶数，左右指针指向的数组元素交换后，左指针会再次检查被换来的元素是否为奇数
         }
        return nums;
    }
}
```

- 有完整思路和代码框架，但是漏了“左指针移动”，和”空数组处理“；不过自行修改完毕了。

#### 官方-双指针+同时移动

解题思路：

考虑定义双指针 ii , jj 分列数组左右两端，循环执行：

1. 指针 ii 从左向右寻找偶数；
2. 指针 jj 从右向左寻找奇数；
3. 将 偶数 nums[i]nums[i] 和 奇数 nums[j]nums[j] 交换。

可始终保证： 指针 ii 左边都是奇数，指针 jj 右边都是偶数 。

![image-20220404161217051](lcof.assets/image-20220404161217051.png)

算法流程：

![image-20220404161240739](lcof.assets/image-20220404161240739.png)

复杂度分析：

![image-20220404161301033](lcof.assets/image-20220404161301033.png)

代码：

![image-20220404161323780](lcof.assets/image-20220404161323780.png)

```java
class Solution {
    public int[] exchange(int[] nums) {
        int i = 0, j = nums.length - 1, tmp;
        while(i < j) {
            while(i < j && (nums[i] & 1) == 1) i++;
            while(i < j && (nums[j] & 1) == 0) j--;
            tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```

- 我：本代码左右同时动，我是一个去找另一个，时间复杂度一样；我的代码没有双循环，甚至更优雅些。

- 网友：本题蕴含快速排序的思想！

### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

#### 首战告捷

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        /**
        尽量用上“递增”这个条件，求和的两个数肯定一大一小；左右两端两个指针，求和，值大了左移右指针，值小了右移左指针
         */

        //处理空数组
        if(nums.length==0)return new int[0];

        int left=0,right=nums.length-1;
        while(left<right){
            if(nums[left]+nums[right]<target){
                left++;
            }else if(nums[left]+nums[right]>target){
                right--;
            }else{
                //注意是输出元素值，而不是输出index
                return new int[]{nums[left],nums[right]};
            }
        }

        //如果没找到，返回空
        return new int[0];
    }
}
```

#### 官方-对撞双指针

解题思路：

![image-20220404164513158](lcof.assets/image-20220404164513158.png)

算法流程：

![image-20220404164531875](lcof.assets/image-20220404164531875.png)

正确性证明：

![image-20220404164558086](lcof.assets/image-20220404164558086.png)

- 我：意会一下，被排除掉的元素都是不可能构成结果的。
  - 以初始状态为例，假设nums[left]+nums[right]<target，那么right左移，最右边的元素被排除在可能的结果集之外，
  - 采用反证法，如果右边的元素不该被排除在结果集之外，那么需要一个比left更小的元素，可是left已经是数组中的最小值了，不会有更小的值，所以反例不存在；
  - 所以被排除的最右边的元素不会是被误排除的，反证法证毕。

复杂度分析：

![image-20220404164622003](lcof.assets/image-20220404164622003.png)

代码：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i = 0, j = nums.length - 1;
        while(i < j) {
            int s = nums[i] + nums[j];
            if(s < target) i++;
            else if(s > target) j--;
            else return new int[] { nums[i], nums[j] };
        }
        return new int[0];
    }
}
```

- 网友：提醒一下，判断条件最好不要用相加后的结果，应该用target - nums[i] 跟 nums[j]比较，这样保证不会溢出。虽然这题中不会出错。同样的例子还有二分查找，(left + right) / 2 可以用left + ((rigth - left) >> 1))代替。



### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

#### 首战寄

- 写的时候就感觉要是用一些工具类，看题解果然至少要用StringBuilder。

#### 官方1-双指针

算法解析：

![image-20220404172111604](lcof.assets/image-20220404172111604.png)

复杂度分析：

![image-20220404172131469](lcof.assets/image-20220404172131469.png)

代码：

```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim(); // 删除首尾空格
        int j = s.length() - 1, i = j;
        StringBuilder res = new StringBuilder();
        while(i >= 0) {
            while(i >= 0 && s.charAt(i) != ' ') i--; // 搜索首个空格
            res.append(s.substring(i + 1, j + 1) + " "); // 添加单词
            while(i >= 0 && s.charAt(i) == ' ') i--; // 跳过单词间空格
            j = i; // j 指向下个单词的尾字符
        }
        return res.toString().trim(); // 转化为字符串并返回，因为单词尾部会被人为加上空格
    }
}
```

- 注意`str.substing(左闭,右开)`和`StringBuilder`的用法。

#### 官方2-分割 + 倒序（不建议）

利用 “字符串分割”、“列表倒序” 的内置函数 *（**面试时不建议使用**）* ，可简便地实现本题的字符串翻转要求。

算法解析：

![image-20220404172258931](lcof.assets/image-20220404172258931.png)

![Picture0.png](lcof.assets/9ef4a9ea565bf1c2d9209ca94881a77288f90f222476cfd44c418fa3f2d2d7c1-Picture0.png)

复杂度分析：

![image-20220404172508237](lcof.assets/image-20220404172508237.png)

代码：

```java
class Solution {
    public String reverseWords(String s) {
        String[] strs = s.trim().split(" "); // 删除首尾空格，分割字符串
        StringBuilder res = new StringBuilder();
        for(int i = strs.length - 1; i >= 0; i--) { // 倒序遍历单词列表
            if(strs[i].equals("")) continue; // 遇到空单词则跳过
            res.append(strs[i] + " "); // 将单词拼接至 StringBuilder
        }
        return res.toString().trim(); // 转化为字符串，删除尾部空格，并返回
    }
}
```

#### 即时再战成功

```java
class Solution {
    public String reverseWords(String s) {
        /**
        用双指针的间距定位单词，类似双指针取倒数第n个数一样
        掌握substring(s小写)， trim， StringBuilder， toString（s大写）的使用
         */

        //先剔除首位空格，保证指针初始指向的不是空格
        s.trim();

        StringBuilder sb= new StringBuilder();
        //ij想做取单词
        int j=s.length()-1,i=j;
        while(i>=0){
            //取单词；同时注意防止一路i--导致超过字符串边界
            while(i>=0&&s.charAt(i)!=' ')i--;
            //单词以空格分开
            sb.append(s.substring(i+1,j+1)+" ");
            //剔除单词间的空格；这里也需要担心超过字符串边界，因为一路清除空格到i=-1时，还会继续执行本内存while，导致超过字符串边界；
            while(i>=0&&s.charAt(i)==' ')i--;
            //空格剔除完毕后，i停在下一个单词的尾部，把j移动过来
            j=i;
        }

        //返回sb，记得去除最后单词后面的空格
        return sb.toString().trim();
    }
}
```

- 难点坑点：
  - 内部循环防超过字符串边界
  - 掌握substring(s小写)， trim， StringBuilder， toString（s大写）的使用



## 搜索与回溯算法(中等)

### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

#### 首战寄

完全无合适思路

#### 官方-DFS+剪枝

解题思路：

![image-20220405153358478](lcof.assets/image-20220405153358478.png)

DFS 解析：

![image-20220405153443132](lcof.assets/image-20220405153443132.png)

- 注意这里java中空字符的表达方式

复杂度分析：

![image-20220405153605776](lcof.assets/image-20220405153605776.png)

代码：

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j++) {
                if(dfs(board, words, i, j, 0)) return true;
            }
        }
        return false;
    }
    boolean dfs(char[][] board, char[] word, int i, int j, int k) {
        if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] != word[k]) return false;
        if(k == word.length - 1) return true;
        board[i][j] = '\0';
        boolean res = dfs(board, word, i + 1, j, k + 1) || dfs(board, word, i - 1, j, k + 1) || 
                      dfs(board, word, i, j + 1, k + 1) || dfs(board, word, i , j - 1, k + 1);
        board[i][j] = word[k];
        return res;
    }
}
```

- 注意点：
  - 注意java中空字符的表示；题目说数组只有英文大小写字母，空字符就能和原本的字符区分开。[参考](http://t.csdn.cn/E8dER)。
  - String 转为CharArray的方法：str.toCharArray()
  - 如何解决“本轮递归把元素置位空字符后，不影响下一轮递归”，即在本轮递归后用`board[i][j] = word[k];`把修改还原。

- 我：感觉就是暴力算法啊，，对每个节点深度递归；不过这题要求得到目标字符串顺序的路径，应该也只能暴力递归

#### 及时再战半寄

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        /**
        因为要求字符串的顺序，所以只能暴力+深度递归
         */
        //把字符串转化成charArrray备用
        char[] wordch=word.toCharArray();
        //题目规定了board不为空数组
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                //对于每一个数组元素，开始深度递归;最后一个参数k记录匹配到wordch的第几个字符了。如果本路劲为头递归成功，直接返回true，否则以board的盘面的另一个字符开始新递归尝试。
                if(recur(board,wordch,i,j,0))
                    return true;
            }
        }

        //整个board递归完都没效果的话，返回false
        return false;

    }
    /**
    自己构造深度递归函数
     */
    public boolean recur(char[][] board, char[] wordch,int i,int j,int k){
        //判断此路不通，即剪枝。注意先判断ij的范围再判断boardij的值，这样不会超过数组边界。
        if(i<0||i>board.length-1||j<0||j>board[0].length-1||board[i][j]!=wordch[k]){
            return false;
        }
        //如果上面的if没退出，那么说明当前字符比对没问题；这时发现wordch全部比对完毕了，就可以返回true；注意这里要用wordch.length-1，别漏掉-1
        if(k==wordch.length-1){
            return true;
        }

        //空字符串标记当前字符，表示以访问过，防止本轮递归再次访问；数组传递的是地址，函数内修改数组会同时影响函数外；\是x的第一画，称为反斜杠。
        board[i][j]='\0';
        //如果又不能返回，当前字符比对又没错，就进行更深度的递归(朝另外的四个方向，不过来时的方向会一进去会返回false)；因为要修复boardij的值，所以要暂存recur的值
        boolean tempb= recur(board,wordch,i-1,j,k+1)||recur(board,wordch,i+1,j,k+1)||recur(board,wordch,i,j-1,k+1)||recur(board,wordch,i,j+1,k+1);
        //本层次更深的递归结束后，把boardij还原，防止影响上层递归的其他方向;能走到这个位置说明board[i][j]==wordch[k]
        board[i][j]=wordch[k];

        //返回递归的结果
        return tempb;
    }
}
```

- 思路都有，但是犯了两个错误：

  - wordch判断结束长度时用了超过wordch边界的wordch.length，而没正确使用wordch.length-1

  - 忘记用空字符串标记本次递归访问过的字符了，提交完看到错误案例才明白。



### [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

#### 首战寄

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        /**
        暴力深度递归，把访问过的字符标记并且不擦除，最后统计被标记的字符数；可访问块可能被不可访问块包围，造成不可访问的现象。
        不能用boolean数组记录，因为有默认值，无法区分是认为标记的还是默认的；还是用int记录吧
        1：可访问到
        -1：不可访问
        0：被-1包围导致无法被访问
        遍历的时候只考虑1的个数即可
         */
        int [][] board=new int[m][n];
        recur(board,m,n,k,0,0);

        //得到可被到达的数目
        int result=0;
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                System.out.println("当前元素为："+board[i][j]);
                if(board[i][j]==1){
                    result++;
                }
            }
        }

        return result;
    }

    /**
    参数讲解：
    board：全局二维数组
    m：最大行
    n：最大列
    k：限制
    i：当前行
    j：当前列
     */
    public void recur(int [][] board,int m,int n,int k,int i, int j){
        System.out.println("进入了recur，此时i:"+i+" j:"+j);
        //下标不符合要求的，直接返回
        if(i<0||i>m-1||j<0||j>n-1){
            System.out.println("下标不对");
            return;
        }
        //如果已经标记过，说明早被处理过的，直接返回
        if(board[i][j]==1||board[i][j]==-1){
            System.out.println("被处理过");
            return;
        }

        //如果没有被标记过，则处理本元素
        //记录数位之和
        int isum=0,jsum=0;
        while(i/10>0){
            int temp=i%10;
            isum+=temp;
            i/=10;
        }
        //单独处理一下m/10==0时的个位数的m
        isum+=i;
        while(j/10>0){
            int temp=j%10;
            jsum+=temp;
            j/=10;
        }
        //单独处理一下m/10==0时的个位数的m
        jsum+=j;
        //根据数位之和做标记
        if(isum+jsum>k){
            System.out.println("这个不是");
            board[i][j]=-1;
        }else{
            System.out.println("找到了");
            board[i][j]=1;
        }

        //本元素处理完后，递归处理周边元素
        recur(board,m,n,k,i-1,j);
        recur(board,m,n,k,i+1,j);
        recur(board,m,n,k,i,j-1);
        recur(board,m,n,k,i,j+1);
    }
}
```

- 问答问：按照自己的思路，没发现什么错误，也通过了32/49的测试案例；不过m==11,n==8,k==16的案例就没通过
  - 我：问题有两个：
    - `if(isum+jsum>k)`中，既然不符合就该及时返回
    - 最致命的错误：`if(isum+jsum>k)和else`中给`board[i][j]`复制，我这里赋参数的i不是传进来的i，因为中间执行了i自除！！所以出现了m>10就会发生和预期结果不一致，因为m>10后i最大就会超过9，即i会大于等于10，就会出发i自除，就会导致board填数错误。这也就解释了：求数位和的算法没错，为什么最后board的结果还不一致；求和是对的，但是最后board记录的位置错了！。
      - 大神评论：自除这种操作，要小心；有的时候宁可多一个变量也别少一个变量。

根据自己发现的两处错误，略微修改代码如下，即成功提交！：

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        /**
        暴力深度递归，把访问过的字符标记并且不擦除，最后统计被标记的字符数；可访问块可能被不可访问块包围，造成不可访问的现象。
        不能用boolean数组记录，因为有默认值，无法区分是认为标记的还是默认的；还是用int记录吧
        1：可访问到
        -1：不可访问
        0：被-1包围导致无法被访问
        遍历的时候只考虑1的个数即可
         */
        int [][] board=new int[m][n];
        recur(board,m,n,k,0,0);

        //得到可被到达的数目
        int result=0;
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                //System.out.println("当前元素为："+board[i][j]);
                if(board[i][j]==1){
                    result++;
                }
            }
        }

        return result;
    }

    /**
    参数讲解：
    board：全局二维数组
    m：最大行
    n：最大列
    k：限制
    i：当前行
    j：当前列
     */
    public void recur(int [][] board,int m,int n,int k,int i, int j){
        //System.out.println("进入了recur，此时i:"+i+" j:"+j);
        //下标不符合要求的，直接返回
        if(i<0||i>m-1||j<0||j>n-1){
            //System.out.println("下标不对");
            return;
        }
        //如果已经标记过，说明早被处理过的，直接返回
        if(board[i][j]==1||board[i][j]==-1){
            //System.out.println("被处理过");
            return;
        }

        //如果没有被标记过，则处理本元素
        //记录数位之和
        int isum=0,jsum=0;
        //保存ij
        int storedi=i,storedj=j;
        while(i/10>0){
            int temp=i%10;
            isum+=temp;
            i/=10;
        }
        //单独处理一下m/10==0时的个位数的m
        isum+=i;
        while(j/10>0){
            int temp=j%10;
            jsum+=temp;
            j/=10;
        }
        //单独处理一下m/10==0时的个位数的m
        jsum+=j;
        //根据数位之和做标记
        if(isum+jsum>k){
            //System.out.println("这个不是");
            board[storedi][storedj]=-1;
            //别忘了在不合法的时候return
            return;
        }else{
            //System.out.println("找到了");
            board[storedi][storedj]=1;
        }

        //本元素处理完后，递归处理周边元素
        recur(board,m,n,k,storedi-1,storedj);
        recur(board,m,n,k,storedi+1,storedj);
        recur(board,m,n,k,storedi,storedj-1);
        recur(board,m,n,k,storedi,storedj+1);
    }
}
```



#### 高赞大佬评论解法-类似我

官方评论里有一个和我代码类似的，可以对照下看看哪错了：

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        return dfs(0, 0, m, n, k, visited);
    }

    private int dfs(int i, int j, int m, int n, int k, boolean visited[][]) {
        if (i < 0 || i >= m || j < 0 || j >= n || (i/10 + i%10 + j/10 + j%10) > k || visited[i][j]) {
            return 0;
        }
        visited[i][j] = true;
        return dfs(i + 1, j, m, n, k, visited) + dfs(i - 1, j, m, n, k, visited) + 
               dfs(i, j + 1, m, n, k, visited) + dfs(i, j - 1, m, n, k, visited) + 1;
    }
}
```

- 我和网友问：(i/10 + i%10 + j/10 + j%10) 当 i = 100 j =100的时候不就 不成立了吗？
  - 网友答：下标最大是99，所有不会出现100
- 网友问：为什么判断条件还要加上当前这个位置visited[i][j]，不加上就不对，这个时候的这个值没有定义，肯定是false啊
  - 我答：没被访问过为默认的false，所以会往下走；如果被访问过即为true，则会return而不会往下走。

#### 官方-BFS/DFS初始分析

- 我：这两个解法现场很难想到“增量公式”的思路，个人不建议

解题思路：

- 本题与 [矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/solution/mian-shi-ti-12-ju-zhen-zhong-de-lu-jing-shen-du-yo/) 类似，是典型的搜索 & 回溯问题。在介绍回溯算法算法前，为提升计算效率，首先讲述两项前置工作： **数位之和计算** 、 **可达解分析** 。

数位之和计算：

![image-20220405194741529](lcof.assets/image-20220405194741529.png)

```java
int sums(int x)
    int s = 0;
    while(x != 0) {
        s += x % 10;
        x = x / 10;
    }
    return s;

```

![image-20220405194811250](lcof.assets/image-20220405194811250.png)

数位和增量公式:

![image-20220405194840568](lcof.assets/image-20220405194840568.png)

![image-20220405194851009](lcof.assets/image-20220405194851009.png)

以下代码为增量公式的三元表达式写法，将整合入最终代码中。

```java
(x + 1) % 10 != 0 ? s_x + 1 : s_x - 8;
```

可达解分析：

![image-20220405195157201](lcof.assets/image-20220405195157201.png)

![image-20220405195214003](lcof.assets/image-20220405195214003.png)

#### 官方-DFS

![image-20220405195255693](lcof.assets/image-20220405195255693.png)

![image-20220405195324541](lcof.assets/image-20220405195324541.png)

代码：

```java
class Solution {
    int m, n, k;
    boolean[][] visited;
    public int movingCount(int m, int n, int k) {
        this.m = m; this.n = n; this.k = k;
        this.visited = new boolean[m][n];
        return dfs(0, 0, 0, 0);
    }
    public int dfs(int i, int j, int si, int sj) {
        if(i >= m || j >= n || k < si + sj || visited[i][j]) return 0;
        visited[i][j] = true;
        return 1 + dfs(i + 1, j, (i + 1) % 10 != 0 ? si + 1 : si - 8, sj) + dfs(i, j + 1, si, (j + 1) % 10 != 0 ? sj + 1 : sj - 8);
    }
}
```

- Java/C++ 代码中 `visited` 为辅助矩阵，

#### 官方-BFS

![image-20220405195545232](lcof.assets/image-20220405195545232.png)

![image-20220405195559049](lcof.assets/image-20220405195559049.png)

代码：

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        boolean[][] visited = new boolean[m][n];
        int res = 0;
        Queue<int[]> queue= new LinkedList<int[]>();
        queue.add(new int[] { 0, 0, 0, 0 });
        while(queue.size() > 0) {
            int[] x = queue.poll();
            int i = x[0], j = x[1], si = x[2], sj = x[3];
            if(i >= m || j >= n || k < si + sj || visited[i][j]) continue;
            visited[i][j] = true;
            res ++;
            queue.add(new int[] { i + 1, j, (i + 1) % 10 != 0 ? si + 1 : si - 8, sj });
            queue.add(new int[] { i, j + 1, si, (j + 1) % 10 != 0 ? sj + 1 : sj - 8 });
        }
        return res;
    }
}
```

- Java/C++ 代码中 `visited` 为辅助矩阵

#### 及时再战成功

没有用官方的解法，用我+大佬的思路，修改了下判断k的条件就成功了；看来之前确实判断k的算法有漏洞。

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        /**
        暴力深度递归，把访问过的字符标记并且不擦除，最后统计被标记的字符数；可访问块可能被不可访问块包围，造成不可访问的现象。
        不能用boolean数组记录，因为有默认值，无法区分是认为标记的还是默认的；还是用int记录吧
        1：可访问到
        -1：不可访问
        0：被-1包围导致无法被访问
        遍历的时候只考虑1的个数即可
         */
        int [][] board=new int[m][n];
        recur(board,m,n,k,0,0);

        //得到可被到达的数目
        int result=0;
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                System.out.println("当前元素为："+board[i][j]);
                if(board[i][j]==1){
                    result++;
                }
            }
        }

        return result;
    }

    /**
    参数讲解：
    board：全局二维数组
    m：最大行
    n：最大列
    k：限制
    i：当前行
    j：当前列
     */
    public void recur(int [][] board,int m,int n,int k,int i, int j){
        //System.out.println("进入了recur，此时i:"+i+" j:"+j);
        //下标不符合要求的，直接返回
        if(i<0||i>m-1||j<0||j>n-1){
            //System.out.println("下标不对");
            return;
        }
        //如果已经标记过，说明早被处理过的，直接返回
        if(board[i][j]==1||board[i][j]==-1){
            //System.out.println("被处理过");
            return;
        }

        // //如果没有被标记过，则处理本元素
        // //记录数位之和
        // int isum=0,jsum=0;
        // while(i/10>0){
        //     int temp=i%10;
        //     isum+=temp;
        //     i/=10;
        // }
        // //单独处理一下m/10==0时的个位数的m
        // isum+=i;
        // while(j/10>0){
        //     int temp=j%10;
        //     jsum+=temp;
        //     j/=10;
        // }
        // jsum+=j;
        // //根据数位之和做标记
        // if(isum+jsum>k){
        //     System.out.println("这个不是");
        //     board[i][j]=-1;
        // }else{
        //     System.out.println("找到了");
        //     board[i][j]=1;
        // }
        if(i/10 + i%10 + j/10 + j%10>k){
            board[i][j]=-1;
            return ;
        }else{
            board[i][j]=1;
        }


        //本元素处理完后，递归处理周边元素
        recur(board,m,n,k,i-1,j);
        recur(board,m,n,k,i+1,j);
        recur(board,m,n,k,i,j-1);
        recur(board,m,n,k,i,j+1);
    }
}
```

- 注意：题目说m和n可以为100，但是数组下标为m-1和n-1，最多2位数。

- 之后推荐“高赞大佬评论解法”的做法，效率高：
  - 他只用boolean标记已访问，是因为它直接用dfs返回值了，说明本身返回的值就是所有可达元素的值；但是我这解法要遍历一遍boolean数组，不知道有哪些是不可达（被不合法包围了导致过不去），有哪些是可达但不合法，所以我至少需要三个状态来记录，boolean就不合适了。

- 至于为什么自己的算法不行，参见“首战寄”

### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    //建立全局列表便于操作
    List<List<Integer>>result=new LinkedList<>();
    public List<List<Integer>> pathSum(TreeNode root, int target) {
        /**
        5000*1000<2的9次方，所以不用担心数组值求和导致超过整数边界

        初步思路：dfs+剪枝
         */
        recur(root,target,new LinkedList<>());

        return result;
    }
    public void recur(TreeNode root, int target, Deque<Integer> way){
        //返回值处理  
        if(root==null){//注意节点可以为负数，所以不能用target<root.val判断路径不通;此步还可以防止后面操作空指针异常。
            return;
        }else if(target==root.val&&root.left==null&&root.right==null){//找到合理路径;注意单单target==root.val还不够，题目要求是从根节点到叶子结点的路径
            //把本元素压入way，再把way压入全局result中;把linkedlist实现的deque强转为list
            way.offerLast(root.val);
            //!!!不能直接result.add((List<Integer>)way);存节点，这样way中修改增删节点会影响result，应该用new另给空间成立新对象。
            result.add(new LinkedList<>((List<Integer>)way));
            //把way中本元素取出来，避免影响其他兄弟枝的递归
            way.pollLast();
            //找到合理路劲表示当前节点是叶子节点，可以直接返回，不需要再对左右子树做递归了
            return;
        }
        //以上都没返回，说明此路径还可以再递归试试
        way.offerLast(root.val);
        recur(root.left,target-root.val,way);
        recur(root.right,target-root.val,way);
        //本节点相关的递归处理完了，从路径中拿出本节点，防止影响其他兄弟枝的递归
        way.pollLast();

    }
}
```

- 注意点：
  - 全局变量往往会让事情简单一点，不用各种传参和考虑返回，容易绕晕。
  - 不能直接result.add((List\<Integer>)way);存节点，这样way中修改增删节点会影响result，应该用new另给空间成立新对象。LinkedList和ArrayList的构造函数都支持传入一个列表，表示根据列表的内容新建本列表。（这个在官方解析中也提到了）
  - 递归前往列表放本节点的值，递归，递归后拿出本节点往列表放的值，（处理并返回）
    - 可以利用deque的特性，精准从尾部往链表增删节点。
- 反思：
  - 我无意中正用了dfs-先序遍历，即按照 “根、左、右” 的顺序，遍历树的所有节点。

#### 官方

解题思路：本问题是典型的二叉树方案搜索问题，使用回溯法解决，其包含 先序遍历 + 路径记录 两部分。

- 先序遍历： 按照 “根、左、右” 的顺序，遍历树的所有节点。
- 路径记录： 在先序遍历中，记录从根节点到当前节点的路径。当路径为 ① 根节点到叶节点形成的路径 且 ② 各节点值的和等于目标值 sum 时，将此路径加入结果列表。

![image-20220406145505142](lcof.assets/image-20220406145505142.png)

算法流程：

- pathSum(root, sum) 函数：
  - 初始化： 结果列表 res ，路径列表 path 。
  - 返回值： 返回 res 即可。
- recur(root, tar) 函数：
  - 递推参数： 当前节点 root ，当前目标值 tar 。
  - 终止条件： 若节点 root 为空，则直接返回。
  - 递推工作：
    1. 路径更新： 将当前节点值 root.val 加入路径 path ；
    2. 目标值更新： tar = tar - root.val（即目标值 tar 从 sum 减至 00 ）；
    3. 路径记录： 当 ① root 为叶节点 且 ② 路径和等于目标值 ，则将此路径 path 加入 res 。
    4. 先序遍历： 递归左 / 右子节点。
    5. 路径恢复： 向上回溯前，需要将当前节点从路径 path 中删除，即执行 path.pop() 。

复杂度分析：

- 时间复杂度 O(N)： N为二叉树的节点数，先序遍历需要遍历所有节点。
- 空间复杂度 O(N)： 最差情况下，即树退化为链表时，path 存储所有树节点，使用 O(N)额外空间。

代码：

```java
class Solution {
    LinkedList<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> path = new LinkedList<>(); 
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        recur(root, sum);
        return res;
    }
    void recur(TreeNode root, int tar) {
        if(root == null) return;
        path.add(root.val);
        tar -= root.val;
        if(tar == 0 && root.left == null && root.right == null)
            res.add(new LinkedList(path));
        recur(root.left, tar);
        recur(root.right, tar);
        path.removeLast();
    }
}
```

- 值得注意的是，记录路径时若直接执行 res.append(path) ，则是将 path 对象加入了 res ；后续 path 改变时， res 中的 path 对象也会随之改变。
  - 正确做法：res.append(list(path)) ，相当于复制了一个 path 并加入到 res 。

- 网友评论：剪枝剪了个寂寞。。。。到 叶子 结点！！！

- 我：本例用一对增删节点包裹整个recur方法内容的方式值得借鉴；我自己写的方法，做了两处增删节点操作，不那么优雅了。



### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

#### 首战寄

只有大概思路，但是没有跑通：

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    int min=Integer.MAX_VALUE;
    public Node treeToDoublyList(Node root) {
        /**
        中序遍历，可以实现节点从小到大阅读
        二叉搜索树：左子树小于本节点，右子树大于本节点
        递归返回的时候，正是二叉搜索树的指针能逆向的时机（返回本节点，到上一层递归让被返回的节点指向上一层递归的节点）
        首尾节点要单独记录才能连上，但是这里要求不能新建节点，就有点难搞。（因为双向列表是有序的，找到最小的节点就能找到最大的节点，就能通过右指针找到最大的节点，中间记录经过节点数，while(节点数)来连上收尾节点，这里只是获得收尾节点的引用，所以没新建节点）
         */
        //返回的是链表中最大的节点,一直查找左指针即可拿到最小节点，最小节点的左指针为null，右指针指向倒数第2小的节点
        Node max=recur(root);
        Node min=null,maxStored=max;//min显式设置为null，防止编译不通过，说“min的值无”
        while(max!=null){
            System.out.println(max.val);
            if(max.left==null){//找到当前节点为最小节点
                min=max;
            }
            max=max.left;
        }
        //把min的左指针练到maxStored，把maxStored的右指针连到min
        min.left=maxStored;
        maxStored.right=min;

        //返回链表第一个节点
        return min;
        
    }
    public Node recur(Node root){
        //递归最重要的：设置返回条件
        //设置叶子节点返回
        if(root.left==null&&root.right==null)return root;
        //中序遍历，先遍历左节点，拿到左节点
        Node left=recur(root.left);
        //让左子节点右指针指向自己(右指针比自己大)
        left.right=root;
        if(root.right!=null){
            root.right.left=root;
        }
        
        //得到的右子节点，返回给当前节点的父节点，因为右子节点在当前节点和当前节点的父节点之间
        return recur(root.right);
    }
}
```

- 虽然没有做出来，但是我抓住了：
  - 二叉搜索树的中序遍历为 **递增序列** 。
  - 思路和题解一致

#### 官方-中序遍历

解题思路：

- 本文解法基于性质：二叉搜索树的中序遍历为 递增序列 。
- 将 二叉搜索树 转换成一个 “排序的循环双向链表” ，其中包含三个要素：
  - 排序链表： 节点应从小到大排序，因此应使用 中序遍历 “从小到大”访问树的节点。
  - 双向链表： 在构建相邻节点的引用关系时，设前驱节点 pre 和当前节点 cur ，不仅应构建 pre.right = cur ，也应构建 cur.left = pre 。
  - 循环链表： 设链表头节点 head 和尾节点 tail ，则应构建 head.left = tail 和 tail.right = head 。

![image-20220406173635111](lcof.assets/image-20220406173635111.png)

**中序遍历** 为对二叉树作 “左、根、右” 顺序遍历，递归实现如下：

```java
// 打印中序遍历
void dfs(Node root) {
    if(root == null) return;
    dfs(root.left); // 左
    System.out.println(root.val); // 根
    dfs(root.right); // 右
}
```

根据以上分析，考虑使用中序遍历访问树的各节点 cur ；并在访问每个节点时构建 cur 和前驱节点 pre 的引用指向；中序遍历完成后，最后构建头节点和尾节点的引用指向即可。

算法流程：

- dfs(cur): 递归法中序遍历；

  1. 终止条件： 当节点 cur 为空，代表越过叶节点，直接返回；

  2. 递归左子树，即 dfs(cur.left) ；
  3. 构建链表：
     1. 当 pre 为空时： 代表正在访问链表头节点，记为 head ；
     2. 当 pre 不为空时： 修改双向节点引用，即 pre.right = cur ， cur.left = pre ；
     3. 保存 cur ： 更新 pre = cur ，即节点 cur 是后继节点的 pre ；
  4. 递归右子树，即 dfs(cur.right) ；

- treeToDoublyList(root)：
  1. 特例处理： 若节点 root 为空，则直接返回；
  2. 初始化： 空节点 pre ；
  3. 转化为双向链表： 调用 dfs(root) ；
  4. 构建循环链表： 中序遍历完成后，head 指向头节点， pre 指向尾节点，因此修改 head 和 pre 的双向节点引用即可；
  5. 返回值： 返回链表的头节点 head 即可；

复杂度分析：

- 时间复杂度 O(N)： N为二叉树的节点数，中序遍历需要访问所有节点。
- 空间复杂度 O(N)： 最差情况下，即树退化为链表时，递归深度达到 N，系统使用 O(N)栈空间。

代码：

```java
class Solution {
    Node head, pre;
    public Node treeToDoublyList(Node root) {
        if(root==null) return null;
        dfs(root);

        pre.right = head;
        head.left =pre;//进行头节点和尾节点的相互指向，这两句的顺序也是可以颠倒的

        return head;

    }

    public void dfs(Node cur){
        if(cur==null) return;
        dfs(cur.left);

        //pre用于记录双向链表中位于cur左侧的节点，即上一次迭代中的cur,当pre==null时，cur左侧没有节点,即此时cur为双向链表中的头节点
        if(pre==null) head = cur;
        //反之，pre!=null时，cur左侧存在节点pre，需要进行pre.right=cur的操作。
        else pre.right = cur;
       
        cur.left = pre;//pre是否为null对这句没有影响,且这句放在上面两句if else之前也是可以的。

        pre = cur;//pre指向当前的cur
        dfs(cur.right);//全部迭代完成后，pre指向双向链表中的尾节点
    }
}
```

#### 即时再战成功

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val,Node _left,Node _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
    Node prev=null,min=null;
    public Node treeToDoublyList(Node root) {
        /**
        二叉搜索树，中序遍历得到递增的数
        通过全局指针，处理pre和cur的关系；由于搜索二叉树的基础，右指针指向比自己大的，左指针指向比自己小的
         */
        //处理恶心人的特殊情况[],传入空节点时，不做下面的处理的话，prev不会被赋值，会导致prev.riht报空指针异常
        if(root==null){
            return null;
        }
        //dfs-中序遍历 返回的后，prev为链表中最大的值
        dfs(root);
        //把双向链表的头尾连接
        prev.right=min;
        min.left=prev;

        //min指向链表最小值，返回min即为头
        return min;
    }
    
    public void dfs(Node cur){
        //设置返回条件：节点超过叶子节点时
        if(cur==null){
            return;
        }
        //中序遍历：先访问左子节点
        dfs(cur.left);
        //中序遍历：再访问本节点，处理本节点相关事宜
        //prev即为值比当前节点更小的节点：可能是当前节点的左子节点（如2的1），也可能是当前节点的左子节点的右子节点（如4的3）
        if(prev==null){//prev为空表示没有更小的数，也即当前节点为最小的数,记录保存下来
            min=cur;
        }else{//如果当前节点不是最小的节点，那么就处理它和之前节点的关系:每次都是处理当前节点和之前更小节点之间的关系，够成链
            prev.right=cur;
            cur.left=prev;
        }
        //要离开本节点了，更新prev节点为当前节点
        prev=cur;
        //中序遍历：最后递归遍历右子树
        dfs(cur.right);

    }
}
```

- 我和官方有一个点不一样，就是`cur.left=prev;`:
  - 我把cur.left放到else里，是只有非链表首节点才需要处理cur.left
  - 官方把cur.left放到ifelse外，相当于在if里和else里都放置了cur.left；但是if时当前节点为链表首节点，且此时无法知道链表尾节点，所以其实不需要在if中放`cur.left=prev;`，这句话在if也就相当于是把链表首节点本来就是null的左子指针又赋值为null；这么看，我的”即时再战“版的代码更合理，没做无意义操作。



### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    List<Integer> vals=new ArrayList<>();
    public int kthLargest(TreeNode root, int k) {
        /**
        理想：二叉搜索树中序遍历得到的是递增的数;利用退栈记录倒数第k个数，但是中序退栈的话就不会回本节点了；全局变量记录目标值即可。
        现实：ArrayList填入结果，再取出。不建议使用deque存储值，再倒序遍历(把deque当栈用)，因为不需要遍历，直接根据下标拿即可，而且linkedlist查找元素效率低。
        deque用offerlast或offerfirst，arraylist就用add即加到链表尾。
         */

        //处理特殊情况(本例应该不会有这个情况)
        if(root==null)return -1;

        //利用dfs获得递增链表
        dfs(root);
        //从递增链表中拿到结果

         return vals.get(vals.size()-k);

    }
    public void dfs(TreeNode root){
        //设定dfs返回条件
        if(root==null)return;
        
        //中序遍历：左
        dfs(root.left);
        
        //中序遍历：本节点
        //存值
        vals.add(root.val);
        
        //中序遍历：右
        dfs(root.right);

    }
}
```

- 性能比较差，时间空间复杂度都是后30%

- 我之前想：二叉搜索树中序遍历得到的是递增的数，全局变量记录一下当前遍历了几个节点即可；但是本题要第k大的数，即升序链表倒数第k个数，但是中序遍历应该又不能在退栈的时候处理节点（中序遍历处理节点的操作，在对左右子树dfs之间）；所以只能用本例这个笨方法。
- 看了“大佬解法”，看来我和最优解很接近了，改成先右再左的中序，就可以得到递减的遍历！

#### 大佬-中序dfs+递减

作为一个普通人，我来分析下这题。

1. 假设，你花了点时间，练习了二叉树的三种遍历方式： a. 前序遍历 b. 中序遍历 c. 后续遍历
2. 你也学习了二叉搜索树，深入研究了二叉树搜索树的特性，并且深刻知道二叉搜索树的一个特性：通过中序遍历所得到的序列，就是有序的。

好，有了以上两点知识，我认为你必须能想到（如果想不到，以上两点知识肯定没有学扎实）：中序遍历二叉搜索树，遍历的同时，把遍历到的节点存到一个可变数组里（Java的话，可以用ArrayList)。 思路转化为代码，如下：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int kthLargest(TreeNode root, int k) {
        // clarification:  root == null?   k <= 1?
        List<Integer> list = new ArrayList<>();
        helper(root, list);
        return list.get(list.size() - k);
    }
    
    private void helper(TreeNode root, List<Integer> list) {
        if (root == null) return;
        if (root.left != null) helper(root.left, list);
        list.add(root.val);
        if (root.right != null) helper(root.right, list);
    }
}
```

到这里，这题，我觉得已经离最优解很近了。作为一个学习了算法的人，你一定会想：上面的算法空间复杂度是O(n)，可不可以优化？

深入再思考下，其实，当遍历到了第K大数的时候，就可以停止遍历了，同时，把遍历到节点对应的数保存下来即可。

想到这里，一定能写出下面的代码：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int ans = 0, count = 0;
    public int kthLargest(TreeNode root, int k) {
        // clarification:  root == null?   k <= 1?
        helper(root, k);
        return ans;
    }
    
    private void helper(TreeNode root, int k) {
        if (root.right != null) helper(root.right, k);
        
        if (++count == k) {
            ans = root.val;
            return;
        }
        
        if (root.left != null) helper(root.left, k);
    }
}
```

第二种方法返回了++count == k的情况，但实际上并没有 `停止` 遍历；可以用题目中的第二个测试用例输出一下count，发现count还在增加的；这是因为得到结果后，++count发现不符合后，会进入left的遍历，我们应该让得到结果后不再进入left递归，改良代码如下：

```java
class Solution {
    int n = 0;
    int res = 0;
    public int kthLargest(TreeNode root, int k) {
        n = k;
        helper(root);
        return res;
    }
    public void helper(TreeNode root){
        if(root.right != null && n>0 )  helper(root.right);
        n--;
        if(n==0){
            res = root.val;
            return;
        }
        if(root.left != null && n>0 )  helper(root.left);
    }
}
```

- n>0会阻止得到目标结果后的遍历。
- 我：他的对节点为null的判断可以像我一样，进入节点后再判断，这样if判断的内容就会少些，更清晰。

这题到这就比较完美了。

#### 官方-中序遍历+提前返回

解题思路：

本文解法基于此性质：二叉搜索树的中序遍历为 递增序列 。

- 根据以上性质，易得二叉搜索树的 中序遍历倒序 为 递减序列 。
- 因此，求 “二叉搜索树第 k大的节点” 可转化为求 “此树的中序遍历倒序的第 k个节点”。

![Picture1.png](lcof.assets/4ebcaefd4ecec0d76bfab98474dfed323fb86bfcd685d1a5bf610200fdca4405-Picture1.png)

中序遍历 为 “左、根、右” 顺序，递归法代码如下：

```java
// 打印中序遍历
void dfs(TreeNode root) {
    if(root == null) return;
    dfs(root.left); // 左
    System.out.println(root.val); // 根
    dfs(root.right); // 右
}
```

中序遍历的倒序 为 “右、根、左” 顺序，递归法代码如下：

```java
// 打印中序遍历倒序
void dfs(TreeNode root) {
    if(root == null) return;
    dfs(root.right); // 右
    System.out.println(root.val); // 根
    dfs(root.left); // 左
}
```

为求第 k 个节点，需要实现以下 三项工作 ：

1. 递归遍历时计数，统计当前节点的序号；
2. 递归到第 kk个节点时，应记录结果 resres ；
3. 记录结果后，后续的遍历即失去意义，应提前终止（即返回）。

递归解析：

![image-20220407152836167](lcof.assets/image-20220407152836167.png)

复杂度分析：

![image-20220407152901363](lcof.assets/image-20220407152901363.png)

代码：

![image-20220407153000649](lcof.assets/image-20220407153000649.png)

```java
class Solution {
    int res, k;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        dfs(root);
        return res;
    }
    void dfs(TreeNode root) {
        if(root == null) return;
        dfs(root.right);
        if(k == 0) return;
        if(--k == 0) res = root.val;
        dfs(root.left);
    }
}
```

- 我：官方代码就用了`if(k == 0) return;`做了提前返回，比“大佬解法”的代码更简洁易读；不过“大佬解法”的优化思路还是很值得借鉴的。

## 排序(简单)

### [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

#### 首战寄

应该是按照数字最高位去降序排序，没思路

- 网友评：这道题似乎变成了lambda讨论会，其实我觉得a+b < b+a 真的好难想
- 网友：我在纸上反复写，找规律，脑子都要炸了，然后突然发现，就是在比较两个字符串不同顺序相加的结果，然后就豁然开朗。
- 网友：华为机试+第一轮技术面的题，有点高频。

#### 官方-自定义排序

解题思路：

![image-20220407155555720](lcof.assets/image-20220407155555720.png)

排序规则的传递性证明：

- 这里是证明，不然上面的解释太牵强了，此题解作者估计也不真正明白为什么可以这样做，这道题目其实属于很难的题目，不能让人信服为什么那样排序一下就可以，也不是简单的 x +y > y +x 则 x > y 譬如 x=6, y=305 x+y(6305) > y+x(3056) 但是真实数字上x < y

为什么这样排个序就可以了呢？简单证明一下。根据算法，如果a < b，那么a排在b前面，否则b排在a前面。可利用反证法，假设排成的最小数字为xxxxxx，并且至少存在一对字符串满足这个关系：a > b，但是在组成的数字中a排在b前面。根据a和b出现的位置，分三种情况考虑：

```
  （1）xxxxab，用ba代替ab可以得到xxxxba，这个数字是小于xxxxab，与假设矛盾。因此排成的最小数字中，不存在上述假设的关系。

  （2）abxxxx，用ba代替ab可以得到baxxxx，这个数字是小于abxxxx，与假设矛盾。因此排成的最小数字中，不存在上述假设的关系。

  （3）axxxxb，这一步证明麻烦了一点。可以将中间部分看成一个整体ayb，则有ay < ya，yb < by成立。将ay和by表示成10进制数字形式，则有下述关系式，这里a，y，b的位数分别为n，m，k。

    关系1： ay < ya => a * 10^m + y < y * 10^n + a => a * 10^m - a < y * 10^n - y => a( 10^m - 1)/( 10^n - 1) < y

    关系2： yb < by => y * 10^k + b < b * 10^m + y => y * 10^k - y < b * 10^m - b => y < b( 10^m -1)/( 10^k -1) 

    关系3： a( 10^m - 1)/( 10^n - 1) < y < b( 10^m -1)/( 10^k -1)  => a/( 10^n - 1)< b/( 10^k -1) => a*10^k - a < b * 10^n - b =>a*10^k + b < b * 10^n + a => a < b

   这与假设a > b矛盾。因此排成的最小数字中，不存在上述假设的关系。

   综上所述，得出假设不成立，从而得出结论：对于排成的最小数字，不存在满足下述关系的一对字符串：a > b，但是在组成的数字中a出现在b的前面。从而得
```

- 根据本证明，只要列表排序后为xyz，且保证xy<yx,yz<zy，就可以保证xyz是用【x y z】三个整数能实现的最小的拼接数；所以我们就自定义Comparator把【x y z】按照“若xy<yx，则x必须放y前面”的规则排序好即可。

算法流程：

![image-20220407155615917](lcof.assets/image-20220407155615917.png)

复杂度分析：

![image-20220407155640803](lcof.assets/image-20220407155640803.png)

代码：

本文列举 快速排序 和 内置函数 两种排序方法，其他排序方法也可实现。

快速排序：

- 需修改快速排序函数中的排序判断规则。字符串大小（字典序）对比的实现方法：
  - Java 中使用函数 A.compareTo(B)；

```java
public String minNumber(int[] nums) {
    String[] strs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) {
        strs[i] = String.valueOf(nums[i]);
    }
    quickSort(strs, 0, strs.length - 1);
    StringBuilder res = new StringBuilder();
    for (String s : strs)
        res.append(s);
    return res.toString();
}

public void quickSort(String[] strs, int low, int high) {
    if (low < high) {
        int middle = getMiddle(strs, low, high);
        quickSort(strs, low, middle - 1);
        quickSort(strs, middle + 1, high);
    }
}

public int getMiddle(String[] strs, int low, int high) {
    //数组的第一个数为基准元素
    String temp = strs[low];
    while (low < high) {
        //从后向前找比基准小的数
        while (low < high && (strs[high] + temp).compareTo(temp + strs[high]) >= 0)
            high--;
        //把比基准小的数移到低端
        strs[low] = strs[high];
        //从前向后找比基准大的数
        while (low < high && (strs[low] + temp).compareTo(temp + strs[low]) <= 0)
            low++;
        //把比基准大的数移到高端
        strs[high] = strs[low];
    }
    strs[low] = temp;
    return low;
}
```

- 网友评：你这个写法像左神。

内置函数：

- 需定义排序规则：
  - Java 定义为 (x, y) -> (x + y).compareTo(y + x) ；

```java
class Solution {
    public String minNumber(int[] nums) {
        //构建字符串数组
        String[] strs = new String[nums.length];
        for(int i = 0; i < nums.length; i++)
            strs[i] = String.valueOf(nums[i]);
        //排序字符串数组
        Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
        //合并字符串数组
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
}
```

- Arrays.sort方法使用，[参考](https://blog.csdn.net/ted_cs/article/details/82713706)；本例用的 Compareable接口，而不是Compartor接口。
  - 但是我看官方的sort接口是传入Comparator接口的实现类，我还是安装java官方的来吧，即自己传Comparator。

- [Java中java.util.Comparator用法](https://www.cnblogs.com/XDU-Lakers/p/13985258.html)
  - 重写方法`public int compare(String o1, String o2) `，凡是返回1或者大于0的正数的时候就要交换位置
  - Comparator是一个函数式接口，所以能用lamda表达式表示。

#### 即时再战半寄

```java
class Solution {
    public String minNumber(int[] nums) {
        /**
        核心就是要把数组，按照首位排序（升序）；首位一样的比较第二位。但是有一种简单的算法：
        1. 把整数数组转化为字符串数组
        2. 把字符串数组排序。例如x=="1"且y=="45"，若x+y<y+x，即认为x<y
        3. 把字符串数组排好升序，就意味着
         */
        //整数数组转换为字符串数组。新建数组列表的末尾不需要括号，别写成new String[nums.length]()
        String [] nstr=new String[nums.length];
        for(int i=0;i<nums.length;i++){
            nstr[i]=nums[i]+"";
        }
        //排序数组，使用sort+Comparator接口。注意匿名内部类的写法，要把重写方法写在花括号{}里。
        Arrays.sort(nstr,new Comparator<String>(){
            //重写public compare方法，指示元素间大小关系的判断。注解首字母大写，参照@Autowired
            @Override
            public int compare(String s1,String s2){
                //防止超过整数边界
                long s1s2= Long.valueOf(s1+s2).longValue();
                long s2s1= Long.valueOf(s2+s1).longValue();
                if(s1s2<s2s1){//返回-1，表示不交换s1和s2的值，因为此时s1本来就小于s2
                    return -1;
                }else if(s1s2==s2s1){
                    return 0;
                }else{
                    return 1;//返回1表示交换s1和s2的值
                }
            }
        });
        //把排序后的数组转换成字符串,StringBuilder线程不安全，但是效率高些
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<nstr.length;i++){
            sb.append(nstr[i]);
        }
        return sb.toString();
    }
}
```

- 思路和代码主题没问题，但是有几个知识点没记牢，靠百度和debug解决的：
  - 新建字符数组用`String [] nstr=new String[nums.length];`，而不是String [] nstr=new String[nums.length]\();就是注意不应该有小括号。
  - Comparator.compare()方法是public的；并且此方法要重写在匿名内部类的花括号中，用lamda表达式也可以，但是个人感觉可读性差。
  - 理解“若x+y<y+x，即认为x<y，按照此规则排序完数组后为什么就能让拼接成的”字符串值最小

- 自我表扬：
  - 记住了注解首字母大写
  - 用`long`避免超过整数边界，用`Long.valueOf(obj)`把字符串转化为Long(long的包装类)，用`Long.longValue();`把Long转化成基本数据类型long。
  - 基本能自主写出Arrays.sort自定义排序。

### [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

#### 首战告捷

```java
class Solution {
    public boolean isStraight(int[] nums) {
        /**
        排序，查gap和0的关系
         */

        //题目说抽5张牌，所以不需要处理特殊情况

        //排序数组，默认升序
        Arrays.sort(nums);

        //遍历排序后的数组，记录gap和0
        int zNum=0,lastNum=-1,gapNum=0,i=0;
        //记录0的个数
        while(nums[i]==0){
            zNum++;
            i++;
        }
        //记录gap的总长度
        lastNum=nums[i];//保存第一个非0的数，作为lastNum
        i++;
        while(i<nums.length){
            if(nums[i]-lastNum>1){//侦查到gap
                gapNum+=(nums[i]-lastNum-1);
            }else if(nums[i]==lastNum){//考虑数目相同的情况，只要出现非0数目相同，即不是扑克牌的顺子，直接返回false。之前以为根据题意，不用考虑非0数多次出现。。失策了
                return false;
            }

            //不管有没有侦查到gap，都更新lastNum和i
            lastNum=nums[i];
            i++;
        }

        return gapNum<=zNum;

    }
}
```

- 初始思路：升序排序数组；遍历数组，碰到0记录下总数，碰到gap区记录下长度。遍历完毕后，如果gap的总长度<=0的个数，即顺子。
- 注意：虽然题目没有说明白“牌连续”的概念，但是扑克牌里，手牌中有非0重复的牌的话，不算顺子。

#### 官方1-set集合+遍历

解题思路：

![image-20220408165259162](lcof.assets/image-20220408165259162.png)

集合 Set + 遍历，做法：

- 遍历五张牌，遇到大小王（即 0 ）直接跳过。
- **判别重复**： 利用 Set 实现遍历判重， Set 的查找方法的时间复杂度为 O(1)；
- **获取最大 / 最小的牌**： 借助辅助变量 ma 和 mi，遍历统计即可。

复杂度分析：

![image-20220408165525298](lcof.assets/image-20220408165525298.png)

代码：

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Set<Integer> repeat = new HashSet<>();
        int max = 0, min = 14;
        for(int num : nums) {
            if(num == 0) continue; // 跳过大小王
            max = Math.max(max, num); // 最大牌
            min = Math.min(min, num); // 最小牌
            if(repeat.contains(num)) return false; // 若有重复，提前返回 false
            repeat.add(num); // 添加此牌至 Set
        }
        return max - min < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```

#### 官方2-排序 + 遍历

![image-20220408165708980](lcof.assets/image-20220408165708980.png)

复杂度分析：

![image-20220408165727381](lcof.assets/image-20220408165727381.png)

代码：

```java
class Solution {
    public boolean isStraight(int[] nums) {
        int joker = 0;
        Arrays.sort(nums); // 数组排序
        for(int i = 0; i < 4; i++) {
            if(nums[i] == 0) joker++; // 统计大小王数量
            else if(nums[i] == nums[i + 1]) return false; // 若有重复，提前返回 false
        }
        return nums[4] - nums[joker] < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
    }
}
```

- 我和网友：max - min < 5的思路很赞！这样效率就比我硬执行高。

### [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

#### 首战告捷

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        /** 
        数组splice(闭,开)表示切数组；类似的有String用substring切
        */
        //快排
        Arrays.sort(arr);
        
        //return arr.splice(0,k);这个函数怎么用？
        int[] result=new int[k];
        for(int i=0;i<k;i++){
            result[i]=arr[i];
        }

        return result;
    }
}
```

- 反省：忘记了数组切割的函数

#### 官方-单纯快速排序

本题使用排序算法解决最直观，对数组 arr 执行排序，再返回前 k个元素即可。使用任意排序算法皆可，本文采用并介绍 快速排序 ，为下文 方法二 做铺垫。

快速排序原理：

- 快速排序算法有两个核心点，分别为 “哨兵划分” 和 “递归” 。

哨兵划分操作： 

- 以数组某个元素（一般选取首元素）为 基准数 ，将所有小于基准数的元素移动至其左边，大于基准数的元素移动至其右边。

> 如下图所示，为哨兵划分操作流程。通过一轮 哨兵划分 ，可将数组排序问题拆分为 两个较短数组的排序问题 （本文称之为左（右）子数组）。

![image-20220408174558724](lcof.assets/image-20220408174558724.png)

- 原版的这个动图不错

递归： 对 左子数组 和 右子数组 递归执行 哨兵划分，直至子数组长度为 1 时终止递归，即可完成对整个数组的排序。

> 如下图所示，为示例数组 [2,4,1,0,3,5] 的快速排序流程。观察发现，快速排序和 二分法 的原理类似，都是以 \loglog 时间复杂度实现搜索区间缩小。

![Picture1.png](lcof.assets/1612615552-rifQwI-Picture1.png)

复杂度分析：

![image-20220408174755183](lcof.assets/image-20220408174755183.png)

- [参考：快排时间复杂度分析](https://zhuanlan.zhihu.com/p/341201904)

代码：

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        quickSort(arr, 0, arr.length - 1);
        return Arrays.copyOf(arr, k);
    }
    private void quickSort(int[] arr, int l, int r) {
        // 子数组长度为 1 时终止递归
        if (l >= r) return;
        // 哨兵划分操作（以 arr[l] 作为基准数）
        int i = l, j = r;
        while (i < j) {
            while (i < j && arr[j] >= arr[l]) j--;
            while (i < j && arr[i] <= arr[l]) i++;
            swap(arr, i, j);
        }
        swap(arr, i, l);
        // 递归左（右）子数组执行哨兵划分
        quickSort(arr, l, i - 1);
        quickSort(arr, i + 1, r);
    }
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```

- 在选最左边的数为哨兵的前提下，因为先whilej--，再whilei++，导致了最后一步总是j早就停在了一个比哨兵小的数，而i主动靠过来导致ij重合；所以重合的地方应该归左边，即交换哨兵和重合位置的元素。（看动图看的更清晰）
- 注意：这里Arrays.copyOf(arr, k)有截取数组的功能，k表示截取前k个数。

#### 官方-基于快速排序的数组划分

优化思路：

![image-20220408175159330](lcof.assets/image-20220408175159330.png)

算法流程：

![image-20220408175449452](lcof.assets/image-20220408175449452.png)

复杂度分析：

![image-20220408175517282](lcof.assets/image-20220408175517282.png)

代码：

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k >= arr.length) return arr;
        return quickSort(arr, k, 0, arr.length - 1);
    }
    private int[] quickSort(int[] arr, int k, int l, int r) {
        int i = l, j = r;
        while (i < j) {
            while (i < j && arr[j] >= arr[l]) j--;
            while (i < j && arr[i] <= arr[l]) i++;
            swap(arr, i, j);
        }
        swap(arr, i, l);
        if (i > k) return quickSort(arr, k, l, i - 1);
        if (i < k) return quickSort(arr, k, i + 1, r);
        return Arrays.copyOf(arr, k);
    }
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```

### [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

#### 首战半寄

思路对，有人用我这种暴力法也过了，可能我这暴力中间有些多余步骤，导致超时；不超时的暴力可以在下面看到。

```java
class MedianFinder {

    /** initialize your data structure here. */
    //使用列表存储数据，每次取中位数之前排序一下
    List<Integer> list=null; 
    int lsSize=0;//自己保存列表大小，省的每次都on查找列表大小
    public MedianFinder() {
        list=new ArrayList<>();
    }
    
    public void addNum(int num) {
        list.add(num);
        lsSize++;
    }
    
    public double findMedian() {
        //排序；列表
        list.sort(new Comparator<Integer>(){
            @Override
            public int compare(Integer o1,Integer o2){
                if(o1<=o2){//本身升序的话，不交换位置
                    return -1;
                }else{
                    return 1;//1表示交换位置
                }
            }
        });

        //拿中位数
        if(lsSize%2==0){
            //拿出数，并把数转为double
            double templ=(double)list.get(lsSize/2-1).intValue();
            double tempr=(double)list.get(lsSize/2).intValue();
            return templ/2+tempr/2;//防止超过边界
        }else{
            return (double)list.get(lsSize/2).intValue();
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

- 失败原因：超时。我的算法对于输入不多的时候都ok，但是对于长输入会有超时；估计是因为长列表排序太消耗时间了。

- 注意：

  - 我这方法每次插入都得排序列表。像Arrays.sort()只能排序数组而不能排序list，想排序列表ls就可以用ls.sort(new Comparator\<T>(){})；或者用`Collections.sort(List<T> list, Comparator<? super T> c)`。
    - 注意本匿名内部类Comparator，”尖括号，小括号，花括号“都不能少。
    - 我：还是喜欢**Collections的静态方法sort**一点，因为这个方法和Arrays.sort的样式统一；java暴力算法过的老哥也是用这个方法。

  - 官方解法用来堆排序，[堆排序实战参考](https://blog.csdn.net/qq_41682302/article/details/95910651)，这里介绍的堆排序实战和官方使用的PriorityQueue一致！

  - 官方用的堆排序，和我自己用的快速排序的[对比](https://www.cnblogs.com/gzshan/p/10904254.html)，为什么使用堆排序？
    - 我理解：
      - 使用插入排序的话，虽然每次插入耗时间但是取数据节省时间；数组实现插入排序的时间复杂度是on2，取数据实现复杂度是o1；但是链表实现插入排序的时间复杂度是on（链表插入数据不用移动后续数据），取数据时间复杂度是o1。。于此同时使用链表+快速排序的话，查找的时候（包含排序）时间复杂度是onlogn，插入的时候时间复杂度是o1。所以n比较大时，“链表+快速排序”的综合性能，弱于“链表+插入排序”，所以首先排除用的这种“每次查询时快排”的方式
      - 现在再看堆排序，但是使用堆排序的话，可以实现ologn的插入，和o1的查找，综合性能比“链表+插入排序”更高。

有老哥就用java暴力法过了，方法和我类似，但是少了“自定义排序，手动装箱拆箱，类型强转“。

```java
class MedianFinder { 
List<Integer> list = new ArrayList<Integer>();
/** initialize your data structure here. */
public MedianFinder() {
}

public void addNum(int num) {
    list.add(num);
}

public double findMedian() {
    Collections.sort(list);
    int len = list.size();
    if (len == 0) {
        return 0.0;
    }
    if (len%2 == 1) {
        return list.get(len/2);
    } else {
        return (list.get(len/2-1) + list.get(len/2))/2.0;
    }
}
}
```

#### 官方-优先队列/堆

解题思路：

![image-20220408204404242](lcof.assets/image-20220408204404242.png)

- billibili堆排序[教学](https://www.bilibili.com/video/BV1Eb41147dK?from=search&seid=8087900261957567930&spm_id_from=333.337.0.0)

![Picture1.png](lcof.assets/25837f1b195e56de20587a4ed97d9571463aa611789e768914638902add351f4-Picture1.png)

算法流程：

![image-20220408204443165](lcof.assets/image-20220408204443165.png)

- 我：之所以要先插入别人那，是因为要让对方堆也能根据新到数据排序，更新对方的堆；并且拿回来的数据，可能在对方堆的数据中间，导致拿回来的数据和插入对方堆的数据是不一样的。

复杂度分析：

![image-20220408204754191](lcof.assets/image-20220408204754191.png)

代码：

Java 使用 `PriorityQueue<>((x, y) -> (y - x))` 可方便实现大顶堆。

```java
class MedianFinder {
    Queue<Integer> A, B;
    public MedianFinder() {
        A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
        if(A.size() != B.size()) {
            A.add(num);
            B.add(A.poll());
        } else {
            B.add(num);
            A.add(B.poll());
        }
    }
    public double findMedian() {
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}
```

- `/2.0`会自动把“integer自动拆箱得到的int”转化为double。
- [PriorityQueue详解](https://www.jianshu.com/p/f1fd9b82cb72):
  - 利用堆排序，保证树里总是最大/最小的元素在父节点；并且除顶部是最大值/最小值外，集合的其余部分是不保证有序的。
  - Comparator自定义排序的话，返回-1表示o1和o2的顺序是默认的升序，即o1<o2，所以不用交换两者顺序。

#### 即时再战半寄

```java
class MedianFinder {

    /** initialize your data structure here. */
    /**
    smaller是较小的一半集合，用大顶堆处理这个集合，让较小集合里的最大值始终在堆顶部，即在中位数附近；允许smaller比bigger多一个元素
    bigger是较大的一半集合，用小顶堆处理这个集合，让较大集合里的最小值始终在堆顶部，即在中位数附近
     */
    PriorityQueue<Integer> smaller=null;
    PriorityQueue<Integer> bigger=null;
    public MedianFinder() {
        smaller=new PriorityQueue<>(new Comparator<Integer>(){//大顶堆递减，不是默认的，要自定义比较规则
            @Override
            public int compare(Integer o1,Integer o2){
                if(o1<o2){//如果前面的更小，返回1表示交换两者顺序，即为了让前面的更大（即实现降序）
                    return 1;
                }else if(o1>o2){
                    return -1;
                }else{
                    return 0;
                }
            }
        });
        //smaller=new PriorityQueue<>((x, y) -> (y - x));
        bigger=new PriorityQueue<>();//小顶堆递增
    }
    
    public void addNum(int num) {
        //因为待插入值可能属于对面堆，所以每个值都要先填入对面堆，再从对面堆顶部拿值；
        if(smaller.size()==bigger.size()){//如果两个堆数目相同，往smaller中插入；但是先插入bigger保证smaller是总集合的较小一半数的集合
            bigger.offer(num);
            smaller.offer(bigger.poll());
        }else{//如果smaller数目多1，就往bigger堆插入；但是先插入smaller,才能保证bigger是总集合的较大一半数的集合
            smaller.offer(num);
            bigger.offer(smaller.poll());
        }
    }
    
    public double findMedian() {
        if(smaller.size()==bigger.size()){//如果两个堆的元素数目相同，那么各取堆顶计算中位数
            return (smaller.peek()+bigger.peek())/2.0;//2.0自动把整个式子转化为float；返回类型是double，我估计就自动转化了。注意查找中位数不能执行poll，poll会移除元素，要用peek
        }else{//如果smaller的元素多1，说明共奇数个元素，直接拿smaller堆的堆顶返回即可
            return smaller.peek();
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

- 关于[`size()`时间复杂度](https://blog.csdn.net/Kevin___________/article/details/108681594)

- 错误点：

  - 求中位数时，误用poll取数，导致更改了堆影响后续操作；应该用peek。
  - poll的是堆顶，小顶堆是递增，那么poll出的就是最小值；反之大顶堆poll出来的就是最大值。

- 问答问：为什么都用了堆排序，我的效率比官方低这么多？（红框是我，绿框是官方）

  <img src="lcof.assets/image-20220409010404047.png" alt="image-20220409010404047" style="zoom:50%;" />

  - 经过使用官方的lamda表达式决定大顶堆，但是线效率没变，说明是其他部位导致的。

  - 自答：发现代码遗留了debug时设置的System.out.println()，删掉之后效率和官方一致；看来输出很耗时啊！

    <img src="lcof.assets/image-20220409010830512.png" alt="image-20220409010830512" style="zoom:50%;" />

## 搜索与回溯算法(中等)

### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        //判断返回条件
        if(root==null)return 0;

        //递归拿到左右子树的深度。取大的深度，加一返回为本节点返回的最大深度
        return Math.max(maxDepth(root.left),maxDepth(root.right))+1;

    }
}
```

#### 官方1-后序遍历（DFS）

树的遍历方式总体分为两类：深度优先搜索（DFS）、广度优先搜索（BFS）；

- 常见的 DFS ： 先序遍历、中序遍历、后序遍历；
- 常见的 BFS ： 层序遍历（即按层遍历）。

求树的深度需要遍历树的所有节点，本文将介绍基于 后序遍历（DFS） 和 层序遍历（BFS） 的两种解法。

思路：

![image-20220409165709831](lcof.assets/image-20220409165709831.png)

算法解析：

![image-20220409165731652](lcof.assets/image-20220409165731652.png)

复杂度分析：

![image-20220409165757840](lcof.assets/image-20220409165757840.png)

代码：

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

- 是在得到左右子树结果后，操作本节点（即+1），所以是后序遍历

#### 官方2-层序遍历（BFS）

思路：

- 树的层序遍历 / 广度优先搜索往往利用 **队列** 实现。
- **关键点：** 每遍历一层，则计数器 +1+1 ，直到遍历完成，则可得到树的深度。

算法解析：

1. 特例处理： 当 root 为空，直接返回 深度 00 。
2. 初始化： 队列 queue （加入根节点 root ），计数器 res = 0。
3. 循环遍历： 当 queue 为空时跳出。
   1. 初始化一个空列表 tmp ，用于临时存储下一层节点；
   2. 遍历队列： 遍历 queue 中的各节点 node ，并将其左子节点和右子节点加入 tmp；
   3. 更新队列： 执行 queue = tmp ，将下一层节点赋值给 queue；
   4. 统计层数： 执行 res += 1 ，代表层数加 11；
4. 返回值： 返回 res 即可。

复杂度分析：

![image-20220409170223187](lcof.assets/image-20220409170223187.png)

代码：

```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    int res = 0;
    while (!queue.isEmpty()) {
        res++;
        int n = queue.size();
        for (int i = 0; i < n; i++) {
            TreeNode node = queue.poll();
            if (node.left != null) queue.add(node.left);
            if (node.right != null) queue.add(node.right);
        }
    }
    return res;
}
```

- 我和网友：这个很像从上往下分层打印二叉树。

### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null)return true;

        //查询本节点为根的树，的左右子树深度是否满足平衡
        if(depth(root.left)-depth(root.right)>1||depth(root.left)-depth(root.right)<-1){
            return false;
        }
        //递归查询左右子树中所有节点的平衡性
        return isBalanced(root.left)&&isBalanced(root.right);
    }
    public int depth(TreeNode root){//查看某节点为根节点的子树的深度。会对每个节点求左右子树深度，导致大量重复计算，最好是tree有个字段存一下深度，但是这又没有
        if(root==null)return 0;

        return Math.max(depth(root.right),depth(root.left))+1;
    }
}
```

- 对每个节点反复计算了深度，重复计算了；但是没想到更好的解法。

#### 官方1-后序遍历 + 剪枝 （从底至顶）

做题前提：

![image-20220409173300101](lcof.assets/image-20220409173300101.png)

思路：

> 后序遍历 + 剪枝 （从底至顶）为此方法为本题的最优解法，但剪枝的方法不易第一时间想到。

思路是对二叉树做后序遍历，从底至顶返回子树深度，若判定某子树不是平衡树则 “剪枝” ，直接向上返回。

算法流程：

![image-20220409173850610](lcof.assets/image-20220409173850610.png)

复杂度分析：

![image-20220409174009514](lcof.assets/image-20220409174009514.png)

代码：

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return recur(root) != -1;
    }

    private int recur(TreeNode root) {
        if (root == null) return 0;
        int left = recur(root.left);
        if(left == -1) return -1;
        int right = recur(root.right);
        if(right == -1) return -1;
        return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
    }
}
```

- 学习剪枝思想。用返回值为-1剪枝是没想到的；自己写的时候一直想在树节点处记录信息，在递归查询的时候查询节点的历史记录来剪枝。
- 我认为思想：求深度的函数，不单单是求深度了，在求每个节点的深度时，还会求已本节点为顶点的二叉树是否为完全二叉树。

#### 官方2-先序遍历 + 判断深度 （从顶至底）

> 此方法(就是我的算法)容易想到，但会产生大量重复计算，时间复杂度较高。

思路是构造一个获取当前子树的深度的函数 depth(root) （即 面试题55 - I. 二叉树的深度 ），通过比较某子树的左右子树的深度差 abs(depth(root.left) - depth(root.right)) <= 1 是否成立，来判断某子树是否是二叉平衡树。若所有子树都平衡，则此树平衡。

算法流程：

![image-20220409174524696](lcof.assets/image-20220409174524696.png)

复杂度分析：

![image-20220409174554930](lcof.assets/image-20220409174554930.png)

代码：

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        return Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
}
```

### [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

#### 首战寄

没思路；并且codetop显示这道题就出现过2次，说明不是考察点；这题目还有个“脑筋急转弯”的标签。。

#### 官方-逻辑符短路

解题思路：

![image-20220409192607863](lcof.assets/image-20220409192607863.png)

方法一： 平均计算

![image-20220409192632853](lcof.assets/image-20220409192632853.png)

方法二： 迭代

![image-20220409192654183](lcof.assets/image-20220409192654183.png)

方法三： 递归

![image-20220409192805645](lcof.assets/image-20220409192805645.png)

![Picture1.png](lcof.assets/2d25bb3aec987712b717f7954d93494beb0a3e352acee486b3ce58bce60ee07c-Picture1.png)

逻辑运算符的短路效应：

![image-20220409193031359](lcof.assets/image-20220409193031359.png)

复杂度分析：

![image-20220409193047238](lcof.assets/image-20220409193047238.png)

代码：

![image-20220409193156424](lcof.assets/image-20220409193156424.png)

```java
class Solution {
    int res = 0;
    public int sumNums(int n) {
        boolean x = n > 1 && sumNums(n - 1) > 0;
        res += n;
        return res;
    }
}
```

#### 即时再战成功

```java
class Solution {
    int res=0;
    public int sumNums(int n) {
        //n==1的时候终止递归，开始退栈
        boolean x=n>1&&sumNums(n-1)>0;
        res+=n;
        return res;
    }
}
```

### [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

#### 首战告捷

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    //全局变量保存结果
    TreeNode result=null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        /**
        后序遍历，先拿到左右子树是否有给定节点；再看本节点是否有给定节点；然后剪枝返回
         */
        //处理特殊情况
        if(root==null)return null;
        
        //深度递归，查找公共祖先
        recur(root,p,q);

        //recur过后目标节点保存在result中，直接返回
        return result;
    }
    public boolean recur(TreeNode root, TreeNode p, TreeNode q){
        //处理返回条件。只要本节点包含目标节点，就给上层递归返回true；注意root==p或q的时候不能直接返回true，因为要对他做公共祖先的判断
        if(root==null)return false;
        // else if(root==p)return true;
        // else if(root==q)return true;

        if(result!=null){//剪枝。防止得到结果后还不停递归。这种一个if没else的剪枝判断感觉可读性较好
            return false;
        }

        //后序遍历
        boolean left=recur(root.left,p,q);
        boolean right=recur(root.right,p,q);
        
        //如果左右子树+本节点中包含了两个目标节点，说明当前节点就是公共祖先，保存当前节点
        if((left&&right)||(left&&root==p)||(left&&root==q)||(right&&root==p)||(right&&root==q)){
            //System.out.println("找到root为："+root.val);
            result=root;
            return false;
        }

        //如果左右子树+本节点中包含了一个目标节点，返回true。如果包含了两个，在上面一个判断就会返回了
        if(left||right||root==p||root==q)return true;

        //如果左右子树都不包含目标节点，当前节点也不是目标节点，返回false。如果包含两个或1个，在上面的两个判断之一就会返回了
        return false;
    }
}
```

- 我这由底向顶的，dfs后续遍历递归处理，没有做重复判断，效率应该还行。但是我感觉三个if的判断语句那可以优化，比如用if(root==q||root==p)套if，这样可以减少不必要的判断；但是可读性会稍差，毕竟就不是并行的三个判断了

- 倒数的三个判断的这种顺序可以参考：
  1. 先放出现多种情况的
  2. 再放只出现一种情况的（此时就不在需要判断多种情况）
  3. 再放一种情况都不出现的（此时甚至都不需要做判断）
- 反思：
  - 没认真看题目，，没用上“二叉搜索树”的条件，当普通树处理了。

#### 官方1-迭代

解题思路：

![image-20220410175623459](lcof.assets/image-20220410175623459.png)

![image-20220410175707490](lcof.assets/image-20220410175707490.png)

迭代的做法：

![image-20220410175729876](lcof.assets/image-20220410175729876.png)

复杂度分析：

![image-20220410175815304](lcof.assets/image-20220410175815304.png)

代码：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while(root != null) {
            if(root.val < p.val && root.val < q.val) // p,q 都在 root 的右子树中
                root = root.right; // 遍历至右子节点
            else if(root.val > p.val && root.val > q.val) // p,q 都在 root 的左子树中
                root = root.left; // 遍历至左子节点
            else break;
        }
        return root;
    }
}
```

- 我：break的情况，正好是当前节点为pq的最近公共祖先的情况

代码优化，若可保证 p.val < q.val ，则在循环中可减少判断条件：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(p.val > q.val) { // 保证 p.val < q.val
            TreeNode tmp = p;
            p = q;
            q = tmp;
        }
        while(root != null) {
            if(root.val < p.val) // p,q 都在 root 的右子树中
                root = root.right; // 遍历至右子节点
            else if(root.val > q.val) // p,q 都在 root 的左子树中
                root = root.left; // 遍历至左子节点
            else break;
        }
        return root;
    }
}
```

#### 官方2-递归

递归思路：

![image-20220410180944663](lcof.assets/image-20220410180944663.png)

复杂度分析：

![image-20220410181003714](lcof.assets/image-20220410181003714.png)

代码：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val < p.val && root.val < q.val)
            return lowestCommonAncestor(root.right, p, q);
        if(root.val > p.val && root.val > q.val)
            return lowestCommonAncestor(root.left, p, q);
        return root;
    }
}
```

- 核心：抓住二叉搜索树的特性！

## 分治算法(中等)

### [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

#### 首战寄

能用笔实现出来，但是用算法实现没有思路。

#### 官方-分治

[文字看不懂的话，可以看这个讲解视频](https://www.bilibili.com/video/BV1LJ411y7Xa?from=search&seid=7337447768713066113&spm_id_from=333.337.0.0)，看完视频后再看下面的解析和代码。

解题思路：

![image-20220411200426772](lcof.assets/image-20220411200426772.png)

![image-20220411200437879](lcof.assets/image-20220411200437879.png)

分治算法解析：

![image-20220411200501485](lcof.assets/image-20220411200501485.png)

复杂度分析：

![image-20220411200517480](lcof.assets/image-20220411200517480.png)

代码：

> 注意：本文方法只适用于 “无重复节点值” 的二叉树。

```java
 //利用原理,先序遍历的第一个节点就是根。在中序遍历中通过根 区分哪些是左子树的，哪些是右子树的
    //左右子树，递归
    HashMap<Integer, Integer> map = new HashMap<>();//标记中序遍历
    int[] preorder;//保留的先序遍历

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        for (int i = 0; i < preorder.length; i++) {
            map.put(inorder[i], i);
        }
        return recursive(0,0,inorder.length-1);
    }

    /**
     * @param pre_root_idx  先序遍历的索引
     * @param in_left_idx  中序遍历的索引
     * @param in_right_idx 中序遍历的索引
     */
    public TreeNode recursive(int pre_root_idx, int in_left_idx, int in_right_idx) {
        //相等就是自己
        if (in_left_idx > in_right_idx) {
            return null;
        }
        //root_idx是在先序里面的
        TreeNode root = new TreeNode(preorder[pre_root_idx]);
        // 有了先序的,再根据先序的，在中序中获 当前根的索引
        int idx = map.get(preorder[pre_root_idx]);

        //左子树的根节点就是 左子树的(前序遍历）第一个，就是+1,左边边界就是left，右边边界是中间区分的idx-1
        root.left = recursive(pre_root_idx + 1, in_left_idx, idx - 1);

        //由根节点在中序遍历的idx 区分成2段,idx 就是根

        //右子树的根，就是右子树（前序遍历）的第一个,就是当前根节点 加上左子树的数量
        // pre_root_idx 当前的根  左子树的长度 = 左子树的左边-右边 (idx-1 - in_left_idx +1) 。最后+1就是右子树的根了
        root.right = recursive(pre_root_idx + (idx-1 - in_left_idx +1)  + 1, idx + 1, in_right_idx);
        return root;
    }
```

- 我理解：用hash关联中序的值和index，是为了根据先序的root值，求得root在中序中的index；进而根据中序中root的index，得到root的左右子树元素的个数。因为无论是中序还是先序数组，左/右子树的所有节点都是紧贴在一起的；而中序遍历的特点就是root能分割左右子树，特别适合用来判断数组中左/右子树的元素的个数。

#### 即时再战成功

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    int[] pre;
    Map<Integer,Integer> in=new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        /**
        利用特性：
        1.先序遍历的第一个节点为当前树的root；
        2.中序中root分割左右子树，可以求得左or右子树分别有多少个元素
        
        本解法的前提是树中元素都不唯一
         */

        //处理特殊情况
        if(preorder.length==0)return null;

        //为了在递归中使用preorder数组，并且不必往方法传入preorder数组，这里用全局变量保存preorder数组
        pre=preorder;
        //把inorder中元素和index用hash关联起来；便于后续根据root值求得左右子树的元素数目
        for(int i=0;i<inorder.length;i++){
            in.put(inorder[i],i);
        }

        //递归， 根据pre中root的index 和 in中树的左右边界，构建树并返回根节点
        return recur(0,0,inorder.length-1);
    }
    public TreeNode recur(int pre_root_idx, int in_left_idx, int in_right_idx){
        //设置递归返回条件，in_left_idx和in_right_idx交叉时说明子树是空节点了，直接返回null
        if(in_left_idx>in_right_idx)return null;

        //先利用pre_root_index拿到root的值
        int rootValue=pre[pre_root_idx];
        //再根据root的值得到中序遍历数组中，root的位置
        int in_root_index=in.get(rootValue);

        //创建当前节点
        TreeNode cur=new TreeNode(rootValue);

        /*
        递归创建当前节点的左右子节点：
        1.根据先序遍历数组的特点可知，左子节点的pre_root_idx即为当前pre_root_idx+1，右子节点的pre_root_idx即为当前pre_root_idx+左子树节点个数+1(加1是为了越过左子树而到达右子树)
        2.根据中序遍历数组的特点可知，中序遍历中，左子树的左边界为in_left_idx，左子树的右边界为in_root_index-1;
                                                  右子树的左边界为in_root_index+1，右子树的右边界为in_right_idx
         */
        cur.left=recur(pre_root_idx+1,in_left_idx,in_root_index-1);
        cur.right=recur(pre_root_idx+(in_root_index-in_left_idx)+1,in_root_index+1,in_right_idx);

        return cur;
    }
}
```

- 我：我感觉我这个代码的命名和编写，搭配注释，比官方还要容易理解。

- 注意：可以如本例`pre=preorder;`，直接对数组增加引用。

### [剑指 Offer 16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

#### 首战半寄

```java
class Solution {
    public double myPow(double x, int n) {
        /**
        对n分类讨论，同时注意分母为0的情况
         */
        //针对x处理特殊情况，减少计算
        if(x==1)return 1;
        if(x==0)return 0;
        
        if(n>0){
            double stordX=x;//注意x是double类型，保存的x也得是声明为double
            for(int i=0;i<n-1;i++){//指数是n的话，x自乘n-1次。注意是n-1次！！！！
                //但是不能使用x*=x;这回导致：第一次计算是n*n，第二次计算实际上是(n的平方)*(n的平方)了
                x*=stordX;
            }
            return x;
        }else if(n<0){
            if(x==0){//0不能做分母
                return 0;
            }
            double stordX=x;
            for(int i=n;i<0;i++){//指数是-n的话，用1除以x进行n次
                //x=1/x;不能直接1/x，因为第一次除后为1/x，再用1/(1/x)那么又得到了x。。
                if(i==n){//第一次是1/x，其他的都是之前的结果除以x
                    x=1/x;
                }else{
                    x=x/stordX;
                } 
            }
            return x;
        }else{//指数n==0，直接返回1即可
            return 1;
        }

    }
}
```

- 用常规思路做的，通过了301/303个测试案例，但是2.00000的 -2147483648次方计算超时了。
- 反思：
  - n>0时，没用心，最开始做成了指数为n就自乘n次，实际上是自乘n-1次。
  - n<0时，没用心，最开始做成了指数为n就不停让x=1/x，但是这样相当于总是把1放在分子，而实际上应该把除后的结果放在分子。
  - 之前计算时，没把x保留下来，导致每次都会把计算后的x当成原x，导致错误。

#### 大佬-快速幂

官方的解释的不清楚，就用大佬的。

解题思路：

![image-20220411180036705](lcof.assets/image-20220411180036705.png)

[知乎快速幂](https://zhuanlan.zhihu.com/p/95902286)：

- 我理解：就不让cpu每次乘一个小数，而是先乘得一个大数，然后两个大数相乘，这样就减少了乘法的次数。

位运算使用：

![image-20220411184903746](lcof.assets/image-20220411184903746.png)

我理解：

![image-20220411183243537](lcof.assets/image-20220411183243537.png)

代码：

```java
class Solution {
    public double myPow(double x, int n) {
        if(x == 0) return 0;
        long b = n;
        double res = 1.0;
        //如果幂是负数，就把它转化为正数来处理
        if(b < 0) {
            x = 1 / x;
            b = -b;
        }
        while(b > 0){
            // 最后一位为1，需要乘上该位上的权重
            if((b & 1) == 1){
                res *= x;
            }
            x *= x;
            b >>= 1;
        }
        return res;
    }
}
```

- 网友问：为什么要把其中的n换成long类型？
  - 网友答：因为32位int是补码形式，正数是和原码相同，范围是0到2的31次方-1，但是对于负数，需要反码+1，范围是负2的31次方到0，负数要比正数多一个数字。如果传进来的int刚好是负2的32次方，取相反数之后就超过int32类型的取值范围了，所以需要用long来扩大取值范围。不好意思啊，评论区数学公式不太好打。
  - 注意：有符号的话有个符号站位，就级数是2的31次方；如果没有正负号，就所有位都用来保存数字，级数就为2的32次方。

#### 即时再战成功

```java
class Solution {
    public double myPow(double x, int n) {
        /**
        使用快速幂，尽量让大数相乘，不要让小数相乘占用CPU
         */
        //特殊情况处理
        if(x==0)return 0;
        
        //用于返回结果，需要的自乘的x的结果会被乘入res
        double res=1;
        
        //n为负数的时候，把它转换为正数的问题来处理
        long b=n;//因为负数的绝对值更大，为了防止负数变成正数问题时超过证书边界，得用long来处理n
        if(b<0){
            b=-b;
            x=1/x;//因为x为0的情况排除了，所以可以安心把x放在分母
        }
        
        //开始快速幂运算，让x不断自己平方，当幂的二进制的对应位为1时，则说明需要把自乘x的当前结果往结果中乘
        //处理幂转化为二进制后的每个二进制位
        while(b>0){
            if((b&1)==1){//如果当前处理而二进制幂的位置为1，说明x自乘的当前结果要乘入结果
                res*=x;
            }
            x*=x;//x不断自己平方，是快速幂节省cpu运算的关键
            b=b>>1;//把幂的二进制右移一位（即从低位砍掉一位），下一次循环就会处理的是更高一位
        }

        return res;
    }
}
```

- 反思：
  - 位运算>>后会得到一个结果，所以必须要用等号来赋值。` b=b>>1;`等价于` b>>=1;`
  - 与运算符&的优先级没有==高，所以要用括号包裹`b&1`
    - 运算符优先级容易记混，总是用括号包裹好了；可读性还蛮好。



### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

