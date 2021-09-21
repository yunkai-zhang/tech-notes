#### list Vs set
- [list,set,map之间的区别](https://www.cnblogs.com/IvesHe/p/6108933.html)
- list有序的接口。适合根据索引访问。LinkedList适合时常增删节点的用法，ArrayList适合时常需要查找的用法。list允许重复
- set无序的接口。无法根据索引访问。但是HashSet的查找的时间复杂度是o（1）。set中不允许重复
- map是一个接口。map不能直接实例化，但是可以指向一个实现了该接口的实例类：`Map <String,String> map = new HashMap <>();`

#### for循环中，i++和++i的区别
- 他们两个在for循环中的效果一样[for循环中i++和++i都可以转化为：在循环体末尾加上语句i++和++i。所以等到判断的时候，两者效果相同。++i运行更快](https://blog.csdn.net/github_37847975/article/details/77369851?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3)

#### 异常处理
- [try catch finally详解](https://blog.csdn.net/sugar_no1/article/details/88593255)
- 只有finally块，执行完成之后，才会回来执行try或者catch块中的return或者throw语句，如果finally中使用了return或者throw等终止方法的语句，则就不会跳回执行，直接停止。
- [try catch finally的执行顺序](https://blog.csdn.net/yuanmoxun0217/article/details/80939114?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-9&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-9)

4. 线程
5. 计算机网络
6. 时间空间复杂度，尤其是logn型
7. 重载和覆盖
8. java中的final，static等具体知识和例子
9. 时间复杂度计算，尤其是logn型
10. tcp和udp在传输层吗
11. java io类用了哪种设计模式
12. 对面向对象的理解

#### 输入输出
- [scanner输入输出详解。int和short讲的不错](https://blog.csdn.net/wobushixiaobailian/article/details/80279880?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-32&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-32)
- [sacnner用于接收字符串的next和nextLine的区别](https://blog.csdn.net/csdn_9527666/article/details/77917047)
- 读取nextInt()后，再想用nextline读取字符串的话，必须先用nextline去清除一下队列中残存的用于输入int的换行符！
- 建立输入流：Scanner sc=new Scanner(System.in);
- 读取整数：if(sc.hasNext())int i=sc.nextInt();
- 读取字符串：if(sc.hasNext()) String i=sc.next();//这个next是空格和换行符输入。nextLine是只承认换行符输入。

#### java中实参和形参的关系
- 如果像非基本类型被传入函数传的是实参，那么函数内的操作时如何影响函数外的实参的？[一个讲得还不错](https://www.cnblogs.com/yaoruozi/p/8569906.html)
#### 抽象类和接口
- 接口不能实例化，但是接口可以作为变量（接口的引用），指向一个实现类（实现该接口的类）的实例
- [文章讲述了1）接口如何指向实例化对象，2）抽象类向上向下转型时函数的使用（如a.work()报错，因为声明的animal类型没有这个函数），3）和继承的关系](https://blog.csdn.net/Qiuzhongweiwei/article/details/80749258?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)

#### String
- string.length()计算的字符串长度包含字符串中的空格。

#### 学习javascript的基本语法（会写小程序就可）

#### 怎么查看一个数字在电脑中二进制储存的模样，并且要删掉二进制前面多余的0


