## 前言

笔记名词解释：

- 大捷：指独立做出来，可能粗心导致有小debug也de出来了
- 半寄：有思路且代码基本写对了，但是在不该错的地方debug，最后de出来了或者放弃本思路
- 寄：有思路但是没完全写出来，或者完全没思路
- codetop==XXX：这个只有主站题才会有，表示这个题在codetop上显示的出题频次

剑指和主站（即codetop）题目的摆放问题：

- 如果主站题目和剑指重合，那么会放到一个单元中
- 如果主站题目和剑指不重合，那么就放到对应标签的最大难度单元里。
- 剑指的题目的名字有“剑指”，主站的题目名字用题号开头。

笔记编写纲领

- 公式和图片演示，可以截图；其他只有一两个不方便复制的内容，比如时间复杂度的讲解，应当以文字方式记录。

## 基础知识

### 时间复杂度

1，计算参考：时间复杂度是总运算次数表达式中受n的变化影响最大的那一项(不含系数)。[详情参考](https://blog.csdn.net/firefly_2002/article/details/8008987)。

- 坛友和我：二分**查找**时间复杂度logn，快速**排序**期望时间复杂度nlogn；两者很像，可以理解为针对每个元素都二分查找它在数组中的位置，这样得到全体元素的位置的时间复杂度就是快排的复杂度。
  - 坛友：和二分有关的就带logn。二分查找和快排都有二分的思想。

### 输入输出+ACM

#### 前言

1，大厂一般用牛客做笔试，使用acm模式，即是空白版让写。这时候就要自己建名为Main的类，在里面写main方法，然后操作。 

2，Java的常用输入输出语句https://www.cainiaojc.com/java/java-basic-input-output.html

3，scanner简单一点：https://www.runoob.com/java/java-scanner-class.html；（但是推荐用bufferreader）。

```java
//以输入int为例
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        while (in.hasNextInt()) { // 注意 while 处理多个 case
            int a = in.nextInt();
            int b = in.nextInt();
            System.out.println(a + b);
        }
        in.close();
    }
}
```

- 对字符和字符串输入用hasNext（碰到空格就结束）和hasNextLine（碰到换行符结束）判断是否还有输入；并通过next和nextLine获取输入字符或字符串。
  - 可以用next接收字符串，然后用charAt拿到字符串中的指定字符。
- 对整数或双精度浮点数，用hasNextInt,hasNextDouble来判断是否还有输入；并通过nextFloat和nextDouble获取输入整数或双精度浮点数。

#### 牛客ACM输入输出练习

牛客输入输出联系[网址](https://ac.nowcoder.com/acm/contest/5657#question)。

- 推荐bufferreader，速度快，并且不会有吞回车的情况
- 我：不需要都练习，直接练习“真实ACM练习”一道题即可

#### 真实ACM练习

1，ACM模式下的两种输入的[接收方式](https://blog.csdn.net/qq_39144436/article/details/124152391)，着重看BufferReader的方式：

- 引入`import java.io.*`
- 类名限定`public class Main{}`
  - 我：注意类名前的class小写

- 主方法限定`public static void main(String[] args) throws IOException {}`
  - 注意main函数的入参String数组
  - 注意`throws IOException`
- BufferedReader的创建方式`new BufferedReader(new InputStreamReader(System.in));`
  - 尤其是Buffer**ed**Reader的构造函数中输入了InputStream**Reader**，并且要给InputStreamReader传入System.in
- BufferReader的接收中，常用`Integer.parseInt(br.readLine())`得到一行单个整数；` String[] ops=br.readLine().trim().split(" ")`处理一行多个参数的情况；
  - 注意：split中要有**" "**


2，大厂一般都用牛客的ACM模式出题，可以[练习这一题目](https://ac.nowcoder.com/acm/problem/13883),题目来自[牛客ACM竞赛](https://ac.nowcoder.com/acm/problem/list):

我用scanner会超时：

```java
import java.util.*;

public class Main{
    public static void main(String[]args){
        //接收最基础的前两行参数
        Scanner sc=new Scanner(System.in);
        int total=Integer.valueOf(sc.nextLine());
        
        while(total>0){
        //java split的用法：https://m.runoob.com/java/java-string-split.html
        String[] s1s=sc.nextLine().split(" ");
        int matrixSize=Integer.valueOf(s1s[0]);
        int opNums=Integer.valueOf(s1s[1]);
        
        //构建数组
        int[][] matrix=new int[matrixSize][matrixSize];
            
            while(opNums>0){
                //针对每次操作做数组处理，获取参数
                String[] ops=sc.nextLine().split(" ");
                boolean doRow=ops[0].equals("1")?true:false;
                //大坑，这里的location不是0起步的，是1起步的第几行/列的意思
                int location=Integer.valueOf(ops[1]);
                int v=Integer.valueOf(ops[2]);

                //执行程序逻辑
                if(doRow){
                    for(int i=0;i<matrix[0].length;i++){
                        matrix[location-1][i]=v;
                    }
                }else{
                    for(int i=0;i<matrix.length;i++){
                        matrix[i][location-1]=v;
                    }
                }

                //while的循环条件处理别忘了     
                opNums--;
            }
            //操作完毕后再打印
            //打印的换行问题：https://www.cnblogs.com/WLCYSYS/p/13202378.html
            for(int i=0;i<matrixSize;i++){
                //注意每行的最后不能有空格,并且换行
                int j=0;
                for(j=0;j<matrixSize-1;j++){
                    System.out.print(matrix[i][j]+" ");
                }
                System.out.println(matrix[i][j]);
            }
        
          total--;  
        }
        
        sc.close();
        
        
    }
}
```

参考不超时的代码用了BufferReader，[参考](https://blog.csdn.net/weixin_43314519/article/details/108590644s)：

```java
import java.io.*;
public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int cases = Integer.parseInt(br.readLine());
        while (cases-- > 0) {
            String[] strs = br.readLine().trim().split(" ");
            int size = Integer.parseInt(strs[0]);
            int times = Integer.parseInt(strs[1]);
            int[][] matrix = new int[size][size];
            while (times-- > 0) {
                String[] strs1 = br.readLine().trim().split(" ");
                int op = Integer.parseInt(strs1[0]);
                int rowOrColum = Integer.parseInt(strs1[1]);
                int toValue = Integer.parseInt(strs1[2]);
                if (op == 1) {
                    for (int k = 0; k < size; k++) {
                        matrix[rowOrColum - 1][k] = toValue;
                    }
                } else if (op == 2) {
                    for (int k = 0; k < size; k++) {
                        matrix[k][rowOrColum - 1] = toValue;
                    }
                }
            }
            for (int m = 0; m < size; m++) {
                for (int n = 0; n < size; n++) {
                    System.out.print(matrix[m][n] + " ");
                }
                System.out.println();
            }
        }
    }
}
```

- [Integer.parseInt(s)与Integer.valueOf(s)的区别](https://blog.csdn.net/u010502101/article/details/79162587)，一个得到int，一个得到Integer。我超时的案例用的是valueOf，相当于拿到数值后解包装；通过的案例用的是parseInt。
- 注意要引入`import java.io.*;`包

自己重新使用BufferedReader改造程序，就运行成功了！！，看来确实是Scanner性能不如BufferedReader：

```java
import java.io.*;

public class Main{
    public static void main(String[]args) throws IOException{
        //接收最基础的前两行参数
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        int total=Integer.parseInt(br.readLine());
        
        while(total>0){
        //java split的用法：https://m.runoob.com/java/java-string-split.html
        String[] s1s=br.readLine().trim().split(" ");
        int matrixSize=Integer.parseInt(s1s[0]);
        int opNums=Integer.parseInt(s1s[1]);
        
        //构建数组
        int[][] matrix=new int[matrixSize][matrixSize];
            
            while(opNums>0){
                //针对每次操作做数组处理，获取参数
                String[] ops=br.readLine().trim().split(" ");
                boolean doRow=ops[0].equals("1")?true:false;
                //大坑，这里的location不是0起步的，是1起步的第几行/列的意思
                int location=Integer.parseInt(ops[1]);
                int v=Integer.parseInt(ops[2]);

                //执行程序逻辑
                if(doRow){
                    for(int i=0;i<matrix[0].length;i++){
                        matrix[location-1][i]=v;
                    }
                }else{
                    for(int i=0;i<matrix.length;i++){
                        matrix[i][location-1]=v;
                    }
                }

                //while的循环条件处理别忘了     
                opNums--;
            }
            //操作完毕后再打印
            //打印的换行问题：https://www.cnblogs.com/WLCYSYS/p/13202378.html
            for(int i=0;i<matrixSize;i++){
                //注意每行的最后不能有空格,并且换行
                int j=0;
                for(j=0;j<matrixSize-1;j++){
                    System.out.print(matrix[i][j]+" ");
                }
                System.out.println(matrix[i][j]);
            }
        
          total--;  
        }
        
        
    }
}
```

20220428自己再次练习：

```java
import java.io.*;

public class Main{
    public static void main(String[]args) throws IOException{
        //创建输入
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        //先接上第一个参数，表示要做几次矩阵打印
        int t=Integer.parseInt(br.readLine().trim());
        while(t-->0){//t==1的话就只会执行1次
            //接上第二行参数
            String[] secPara=br.readLine().trim().split(" ");
            int n=Integer.parseInt(secPara[0]);
            int q=Integer.parseInt(secPara[1]);
            
            int[][] matrix=new int [n][n];
            //根据操作数去接操作指令
            while(q-->0){
                String[] ops=br.readLine().trim().split(" ");
                int op=Integer.parseInt(ops[0]);
                int k=Integer.parseInt(ops[1]);
                int v=Integer.parseInt(ops[2]);
                
                if(op==1){
                    for(int i=0;i<n;i++){
                        matrix[k-1][i]=v;
                    }
                }else{
                    for(int i=0;i<n;i++){
                        matrix[i][k-1]=v;
                    }
                }
            }
            
            //打印案例，把做过一些列变换后的矩阵打印
            for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                    System.out.print(matrix[i][j]+" ");
                }
                //打印完一行要换行
                System.out.println("");
            }
        }
    }
}
```

- 注意：类名后无括号；main的小括号中有String[]args；main函数要throws IOException；

练习[一道简单的](https://ac.nowcoder.com/acm/contest/5657/A?&headNav=acm)：

```java
import java.io.*;

public class Main{
    public static void main(String [] args) throws IOException{
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        
        String line=br.readLine();
        //只要还有输入就继续处理。
        while(line!=null&&!line.isEmpty()){//isempty不能判断null。我：其实isempty没必要，因为要不读到有数据的行，要不就是读完了返回null。
            String[] lineSplited=line.trim().split(" ");
            int a=Integer.parseInt(lineSplited[0]);
            int b=Integer.parseInt(lineSplited[1]);
            System.out.println(a+b);
            
            //再尝试读一行
            line=br.readLine();
        }
    }
} 
```

- 我：这里用while循环自己实现了Scanner中hasNext的功能。这也可以看到，所有行的输入都读取完后，readLine拿到的是null，[参考](https://www.cnblogs.com/feizai-java/p/16193296.html)

我：最核心的BufferedReader的用法

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        String[] splited=br.readLine().trim().split(" ");
        Integer.parseInt(splited[0]);
    }
}
```



### 类型转换

1，包装类 基本数据类型 String 的转换：

- [1](https://blog.csdn.net/HY845638534/article/details/84669490)

2，set遍历及效率：[参考](https://www.cnblogs.com/east7/p/16115176.html)

- 可以看到使用增强型for就不错，但是这种方式不能修改set的值[参考](https://wenku.baidu.com/view/f3786de0b84cf7ec4afe04a1b0717fd5360cb221.html?_wkts_=1676015652834)

### 查找

#### 二分查找

1，又名折半查找，binary search。[参考](https://www.zhihu.com/question/36132386)。[参考](https://zhuanlan.zhihu.com/p/141480088)。代码模板：

```java
public int lowerBound(int[] nums,int left,int right,int target) {//求非降序范围[left,right)内第一个不小于target的值的位置。注意left和right夹住的范围是左闭右开的。
        while(left<right){//搜索区间[left,right)不为空
            int mid=left+(right-left)/2;//防止溢出整数范围
            if(nums[mid]<target){
                left=mid+1;//这个+1体现了”左闭“
            }else{
                right=mid;//这里没有+1体现了“右开”
            }
        }
        
        return left;//right也行，因为[left,right)为空的时候他们重合
}
```

- 左闭右开，while<，nums[mid]<target，return left;
- 我：else包含了`>=target`的情况，所以可以拿到第一个不小于target的值的位置。
- 我：`nums[mid]<target`可以拿到第一个不小于target的值的位置；`nums[mid]<=target`可以拿到第一个大于target的值的位置；已用“剑指 Offer 53”验证这个结论。

2，leetcode练习，我使用代码模板并略微结合题目“非重复”优化，首战告捷

```java
class Solution {
    public int search(int[] nums, int target) {
        int left=0,right=nums.length;//左闭右开
        while(left<right){
            int mid=left+(right-left)/2;//防止溢出整数范围
            if(nums[mid]<target){
                left=mid+1;
            }else if(nums[mid]>target){//把经典的左闭右开区间的移动右指针拆成两步，方便找到值后及时返回，避免无效计算
                right=mid;
            }else{
                return mid;//及时返回，避免无效计算
            }
        }
        //退出循环时，两个指针重合。由于左闭右开，表示区间内没有数字了，所以遍历了整个数组都没找到，直接返回-1
        return -1;
    }
}
```

### 常见手撕

[网址](https://blog.nowcoder.net/n/00d691a3f9c04d6c9f5177624529d6cd)

### 排序(手撕)

手撕排序可以到力扣中的[这道题](https://leetcode-cn.com/problems/sort-an-array/)自行验证是否手撕正确。

#### 排序基础知识

1，排序的稳定性：

- 参考：https://zhuanlan.zhihu.com/p/116046849
  - 不稳定：堆排序，快速排序
  - 稳定：归并排序，冒泡排序


2，排序的复杂度：

- [参考](http://t.csdn.cn/61jWc)，留意快排 归并 堆 冒泡（冒泡因为太基础基本不会考）即可
  - 快排：时间（最好nlogn 平均nlogn 最坏n2），空间（平均logn 最坏n）
  - 归并：时间（最好nlogn 平均nlogn 最坏nlogn），空间（平均n 最坏n）
  - 堆排序：时间（最好nlogn 平均nlogn 最坏nlogn），空间（平均1 最坏1）
  - 冒泡排序：时间（最好n 平均n2 最坏n2），空间（平均1 最坏1）


3，排序对比，[参考](https://www.cnblogs.com/wwp666/p/15977170.html)：

- 快速排序：
  - 优点：时间复杂度的常数项是最低的（所以平均来说是**最快**）
  - 缺点：最差时间复杂度可能n2（但尤其是大数据量下很难出现这种情况）
- 归并排序：
  - 优点：唯一满足**稳定性**的top3排序；并且时间复杂度总是nlogn能保证不会慢。
  - 缺点：空间复杂度n是top3排序里最高的（所以排序大量数据的时候不推荐使用）
- 堆排序：
  - 优点：**空间复杂度**是o1为top3排序里最小的；时间复杂度总是nlogn能保证不慢
  - 缺点：[实验效果](https://www.cnblogs.com/FengZeng666/p/14446495.html)看是top3排序里最慢的（虽然三者时间复杂度都是nlogn）

#### 快速排序

1，思路参考https://www.runoob.com/w3cnote/quick-sort.html。

- 时间空间复杂度分析：https://blog.csdn.net/zrh_CSDN/article/details/81178125。

1，快速排序优化[参考](http://t.csdn.cn/F7KfG)：

- 三数取中法优化
  上述算法采用将区间的最后一个元素作为关键字进行进一步的排序，但为了防止出现每次选择到最值元素这种最坏情况，引入三数取中法对算法进行优化。
  三数取中法是取区间的第一个、最后一个和中间元素相比较，选择出大小居中的元素作为关键字。
  引入了三数取中法之后，可以将最坏情况即原始序列有序的情况转换为最好情况，即每一次取到序列的中值元素，即每一次划分完全平均的二分迭代的最好情况。

2，实战手撕成功：

```java
class Solution {
    public int[] sortArray(int[] nums) {
        quick_sort(nums,0,nums.length-1);
        return nums;
    }
    
    public void quick_sort(int[] nums,int left,int right){
        //[left,right]区间内，有至少两个元素时，才会进行递归快排
        if(left<right){
            int i=getMid(nums,left,right);
            quick_sort(nums,left,i-1);
            quick_sort(nums,i+1,right);
        }
    }
    
    //把传入数组的分割成左小右大的情况，中间点
    public int getMid(int[] nums,int left,int right){
        //选定好用作pivot的元素，可以是[left,right]内任意位置的元素。这个pivot相当于是一个坑，坑要不停拆东墙补西墙去填平。
        int target=nums[left];
        while(left<right){
            //先移动右指针，找到第一个比target小的元素。！！！注意while的同时要确保left<right！！！注意确定left是标杆的话就先移动右指针
            while(left<right&&target<=nums[right])right--;
            //拿right位置的元素填平left位置的坑。那么right就是新的坑的位置
            nums[left]=nums[right];//不需要判断left<right再赋值，因为及时上面的循环是因为left==right退出的，这里赋值相当于自我赋值，不影响结果

            //再移动左指针，找到第一个比pivot大的元素，填入right的坑中
            while(left<right&&target>=nums[left])left++;
            //拿left位置的元素填平right位置的坑。那么left位置就是新的坑的位置
            nums[right]=nums[left];

        }
        //退出while循环时，left==right，此时把之前找的用作pivot的元素的值填入left和right重合的此位置；这样pivot左边元素比它小右边元素比它大
        nums[left]=target;

        return left;
    }

}
```

- 我：注意，确定left为哨兵的话，移动的时候就得先移动右指针；根据我这个模板的填坑思想很容易理解为什么要移动对面的指针。
- 我：注意和target比较时，用的是>=和<=，这样遍历过程中和target相等的值都不会被交换
- 我：注意，这种填坑算法，不需要使用swap交换值，只需要往坑里直接赋值。

我：还有一种两个while先执行的快排写法(来自本文的“剑指 Offer 40. 最小的k个数”)，但是这个可读性差，不推荐(可以看到下面语言不会高亮)；而且最致命的是这种写法过不了leetcode排序的时间限制：

```
class Solution {
    public int[] sortArray(int[] nums) {
        quick_sort(nums,0,nums.length-1);
        return nums;
    }

	private void quick_sort(int[] arr, int l, int r) {
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

- 我：这种版本碰到大量相同数字连在一起的时候会超时；我猜测是因为这种版本的`quick_sort`函数内容比较大，而只有加载函数入栈后走到`if (l >= r) return;`才能出栈；而能通过测试案例的我推荐的快排，它的`quick_sort`就很小，虽然也是入栈后碰到`if(left<right)`就出栈，但是因为加载的`quick_sort`很小所以节约了一点时间。
  - 我：以上是我的猜测；这样才勉强解释两种写法几乎一样的快排，一种能通过测试，另一种不行。

#### 归并排序

1，思路和方法：https://www.runoob.com/w3cnote/merge-sort.html。

- 我：快排和归并都用到了分治的思想；不过快排是自顶向下排序，让每层递归排序后，左边都小于pivot，右边都大于pivot；归并是自下向顶排序，每层递归排序时，把有序的左右两边合并到一起。

2，力扣手撕归并实战，成功：

```java
class Solution {
    public int[] sortArray(int[] nums) {
        /**
        归并：把数组二分到两个为止，然后左右构建有序数组；然后返回上一层，根据更大一些的数组的两边有序数组，合并两边构建一个有序数组；
         */
        
        //预先分配号临时数组的内存，避免递归时频繁申请数组空间
        int[] temp=new int[nums.length];
        merge_sort(nums,0,nums.length-1,temp);

        return nums;

    }
    
    //递归的让左右两半的数组归并排序为有序数组，然后把有序的左右数组合并成大的有序数组
    public void merge_sort(int[] nums,int left,int right,int[] temp){
        //不停分割数组，直到最后数组中只存在两个数的时候才开始做归并
        if(left<right){
            //获取当前数组的位置中点;使用防整数溢出的写法
            int mid=left+(right-left)/2;
            /**
            对数组左右继续分割，让他们从底层开始归并排序。
            
            mid必须放在左半边，因为划分到最后的index为left==0且right==1，此时mid==0，
            如果mid放在右边那么右边的下一层递归的数组范围为[mid,right]即[0,1]没有缩小范围，
            会一直递归下去导致栈溢出。
             */
            merge_sort(nums,left,mid,temp);
            merge_sort(nums,mid+1,right,temp);

            //把mid左右的已有序的数组合并成一个大的有序数组
            merge(nums,left,mid,right,temp);
        }
        //如果没进入上面的if，说明数组已经分割为了单个元素，那么直接返回上一层递归，把左右两个元素合并为一个长度为2的有序数组

    }

    //把有序的左右数组合并成大的有序数组
    public void merge(int[] nums,int left,int mid,int right,int[] temp){
        //避免修改left和right，因为这两者要作为合并的终点。因为merge_sort中解释了mid要放到左边处理，所以这里处理的右边的第一个元素设置为mid+1
        int i=left,j=mid+1;
        //设置temp数组的初始元素位置
        int t=0;
        while(i<=mid&&j<=right){
            //把左右数组当前元素中小的那个加入temp数组，从而构建了升序的temp数组
            if(nums[i]<nums[j]){
                temp[t++]=nums[i++];
            }else{
                temp[t++]=nums[j++];
            }
        }
        //左半或右半数组有一个会先被处理完，就把右半或左半数组中剩余的元素加入temp
        while(i<=mid){
            temp[t++]=nums[i++];
        }
        while(j<=right){
            temp[t++]=nums[j++];
        }

        /**
        把temp数组中的元素复制到原始数组即结果数组中,这个时候可以直接操作left和right了;
        即把temp数组的[0,right-left]位置的元素移到nums数组的[left,right]处，使nums数组的[left,right]处为有序数组
         */
        t=0;
        while(left<=right){
            nums[left++]=temp[t++];
        }
    }
}
```

#### 堆排序

1，参考知识：

- 简易视频：https://www.youtube.com/watch?v=MtQL_ll5KhQ

- 详细视频：https://www.bilibili.com/video/BV1b7411N798?p=83
- 知乎：https://zhuanlan.zhihu.com/p/124885051
  - 完全二叉树解释：https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232
  - 完全二叉树的三个重要知识：
    - 节点坐标为i，左子节点坐标为`2i+1`
    - 节点坐标为i，右子节点坐标为`2i+2`
    - 平衡二叉树第一个非叶子节点的坐标为`floor(length/2)-1`。[来源](https://blog.csdn.net/weixin_41558411/article/details/104950162)
  - 完全二叉树，平衡二叉树，满二叉树，[区别](https://blog.csdn.net/hytdsky/article/details/4783707)
- 复杂度：时间复杂度onlogn，空间复杂度on

2，大佬代码：

```java
import java.util.Arrays;
public class HeapSort {

    private int[] arr;

    public HeapSort(int[] arr){
        this.arr = arr;
    }

    /**
     * 堆排序的主要入口方法，共两步。
     */
    public void sort(){
        /*
         *  第一步：将数组堆化
         *  beginIndex = 第一个非叶子节点。
         *  从第一个非叶子节点开始即可。无需从最后一个叶子节点开始。
         *  叶子节点可以看作已符合堆要求的节点，根节点就是它自己且自己以下值为最大。
         */
        int len = arr.length - 1;
        int beginIndex = (len - 1) >> 1;
        for(int i = beginIndex; i >= 0; i--){
            maxHeapify(i, len);
        }

        /*
         * 第二步：对堆化数据排序
         * 每次都是移出最顶层的根节点A[0]，与最尾部节点位置调换，同时遍历长度 - 1。
         * 然后从新整理被换到根节点的末尾元素，使其符合堆的特性。
         * 直至未排序的堆长度为 0。
         */
        for(int i = len; i > 0; i--){
            swap(0, i);
            maxHeapify(0, i - 1);
        }
    }

    private void swap(int i,int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    /**
     * 调整索引为 index 处的数据，使其符合堆的特性。
     *
     * @param index 需要堆化处理的数据的索引
     * @param len 未排序的堆（数组）的长度
     */
    private void maxHeapify(int index,int len){
        int li = (index << 1) + 1; // 左子节点索引
        int ri = li + 1;           // 右子节点索引
        int cMax = li;             // 子节点值最大索引，默认左子节点。

        if(li > len) return;       // 左子节点索引超出计算范围，直接返回。
        if(ri <= len && arr[ri] > arr[li]) // 先判断左右子节点，哪个较大。
            cMax = ri;
        if(arr[cMax] > arr[index]){
            swap(cMax, index);      // 如果父节点被子节点调换，
            maxHeapify(cMax, len);  // 则需要继续判断换下后的父节点是否符合堆的特性。
        }
    }

    /**
     * 测试用例
     *
     * 输出：
     * [0, 0, 0, 1, 1, 1, 2, 2, 2, 3, 3, 3, 4, 4, 4, 5, 5, 5, 6, 6, 6, 7, 7, 7, 8, 8, 8, 9, 9, 9]
     */
    public static void main(String[] args) {
        int[] arr = new int[]{3,5,3,0,8,6,1,5,8,6,2,4,9,4,7,0,1,8,9,7,3,1,2,5,9,7,4,0,2,6};        
        new HeapSort(arr).sort();        
        System.out.println(Arrays.toString(arr));
    }
```

3，自己手撕实战成功：

```java
class Solution {
    public int[] sortArray(int[] nums) {
        /**
        堆排序：
        1.把数组构建成bfs形式打印出来的大顶堆
        2.在已有大顶堆的基础上，每次把堆最后一个元素替换掉堆顶，并使堆大小减一，重新堆化；直到堆只剩最后一个节点
         */
        
        //构建堆
        int len=nums.length;//输入的数组的长度
        int beginIndex=(len>>1)-1;//从完全二叉树的第一个非叶子节点开始往前堆化，堆化就是让以本节点为根的树的所有根节点大于自己的左右子树中的所有节点
        for(int i=beginIndex;i>=0;i--){//把数组转化成大顶堆的格式
            maxHeapify(nums,i,len-1);
        }

        //排序，把堆顶不停放置到堆的末尾，达到升序排序
        for(int i=len-1;i>=0;i--){
            swap(nums,0,i);//交换堆的末尾和堆顶
            maxHeapify(nums,0,i-1);//为了保证拿出一个节点后的堆是大顶堆，因为换上大顶堆顶的值可能不是最大的，所以抓住这个值对树进行大顶堆化
        }

        return nums;

    }

    //交换nums数组中，i和j位置的值
    public void swap(int[] nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }

    //保证以坐标i的元素为根，大小不超过size的完全二叉树是大顶堆，采用了递归的方式
    public void maxHeapify(int[] nums,int i,int size){
        //根据完全二叉树的性质，求出当前节点i的左右子节点的坐标
        int leftChildIndex=(i<<1)+1;
        int rightChildIndex=leftChildIndex+1;
        int maxChildIndex=leftChildIndex;//默认的左右两个子节点中，左子节点的值更大，设置最大子节点十分必要

        if(leftChildIndex>size){//如果左子节点的坐标不在讨论的树的范围内，则说明当前i节点不违反大顶堆的性质，直接返回
            return;
        }
        if(rightChildIndex<=size&&nums[leftChildIndex]<nums[rightChildIndex]){//如果左右子节点都在讨论的树的范围内，且右子节点更大，则让maxChild为右子节点
            maxChildIndex=rightChildIndex;
        }
        //比较最大子节点，和当前节点的大小关系；保证当前节点比左右子节点大
        if(nums[i]<nums[maxChildIndex]){
            swap(nums,i,maxChildIndex);//因为最大子节点被换到堆顶的，所以可以确保swap后的堆顶比另一个子节点大
            maxHeapify(nums,maxChildIndex,size);//保证以交换到子节点的数为根节点的树，为大顶堆。
        }
    }

}
```

#### 变种题

1，实战可见于本文的“148. 排序链表”。对链表进行排序比较高频，我面试也碰到过，一些思路可以参考：

- [快速排序的方式参考](https://www.bilibili.com/video/BV1HA411q7tH?p=3&vd_source=8be62db2c8e19174231a64770292e191)。array中，会设置左右两个指针，往相对的方向进行遍历，使得middle的左边小右边大；但是这种方式不适合单链表。本方法是两个指针同向遍历。
  - 我：这个做法，知识交换节点的值，并不是真的交换节点。真的要交换节点的话也简单。
    - 但是允许的话，还是用归并是最合适的！
- 单链表排序常用**归并排序**来做。
  - 我：归并从底层开始，合并两个有序链表

### DP

1，贪心和DP问题的区别在哪？

- 参考：https://www.zhihu.com/question/36662980。

## 栈与队列(简单)

### 知识

#### Queue和Deque用法

1，[参考](http://t.csdn.cn/h7UcY)。尽量只用双向队列deque，它可以实现栈和队列；

- 为了不抛异常，应该用`offer poll peek`,结合first和last。
- deque本身是可以直接当成栈使用的，可以调用push() pop() peek() 这三个栈的关键方法。

#### 集合

1，linkedlist一般用来实现list接口，linkedlist中：

- 删除：有remove(int index)和remove(Object obj)，这两个方法是linkedlist自己的而不是从别的类或接口继承的，区分这两可以[参考](https://www.cnblogs.com/CCTVCHCH/p/14778299.html)
- 增加：boolean add(E e)在链表末尾增加指定元素，从collection deque list queue AbstractList继承过来的；void add(int index,E e)在指定位置添加元素，从List接口实现的。
  - 以上增加和删除提到的四个函数，在arraylist也完全一样用法。

2，PriorityQueue从AbstractQueue继承了remove(Object obj)，但是并没有remove(int index)

### [剑指

### [ Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

#### 首战寄

有思路但是不知道怎么用代码实现

#### 官方1-双栈

思路和算法：

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

代码：

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

复杂度分析：

- 时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)O(1)。插入不多说，对于删除操作，虽然看起来是 O(n)O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)O(1)。

- 空间复杂度：O(n)O(n)。需要使用两个栈存储已有的元素。

反思：

- 20220302：
  - 不记得java中stack怎么实现
  - 知道用两个栈实现队列。但是针对删除，想的是“插入队列移动到删除队列，删除元素后，要把删除队列的所有元素移动回插入队列”，但是实际上不需要把删除队列的元素移动回去；
    - 正确思路就是如题解：删除队列负责删除，插入队列负责插入，删除队列空了才能把元素从插入队列移入删除队列，如果删除队列和插入队列同时为空则返回-1.

#### 即时再战

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

- 反思20220426：栈不推荐用stack，这个已经废弃了，直接用deque为好！

### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

#### 首战告捷

```java
class MinStack {

    /** initialize your data structure here. */
    Stack<Integer> myStack=null;
    Stack<Integer>historicalMin=null;
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

#### 官方-辅助栈

解题思路：

普通栈的 push() 和 pop() 函数的复杂度为 O(1)；而获取栈最小值 min() 函数需要遍历整个栈，复杂度为 O(N)。

本题难点： 将 min() 函数复杂度降为 O(1)，可通过建立辅助栈实现；

- 数据栈 A： 栈 A用于存储所有元素，保证入栈 push() 函数、出栈 pop() 函数、获取栈顶 top() 函数的正常逻辑。
- 辅助栈 B ： 栈 B 中存储栈 A 中所有 非严格降序（） 的元素，则栈 A中的最小元素始终对应栈 B的栈顶元素，即 min() 函数只需返回栈 B 的栈顶元素即可。
  - 我：非降序是 “非降-序”,而不是 “非-降序”，表示序列是增的样式；非严格降序，就是在大体是增的排序上，允许出现相连的相等的数。


因此，只需设法维护好 栈 B的元素，使其保持非严格降序，即可实现 min() 函数的 O(1)复杂度。

函数设计：

- push(x) 函数： 重点为保持栈 B 的元素是 非严格降序 的。

  - 将 x 压入栈 A（即 A.add(x) ）；

  - 若 ① 栈 B 为空 或 ② xx 小于等于 栈 B 的栈顶元素，则将 xx 压入栈 B （即 B.add(x) ）。

- pop() 函数： 重点为保持栈 A,B 的 元素一致性 。

  - 执行栈 A 出栈（即 A.pop() ），将出栈元素记为 y ；

  - 若 y 等于栈 B 的栈顶元素，则执行栈 B 出栈（即 B.pop() ）。

- top() 函数： 直接返回栈 A 的栈顶元素即可，即返回 A.peek() 。

- min() 函数： 直接返回栈 B 的栈顶元素即可，即返回 B.peek() 。

复杂度分析：

- 时间复杂度 O(1) ： push(), pop(), top(), min() 四个函数的时间复杂度均为常数级别。
- 空间复杂度 O(N)： 当共有 N 个待入栈元素时，辅助栈 BB 最差情况下存储 N 个元素，使用 O(N) 额外空间。

代码：

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

- 我：重申，栈用deque实现！
- 我：单调队列（比如“剑指 Offer 59 - I. 滑动窗口的最大值”用到的）是o1拿到队列窗口内部的最值，是队列问题，队列是先进先出；本题是栈遵循先进后出，所以与单调队列问题相区别一下！所以本题实现o1拿到栈中最小值很简单，只需要辅助栈；但是窗口队列想o1拿到最小值就只能用单调队列实现了！

## 栈与队列(困难)

### [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

codetop 7

#### 首战寄

没有好思路，只想到：遍历时没到达一个数，就遍历窗口内部拿到最大值，这么写没什么意义

#### 大佬-priorityQueue

优先队列秒了没有超时， 不能该是困难题吧

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length==0) return new int[0];
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>((o1,o2)->{return o2-o1;});
        int[] res = new int[nums.length-k+1];
        for(int i = 0;i<k-1;++i) queue.offer(nums[i]);
        for(int i = 0;i<res.length;++i){
            queue.offer(nums[i+k-1]);
            res[i] = queue.peek();
            queue.remove(nums[i]);
        }
        return res;
    }
}
```

- 网友评价：虽然时间复杂度是 nlogk ，但胜在思想和编写都简单。
  - 我：因为logk可以大于1，所以k比较大的时候，时间复杂度比单调队列还是差不少

- 我：因为PQ内部会把元素排序，不是先进先出，而是保证最值先出；所以得用remove(Object obj)来删除指定元素；用poll则每次把PQ的最大值移除了，不合理。
  - [javadoc官方描述PQ](https://docs.oracle.com/javase/8/docs/api/)：PQ只有remove(obj)，没有remove(index)

- [PriorityQueue详解](https://blog.csdn.net/hellokitty136/article/details/105831884)，

- 我**注意**：这里是本文目前第一次出现**lamda表达式**实现函数式接口，我总结下lamda表达式的用法：
  - 正规lamda表达式是()->{}，但是只有一行的时候`{}`可以省略，省略{}后必须省略`return`和`;`；省略return后，return的内容可以用小括号包裹增加可读性，也可以不用。下面罗列可以or不可以的写法：
    - 可以：`(o1,o2)-> {return o2-o1;}`，`(o1,o2)-> o2-o1`，`(o1,o2)-> (o2-o1)`，
    - 不可以：`(o1,o2)-> {return o2-o1}`，`(o1,o2)-> {o2-o1}`，`(o1,o2)-> return o2-o1`，`(o1,o2)-> return o2-o1;`
    - 总结规律：`{}return;`三者必须同存亡


#### 官方-单调队列

解题思路：

![image-20220418193659791](lcof.assets/image-20220418193659791.png)

![Picture1.png](lcof.assets/1600878237-pBiBdf-Picture1.png)

![image-20220418193737505](lcof.assets/image-20220418193737505.png)

- 问答问：获取窗口中最大值的时间复杂度是降到o1了，但是每次元素添加进deque，都需要遍历删除所有值比新加进来的元素小的元素，这我感觉也是ok的时间复杂度，那么总的时间复杂度是不是没变？
  - 自答：就像分析时间复杂度处说的，deque中每个元素最多只会入队一次和出队一次，并且元素出队时removelast的时间复杂度是o1；所以就算是批量一次性删除多个元素，那么后续需要删除的元素就少了（总数最多会有n个元素入队），所以这块不构成on中嵌套ok的时间复杂度，而是on+on==o2n==on的时间复杂度。

算法流程：

![image-20220418194145067](lcof.assets/image-20220418194145067.png)

复杂度分析：

![image-20220418194228120](lcof.assets/image-20220418194228120.png)

代码：

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        //单调队列
        //下面是要注意的点：
        //队列按从大到小放入
        //如果首位值（即最大值）不在窗口区间，删除首位
        //如果新增的值小于队列尾部值，加到队列尾部
        //如果新增值大于队列尾部值，删除队列中比新增值小的值，再把新增值加入到队列中
        //如果新增值大于队列中所有值，删除所有，然后把新增值放到队列首位，保证队列一直是从大到小
        if (nums.length == 0)   return nums;

        Deque<Integer> deque = new LinkedList<>();
        int[] arr = new int[nums.length - k + 1];
        int index = 0;  //arr数组的下标
        //未形成窗口区间
        for (int i = 0; i < k; i++) {
            //队列不为空时，当前值与队列尾部值比较，如果大于，删除队列尾部值
            //一直循环删除到队列中的值都大于当前值，或者删到队列为空
            while (!deque.isEmpty() && nums[i] > deque.peekLast())  deque.removeLast();
            //执行完上面的循环后，队列中要么为空，要么值都比当前值大（因为单调队列是递减的，由此保证单调队列的第一个值是滑动窗口范围最大），然后就把当前值添加到队列中
            deque.addLast(nums[i]);
        }
        //窗口区间刚形成后，把队列首位值添加到队列中
        //因为窗口形成后，就需要把队列首位添加到数组中，而下面的循环是直接跳过这一步的，所以需要我们直接添加
        arr[index++] = deque.peekFirst();
        //窗口区间形成
        for (int i = k; i < nums.length; i++) {
            //i-k是已经在区间外了，如果首位等于nums[i-k]，那么说明此时首位值已经不再区间内了，需要删除
            if (deque.peekFirst() == nums[i - k])   deque.removeFirst();
            //删除队列中比当前值小的值
            while (!deque.isEmpty() && nums[i] > deque.peekLast())  deque.removeLast();
            //把当前值添加到队列中
            deque.addLast(nums[i]);//我：总是要加入当前值；当前值可能会在下一个值被插入时删掉（如果当前值比下一个值小的话）
            //把队列的首位值添加到arr数组中
            arr[index++] = deque.peekFirst();
        }
        return arr;
    }
}
```

#### 即时再战成功

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        /**
        使用单调队列
         */

        //处理特殊情况
        if(nums.length==0)return new int[0];

        //新建单调队列和存结果的数组
        Deque<Integer> queue=new LinkedList<>();
        int[] res=new int[nums.length-k+1];

        //处理窗口未完全覆盖数组前的情况,即没有元素从窗口左边滑出的情况。i表示窗口未完全展现时的右边缘,0~k-1。删除和增加都是从last做，这样保证first总是最大元素。
        for(int i=0;i<k;i++){
            //把队列中比当前元素小的数都删掉，由于每个元素只会入栈出栈一次，所以这个while不会造成嵌套而成的乘法时间复杂度。
            while(!queue.isEmpty()&&queue.peekLast()<nums[i]){//!!!!while要确保queue不为空，否则会做peeklast会有空指针异常
                queue.pollLast();
            }
            //把当前元素加入单调队列末尾，此时保证了队列中不会有比它还小的元素
            queue.offerLast(nums[i]);
        }

        //此时窗子的左边缘正好在nums[0]，把此时窗口中的最大值记录下来
        res[0]=queue.peekFirst();

        //此时窗子的左边缘正好在nums[0]，接下来的滑动窗口就会有元素从窗户左边滑出了。这里的i还是以滑动窗口的右边缘为准
        for(int i=k;i<nums.length;i++){
            //先处理出队，再处理入队，这样可以保证队列单调递减
            //如果出队的元素是queue的最大元素，就把它从queue中移除
            if(nums[i-k]==queue.peekFirst()){
                queue.pollFirst();
            }
            //窗口有边缘要往queue中进元素时，老规矩，先把queue中比要进的元素小的元素删除，再进元素
            while(!queue.isEmpty()&&queue.peekLast()<nums[i]){
                queue.pollLast();
            }
            queue.offerLast(nums[i]);

            //把queue的头部元素放入res，因为它就是此时窗口中最大的元素；第一次添加时窗口左侧在nums[1]
            res[i-k+1]=queue.peekFirst();
        }

        return res;

    }
}
```

### [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

codetop 5

#### 首战半寄

代码都写出来了，但是花了很多时间debug阿里手册提过的integer用equals比较的问题，所以算”半寄“

```java
class MaxQueue {
    /**
    pushback和popfront用队列实现，都天然是o1的时间复杂度
    maxvalue就得用一个额外的单调队列，来记载此时存储队列中的最大值
     */
    //单调队列，用于保存当前deque的（潜在）最大值
    Deque<Integer> monotonousQueue=null;
    //当前deque，用于存储数据
    Deque<Integer> queue=null;

    //构造函数为本对象分配内存
    public MaxQueue() {
        monotonousQueue=new LinkedList<>();
        queue=new LinkedList<>();
    }
    
    //monotonousQueue的头部总是当前队列的最大值；如果队列为空则返回-1；
    public int max_value() {
        //printQueues();
        if(monotonousQueue.isEmpty()){
            return -1;
        }else{
            return monotonousQueue.peekFirst();
        }
    }
    
    public void push_back(int value) {
        //数据直接往queue中放；
        queue.offerLast(value);

        //把monotonousQueue中比value小的节点删掉后，把value放入monotonousQueue的尾部，保证monotonousQueue单调递减
        while(!monotonousQueue.isEmpty()&&monotonousQueue.peekLast()<value) monotonousQueue.pollLast();
        monotonousQueue.offerLast(value);
    }
    
    public int pop_front() {
        //如果队列为空，不需要实际pop，直接返回-1
        if(queue.isEmpty())return -1;

        //队列不为空，查看要pop的是不是monotonousQueue的头节点，即queue中的最大节点；如果是的话，则删掉monotonousQueue中的头结点。注意queue.peekFirst().equals(monotonousQueue.peekFirst())不能写成用==比较，因为返回的是Integer对象会出现无法判等于的情况，原因见（https://blog.csdn.net/qq_39677544/article/details/78849988），这是阿里手册提过的大坑！！
        if(queue.peekFirst().equals(monotonousQueue.peekFirst()))monotonousQueue.pollFirst();
        //处理完monotonousQueue后，直接把queue的头节点即要出队的节点返回
        return queue.pollFirst();
    }

    //debug测试时用的，提交时可以注释掉
    public void printQueues(){
        System.out.println("====================当前queue为：");
        Iterator<Integer> iterator1 = queue.iterator();
        while(iterator1.hasNext()){
            System.out.print(iterator1.next()+" ");
        }
        System.out.println("====================当前queue结束");

        System.out.println("=====================当前monotonousQueue为：");
        Iterator<Integer> iterator2 = monotonousQueue.iterator();
        while(iterator2.hasNext()){
            System.out.print(iterator2.next()+" ");
        }
        System.out.println("=====================当前monotonousQueue结束");

    }
}

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue obj = new MaxQueue();
 * int param_1 = obj.max_value();
 * obj.push_back(value);
 * int param_3 = obj.pop_front();
 */
```

- 注意： 大坑，Integer和String在比较内容值大小时，要使用equals，而不是使用比较地址的`==`。[原因](https://blog.csdn.net/qq_39677544/article/details/78849988)
- 打印集合可以用forEach方式：`for(Map.Entry<Integer,String> entry:myHashMap.entrySet())`，表示foreach遍历一个key为integer，value为string的hashmap。

#### 官方-单调双向队列

解题思路：

![image-20220419161423045](lcof.assets/image-20220419161423045.png)



![Picture1.png](lcof.assets/1609261470-WanZuG-Picture1.png)

![image-20220419161504057](lcof.assets/image-20220419161504057.png)

![Picture2.png](lcof.assets/1609261470-gMTEAf-Picture2.png)

![image-20220419161545885](lcof.assets/image-20220419161545885.png)

函数设计：

![image-20220419161614034](lcof.assets/image-20220419161614034.png)

复杂度分析：

![image-20220419161638177](lcof.assets/image-20220419161638177.png)

代码：

```java
class MaxQueue {
    Queue<Integer> queue;
    Deque<Integer> deque;
    public MaxQueue() {
        queue = new LinkedList<>();
        deque = new LinkedList<>();
    }
    public int max_value() {
        return deque.isEmpty() ? -1 : deque.peekFirst();
    }
    public void push_back(int value) {
        queue.offer(value);
        while(!deque.isEmpty() && deque.peekLast() < value)
            deque.pollLast();
        deque.offerLast(value);
    }
    public int pop_front() {
        if(queue.isEmpty()) return -1;
        if(queue.peek().equals(deque.peekFirst()))
            deque.pollFirst();
        return queue.poll();
    }
}

```

- 网友问（和我栽进一个坑了）：`queue.peek().equals(deque.peekFirst())`，询问个问题，为何==的时候就会不对，能讲下区别吗？
  - 网友答：queue 里面保存的是 Integer 而非 int ，peek() 返回的是 Integer 类型，没有自动拆箱，因此需要用 equals() 来比~
  - 网友答：比如Integer i1 = new Integer(1); Integer i2 = new Integer(1) 这样子比较i1 == i2是返回false的，因为比较的是对象， 同理 Integer i1 = 1; Integer i2 = new Integer(1);这样i1==i2也是返回false的，因为i1自动装箱了。 但是Integer i1 = 1; Integer i2 =1;这样i1==i2，返回是true， 因为自动装箱缓存机制，返回的同一个对象给i1和i2。在-128~127（比较常用？）的整数范围内都会存在自动拆装箱的缓存，所以126 和 900 的区别就在这里（126两个都是返回的同一个对象，900超过范围了返回的不是同一个对象）。 你也可以自己设定缓存的整数范围。 两个都是包装类最好用.equals()方法比较，这样比较的是值。 如果一个是int类型，一个是包装类，那么可以直接用==，因为包装类自动拆箱为int类型了，所以这时候比较的也是值，而不会存在一个是基本类型，一个是引用类型不能比较，这就是自动拆装箱。

- 优先队列vs单调队列：

  - 自答：

    - 之前有点迷糊，单调队列和优先队列都能保障队列头为当前输入(窗口)的最大值，优先队列的时间复杂度还更高，那为什么不总是使用单调队列?

    - 后面我想了下，优先队列能保证存储所有元素，但是单调队列为了保证单调会删掉众多元素，所以有存储所有节点的场景需要的话，只用单调队列是不行的。

  - 网友：这俩使用场景区别还挺大的吧，单调队列经典题接雨水，用优先队列不行吧

  - 网友：你只考虑最大为什么要优先队列和单调队列，直接maxx遍历取max就好了
    用到这俩也是结束遍历的时候两个队列存的东西不一样
    大家不要带偏了啊，你只关系当前最大用变量存max就好了

### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

codetop==137

#### 首战寄

没思路

#### [进阶优化思路](https://leetcode.cn/problems/trapping-rain-water/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-8/)

1，进阶的思路不错，但是双指针那讲的不算好，要结合评论和b站视频来理解。

2，这里着重掌握思路优化的最后一步，即单调栈的做法

#### 大佬-单调不增栈

思路：

1. 使用单调栈，[单调栈入门](https://leetcode.cn/problems/largest-rectangle-in-histogram/solution/84-by-ikaruga/)

2. 单调递减栈

   21. 理解题目，参考图解，注意题目的性质，当后面的柱子高度比前面的低时，是无法接雨水的

   22. 当找到一根比前面高的柱子，就可以计算接到的雨水
   23. 所以使用单调递减栈

3. 对更低的柱子入栈

    	31. 更低的柱子以为这后面如果能找到高柱子，这里就能接到雨水，所以入栈把它保存起来
    	32. 平地相当于高度 0 的柱子，没有什么特别影响

4. 当出现高于栈顶的柱子时

   41. 说明可以对前面的柱子结算了
   41. 计算已经到手的雨水，然后出栈前面更低的柱子


5. 计算雨水的时候需要注意的是
   51. 雨水区域的右边 r 指的自然是当前索引 i
   52. 底部是栈顶 st.top() ，因为遇到了更高的右边，所以它即将出栈，使用 cur 来记录它，并让它出栈
   53. 左边 l 就是新的栈顶 st.top()
   54. 雨水的区域全部确定了，水坑的高度就是左右两边更低的一边减去底部，宽度是在左右中间
   55. 使用乘法即可计算面积

图解：

- [参考官方](https://leetcode.cn/problems/trapping-rain-water/solution/jie-yu-shui-by-leetcode-solution-tuvc/)的“方法二：单调栈”的图解

代码，参考了高赞的java版代码：

```java
class Solution {
    public int trap(int[] walls) {
        if (walls == null || walls.length <= 2) {
            return 0;
        }

        // 思路：
        // 单调不增栈，walls元素作为右墙依次入栈
        // 出现入栈元素（右墙）比栈顶大时，说明在右墙左侧形成了低洼处，低洼处出栈并结算该低洼处能接的雨水

        int water = 0;
        Stack<Integer> stack = new Stack<>();
        for (int right=0; right<walls.length; right++) {
            // 栈不为空，且当前元素（右墙）比栈顶（右墙的左侧）大：说明形成低洼处了
            while (!stack.isEmpty() && walls[right]>walls[stack.peek()]) {
                // 低洼处弹出，尝试结算此低洼处能积攒的雨水
                int bottom = stack.pop();
                // 看看栈里还有没有东西（左墙是否存在）
                // 有右墙+有低洼+没有左墙=白搭
                if (stack.isEmpty()) {
                    break;
                }

                // 左墙位置以及左墙、右墙、低洼处的高度
                int left = stack.peek();
                int leftHeight = walls[left];
                int rightHeight = walls[right];
                int bottomHeight = walls[bottom];

                // 能积攒的水=(右墙位置-左墙位置-1) * (min(右墙高度, 左墙高度)-低洼处高度)
                water += (right-left-1) * (Math.min(leftHeight, rightHeight)-bottomHeight);
            }

            // 上面的pop循环结束后再push，保证stack是单调不增
            stack.push(right);
        }

        return water;
    }
}
```

- 我：每个右墙（right）都会入栈，但是在入栈前，都会把栈中比当前right小的元素弹出，并计算接到的水的数量；这样就实现的是名副其实的单调不增栈！

#### 即时再战成功-单调栈

```java
class Solution {
    public int trap(int[] height) {
        /**
        单调不增栈：
        1. right依次入栈。入栈前确保栈顶不比自己小，并计算承接的雨水的量。这样就保证栈是单调不增栈
         */
        
        //保存水量
        int water=0;

        //新建栈保存index，根据height和index就可以得到各个柱子的高度。deque支持栈的方法使用，即push和pop
        Deque<Integer> stack=new LinkedList<>(); 

        //把每个柱子作为right入栈
        for(int right=0;right<height.length;right++){
            //把right左边的水量统计出来；并确保栈顶不小于right的情况下才能压入right，这样栈才是单调不增栈
            while(!stack.isEmpty()&&height[stack.peek()]<height[right]){
                //取出栈顶，准备计算夹着栈顶的left墙和right墙能存多少水
                int bottom=stack.pop();
                //找左墙。如果没有左墙则当前right+bottom无法存水
                if(stack.isEmpty()) break;

                //根据左墙 bottom 右墙，计算bottom对应的存水量
                int left=stack.peek();//注意只有拿bottom值才弹栈；其他时候取栈中的值都用peek
                int leftHeight=height[left];
                int rightHeight=height[right];
                int bottomHeight=height[bottom];
                water+=(right-left-1)*(Math.min(leftHeight,rightHeight)-bottomHeight);
            }

            stack.push(right);
        }

        return water;

    }
}
```



## 链表(简单)

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
  - 我补充：这里递归也要占有栈空间，最大深度时n，所以空间复杂度也是O(n)；这么一看还是栈的方法好，可以避免stackoverflow。
- 我20220426：递归函数传参容易把自己弄混，一般最好使用全局变量和数组；不过本题递归用到了本地变量deep倒序记录递归层级，所以这里不推荐使用全局变量。

---

#### 官方-使用栈

思路：

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

代码：

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
- 我：重申队列用deque。

复杂性分析：

- 时间复杂度：O(n)。正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。

- 空间复杂度：O(n)。额外使用一个栈存储链表中的每个节点。



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

- 我：这里实际用了三指针 prev cur next

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

#### 20230120再战成功

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
    public ListNode reverseList(ListNode head) {
        //处理特殊情况
        if(head==null||head.next==null) return head;

        /**
        使用三个指针反转链表，直到cur.next为null
        1，需要初始化的只有cur；pre需要设置为null才能让原头结点指向null；next不需要初始化，可以在循环中赋值，所以这里先设为null
         */
        ListNode prev=null,cur=head,next=null;
        while(cur!=null){
            //先用next接住断开前的下一个节点
            next=cur.next;
            //调转cur的指向
            cur.next=prev;
            //prev后移一位
            prev=cur;
            //cur后移一位
            cur=next;
        }

        //循环结束后。prev是头结点；cur和next都指向null
        return prev;
    }
}
```

- 我：用的是经典的三指针迭代法。我记得迭代比递归好，因为不会栈溢出。

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

### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

codetop==481

剑指写过

#### 首战告捷-迭代法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {

        //特殊值处理
        if(head==null)return null;

        //迭代方式翻转链表
        ListNode prev=null,cur=head;
        //交换cur和prev之间的指向
        while(cur!=null){
            ListNode temp=cur.next;
            cur.next=prev;
            prev=cur;
            cur=temp;
        }

        //遍历结束时，prev为头结点
        return prev;

    }
}
```

#### 首战告捷-递归法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        /**
        递归的方式，每次都返回最后一个节点。在当前递归交换两个节点间的指向
         */
        //处理特殊情况，并设置递归触底条件。如果传入的是空节点，则直接返回null，结束程序；如果传入的不是null，那么只会在“head.next==null”触底
        if(head==null||head.next==null){
            return head;
        }

        //类似树的后续遍历，先递归拿到下一层的结果，这样对应退栈的时候从链表尾部往链表头部的方向处理。总是把逆序后的头结点返回。
        ListNode newHead=reverseList(head.next);

        //让当前节点的下一节点指向当前节点，即交换cur和later的指向。因为之前判断过head.next!=null，所以head.next.next不会有空指针异常
        head.next.next=head;
        //当前节点指向null是为了让逆序链表的最后一个节点能指向null
        head.next=null;

        return newHead;
    }
}
```

### [146. LRU 缓存](https://leetcode-cn.com/problems/lru-cache/)

#### 首战寄

想到用hash达到o1时间复杂度的查找，但是实施细节的思路不是很清晰。

#### 官方-LinkedHashMap版(不推荐)

前言
实现本题的两种操作，需要用到一个哈希表和一个双向链表。在面试中，面试官一般会期望读者能够自己实现一个简单的双向链表，而不是使用语言自带的、封装好的数据结构。在 Python 语言中，有一种结合了哈希表与双向链表的数据结构 OrderedDict，只需要短短的几行代码就可以完成本题。在 Java 语言中，同样有类似的数据结构 LinkedHashMap。这些做法都不会符合面试官的要求，因此下面只给出使用封装好的数据结构实现的代码，而不多做任何阐述。

```java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}
```



#### 官方-哈希表 + 双向链表

算法：

![image-20220504174444905](lcof.assets/image-20220504174444905.png)

小贴士：

- 在双向链表的实现中，使用一个伪头部（dummy head）和伪尾部（dummy tail）标记界限，这样在添加节点和删除节点的时候就不需要检查相邻的节点是否存在。

复杂度分析:

![image-20220504174854682](lcof.assets/image-20220504174854682.png)

代码：

```java
public class LRUCache {
    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        public DLinkedNode() {}
        public DLinkedNode(int _key, int _value) {key = _key; value = _value;}
    }

    private Map<Integer, DLinkedNode> cache = new HashMap<Integer, DLinkedNode>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 使用伪头部和伪尾部节点
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }5
        // 如果 key 存在，先通过哈希表定位，再移到头部
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            // 如果 key 不存在，创建一个新的节点
            DLinkedNode newNode = new DLinkedNode(key, value);
            // 添加进哈希表
            cache.put(key, newNode);
            // 添加至双向链表的头部
            addToHead(newNode);
            ++size;
            if (size > capacity) {
                // 如果超出容量，删除双向链表的尾部节点
                DLinkedNode tail = removeTail();
                // 删除哈希表中对应的项//我：参考https://www.runoob.com/java/java-hashmap-remove.html
                cache.remove(tail.key);
                --size;
            }
        }
        else {
            // 如果 key 存在，先通过哈希表定位，再修改 value，并移到头部
            node.value = value;
            moveToHead(node);
        }
    }

    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }

    private DLinkedNode removeTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

- 我讲解remove：
  - `hashmap.remove(key)`可以删掉hashmap中的一个键值对
  - `priorityQueue.remove(value)`可以删掉优先队列中的值为value的元素
  - `list.remove(index)`可以删掉列表中第index个元素。

#### 即时再战成功

```java
class LRUCache {
    /**
    o1的get和put，那么就得想到hash
    超过capacity后删除最不常使用的kv，那么应该每次操作kv后都把被操作的kv移动到链表头部，这样尾部的就自然是最久没被使用过的kv了
     */

    //定义双向链表，这样才能记录最不常用的kv（链表尾部）
    class Node{
        //node中不仅要存value，还得存value对应的key；这样拿到node后才能操作hash映射，实现有hash key可以拿到node，有node可以拿到hash key。
        int key;
        int value;

        Node prev;
        Node next;

        //存在自定义有参构造函数时，总是显式声明一个无参构造
        public Node(){};
        public Node(int key,int value){
            this.key=key;
            this.value=value;
        }

    }

    //定义LRUCache类需要的其他变量，不需要初始化，初始化放到构造函数中做
    int capacity;
    int size;
    Map<Integer,Node> cache=null;
    //构建伪头尾节点，方便操作头尾的情况，并且避免空指针异常
    Node head;
    Node tail;

    //做所有初始化
    public LRUCache(int capacity) {
        //虽然this可有可无，但是都使用this标记，强调是给本类变量赋值
        this.capacity=capacity;
        this.size=0;
        this.cache=new HashMap<>();
        this.head=new Node(-1,-1);
        this.tail=new Node(-1,-1);
        this.head.next=this.tail;
        this.tail.prev=this.head;
    }
    
    public int get(int key) {
        //如果cache存在key对应的node，则返回node的值，并把node移动到双向链表头部
        if(cache.containsKey(key)){
            Node curNode=cache.get(key);
            move2head(curNode);
            return curNode.value;
        }else{//如果不存在这个node，那么直接返回-1
            return -1;
        }
    }
    
    public void put(int key, int value) {
        //如果cache存在key对应node，则更新value，并把node移动到双向链表头部
        if(cache.containsKey(key)){
            Node curNode =cache.get(key);
            curNode.value=value;
            move2head(curNode);
        }else{//如果当前cache不存在key对应的node，则添加一个node，并且add中要处理可能的超过capacity的情况
            Node curNode=new Node(key,value);
            add2head(curNode);//add包含在双向链表中add，和在cache中add
        }

    }

    private void remove(Node curNode){
        //从双向链表中删掉
        curNode.prev.next=curNode.next;
        curNode.next.prev=curNode.prev;
        
        //从cache中删掉。使用hashmap的根据key删除kv的remove方法
        cache.remove(curNode.key);

        size--;
    }
    private void add2head(Node curNode){
        //在双向链表中添加
        this.head.next.prev=curNode;
        curNode.prev=this.head;
        curNode.next=this.head.next;
        this.head.next=curNode;

        //在cache中添加
        cache.put(curNode.key,curNode);
        
        size++;
        
        //处理可能的超过capacity的情况
        if(this.size>this.capacity){
            remove(this.tail.prev);//remove包括在双向链表中remove，和在cache中remove；
        }

        
        
    }
    private void move2head(Node curNode){
        remove(curNode);
        add2head(curNode);
    }

}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

- 反思：
  - LRUCache构造函数初始化伪头尾节点后，不要忘记把伪头尾节点连接起来
  - 自定义的move2head等函数，虽然不需要返回值，但是需要在函数定义处显式设置void表示函数不需要返回，即别忘了写void。



### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

#### 首战寄

```java
/**
   使用伪头节点起步，然后用除法求段数；用求余判断有没有末尾剩余节点，操作完除法次数后，剩下的部分就是需要保证原有顺序的部分
*/
```

没有什么很好的思路

#### 官方

一图胜千言，根据图片看代码，马上就懂了

步骤分解:

![image-20220509001508854](lcof.assets/image-20220509001508854.png)

![k个一组翻转链表.png](lcof.assets/866b404c6b0b52fa02385e301ee907fc015742c3766c80c02e24ef3a8613e5ad-k个一组翻转链表.png)

- 我：像这种`相差k个`的问题，就可以想到用快慢指针来定位k的区段

代码：

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
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null){
            return head;
        }
        //定义一个假的节点。
        ListNode dummy=new ListNode(0);
        //假节点的next指向head。
        // dummy->1->2->3->4->5
        dummy.next=head;
        //初始化pre和end都指向dummy。pre指每次要翻转的链表的头结点的上一个节点。end指每次要翻转的链表的尾节点
        ListNode pre=dummy;
        ListNode end=dummy;

        while(end.next!=null){
            //循环k次，找到需要翻转的链表的结尾,这里每次循环要判断end是否等于空,因为如果为空，end.next会报空指针异常。
            //dummy->1->2->3->4->5 若k为2，循环2次，end指向2
            for(int i=0;i<k&&end != null;i++){
                end=end.next;
            }
            //如果end==null，即需要翻转的链表的节点数小于k，不执行翻转。
            if(end==null){
                break;
            }
            //先记录下end.next,方便后面链接链表
            ListNode next=end.next;
            //然后断开链表
            end.next=null;
            //记录下要翻转链表的头节点
            ListNode start=pre.next;
            //翻转链表,pre.next指向翻转后的链表。1->2 变成2->1。 dummy->2->1
            pre.next=reverse(start);
            //翻转后头节点变到最后。通过.next把断开的链表重新链接。
            start.next=next;
            //将pre换成下次要翻转的链表的头结点的上一个节点。即start
            pre=start;
            //翻转结束，将end置为下次要翻转的链表的头结点的上一个节点。即start
            end=start;
        }
        return dummy.next;


    }
    //链表翻转
    // 例子：   head： 1->2->3->4
    public ListNode reverse(ListNode head) {
         //单链表为空或只有一个节点，直接返回原单链表
        if (head == null || head.next == null){
            return head;
        }
        //前一个节点指针
        ListNode preNode = null;
        //当前节点指针
        ListNode curNode = head;
        //下一个节点指针
        ListNode nextNode = null;
        while (curNode != null){
            nextNode = curNode.next;//nextNode 指向下一个节点,保存当前节点后面的链表。
            curNode.next=preNode;//将当前节点next域指向前一个节点   null<-1<-2<-3<-4
            preNode = curNode;//preNode 指针向后移动。preNode指向当前节点。
            curNode = nextNode;//curNode指针向后移动。下一个节点变成当前节点
        }
        return preNode;

    }


}
```

- 网友：reverse和dummy绝了。

#### 即时再战成功

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        //处理特殊情况
        if(head==null||head.next==null)return head;

        //构建伪头节点方便翻转
        ListNode dummyHead=new ListNode(-1);
        dummyHead.next=head;

        //pre为待翻转段的前一个节点
        ListNode prev=dummyHead;
        ListNode end=prev;
        //分段翻转整个链表。因为要断开end和后续链表部分，所以end.next不能为空才能“断开”
        while(end.next!=null){
            //把end移动到当前段的末尾，要防止移动的时候触碰到链表边界导致空指针异常
            for(int i=0;i<k&&end!=null;i++){
                end=end.next;
            }

            //如果for循环结束后，end==null，说明当前链表段的长度不够翻转，那么说明链表处理完了，直接break。
            //正常情况下，end停留在需要翻转的链表段的最后一个节点，肯定不是null
            if(end==null)break;

            //保存end后面的链表
            ListNode next=end.next;
            //断开end后面的链表，保证reverse函数可以在本翻转段触底null
            end.next=null;
            //记录链表段翻转前的第一个节点，这个节点将是链表段翻转后的最后一个节点
            ListNode cur=prev.next;
            //翻转prev后面到end的链表段
            prev.next=reverse(cur);
            //翻转完毕的链表段的末尾重新接上整体的大链表
            cur.next=next;

            //移动pre和end，准备下一链表段的翻转
            prev=cur;
            end=prev;
        }

        //跳出while的时候说明,已经处理完了，返回
        return dummyHead.next;
    }

    //翻转以head为头的链表片段（链表末尾截断，所以是null）
    public ListNode reverse(ListNode head){
        ListNode prev=null;
        ListNode cur=head;

        while(cur!=null){
            //因为是单向链表，cur的指向变了后，就会遗失next，所以要先把next保存下来
            ListNode next=cur.next;
            //倒转cur和prev的关系
            cur.next=prev;
            //移动pre和cur
            prev=cur;
            cur=next;
        }

        //返回链表段翻转后的头节点。此时prev是头节点，cur是空（cur为空才能跳出while循环）
        return prev;
    }
}
```

### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

这个题应该是和“剑指23”相同，但是leetcode剑指上没有23题

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        /**
        使用快慢指针
         */

        //保证链表至少有两个节点才做判断环的处理
        if(head==null||head.next==null)return false;

        //初始化快慢指针，如果前面的快指针能和后面的慢指针重合，那么说明有环
        ListNode fast=head.next;
        ListNode slow=head;
        while(fast!=null&&slow!=null){
            if(fast==slow)return true;//追赶上的话
            
            //fast移动时要防止空指针异常
            if(fast.next==null)return false;//表示fast已经来到链表末尾

            fast=fast.next.next;//上一个if保证这里的fast.next不为空即不会发生空指针异常
            slow=slow.next;
        }

        //fast或slow能到链表结尾，说明没有环，返回false
        return false;
        
    }
}
```

#### 官方-hash表

思路及算法：

最容易想到的方法是遍历所有节点，每次遍历到一个节点时，判断该节点此前是否被访问过。

具体地，我们可以使用哈希表来存储所有已经访问过的节点。每次我们到达一个节点，如果该节点已经存在于哈希表中，则说明该链表是环形链表，否则就将该节点加入哈希表中。重复这一过程，直到我们遍历完整个链表即可。

代码：

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<ListNode>();
        while (head != null) {
            if (!seen.add(head)) {
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

- 我：这里的hash表法，其实用的hashset；但是hashset的底层是hashmap；hashmap可以在o1的时间复杂度下判断两个对象（这里是key）是否相等。
- 我：我觉得可以用`hashset.contains(obj)`来判断obj是否在hashset中；示例代码通过add的方式确实不容易记住用法，因为我记得有些add就不会返回值。

复杂度分析:

![image-20220515153137428](lcof.assets/image-20220515153137428.png)

#### 官方2-快慢指针

思路及算法：

本方法需要读者对「Floyd 判圈算法」（又称龟兔赛跑算法）有所了解。

假想「乌龟」和「兔子」在链表上移动，「兔子」跑得快，「乌龟」跑得慢。当「乌龟」和「兔子」从链表上的同一个节点开始移动时，如果该链表中没有环，那么「兔子」将一直处于「乌龟」的前方；如果该链表中有环，那么「兔子」会先于「乌龟」进入环，并且一直在环内移动。等到「乌龟」进入环时，由于「兔子」的速度快，它一定会在某个时刻与乌龟相遇，即套了「乌龟」若干圈。

我们可以根据上述思路来解决本题。具体地，我们定义两个指针，一快一满。慢指针每次只移动一步，而快指针每次移动两步。初始时，慢指针在位置 head，而快指针在位置 head.next。这样一来，如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。否则快指针将到达链表尾部，该链表不为环形链表。

细节：

![image-20220515153724894](lcof.assets/image-20220515153724894.png)

代码：

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

- 我：示例这种循环判断的写法比我首战的写法更合理。

复杂度分析：

![image-20220515153802000](lcof.assets/image-20220515153802000.png)

### [23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        /**
        如果列表包含的链表个数小于1，那么就不需要在列表内部进行链表合并。
        
        返回的[]是null吗
        - 我：应该是直接new ListNode生成的没有val的节点
         */
        if(lists.length==0)return null;
        else if(lists.length==1)return lists[0];

        //走到这步说明lists中至少有两条链表，可以进行合并；我们指定one为第i-1条链表，two为第i条链表；
        ListNode one=lists[0],two=lists[1];
        //不断合并链表
        for(int i=1;i<lists.length;i++){
            //合并one two后更新one two。
            one=merged(one,two);
            if(i<lists.length-1)two=lists[i+1];//更新two的时候要防止超过数组边界
        }

        //返回one，one是最后合并后的链表
        return one;
    }

    public ListNode merged(ListNode one,ListNode two){
        ListNode resHead=new ListNode(); 
        ListNode resCur=resHead,oneCur=one,twoCur=two;
        //两个链表都不为空时，把小的那个赋值给resCur；并移动指针
        while(oneCur!=null&&twoCur!=null){
            if(oneCur.val<twoCur.val){
                resCur.next=oneCur;
                oneCur=oneCur.next;
            }else{
                resCur.next=twoCur;
                twoCur=twoCur.next;
            }
            resCur=resCur.next;
        }

        //走到这里，onecur和twocur至少有一个是空，把不为空的列表直接放到rescur后面
        if(oneCur!=null){
            resCur.next=oneCur;
        }else{
            resCur.next=twoCur;//就算twocur是null，这么赋值也是没问题的
        }

        return resHead.next;
    }
}
```

- 我这种做法很朴素，是官方的方法一；不同链表间做了大量重复比较，可以用分治法或者优先队列法去减少重复比较

#### 官方方法一-顺序合并

思路：

- 我们可以想到一种最朴素的方法:用一个变量ans来维护以及合并的链表，第i次循环把第i个链表和ans合并，答案保存到ans 中。

代码：

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for (int i = 0; i < lists.length; ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}
```

复杂度分析:

![image-20230119204548944](lcof.assets/image-20230119204548944.png)

#### 官方方法二-分治合并

思路：

![image-20230119204656705](lcof.assets/image-20230119204656705.png)

代码：

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists, 0, lists.length - 1);
    }

    public ListNode merge(ListNode[] lists, int l, int r) {
        if (l == r) {
            return lists[l];
        }
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
}
```

- 我：这个“分治的合并链表”和归并排序好像

复杂度分析：

![image-20230119205006336](lcof.assets/image-20230119205006336.png)

#### 官方方法三-使用优先队列合并

思路：

- 这个方法和前两种方法的思路有所不同，我们需要维护当前每个链表没有被合并的元素的最前面一个，k个链表就最多有k个满足这样条件的元素，每次在这些元素里面选取val属性最小的元素合并到答案中。在选取最小元素的时候，我们可以用优先队列来优化这个过程。

代码（不用官方的，用高赞的大佬代码）：

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {

        if (lists.length == 0) {
            return null;
        }

        ListNode dummyHead = new ListNode(0);//创建结果链表
        ListNode curr = dummyHead;
        PriorityQueue<ListNode> pq = new PriorityQueue<>(new Comparator<ListNode>() {//我：创建小顶堆
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val - o2.val;
            }
        });

        for (ListNode list : lists) {//我：每个链表都把首结点放入PQ
            if (list == null) {
                continue;
            }
            pq.add(list);
        }

        while (!pq.isEmpty()) {
            ListNode nextNode = pq.poll();//我：取出最小的节点
            curr.next = nextNode;//我：把堆中最小的节点放入结果中
            curr = curr.next;
            if (nextNode.next != null) {//我：从PQ取出的节点如果有后续节点，就把后续节点补充进PQ
                pq.add(nextNode.next);
            }
        }
        //我：走到这就说明PQ为空即所有链表都处理完了；返回结果即可
        return dummyHead.next;
    }
}
```

- 我：这个代码时间复杂度和分治一样，但是用了更多的空间复杂度；胜在思想简单
- 网友：建议用lambda表达式声明优先队列，不然面试的时候能把自己写死。
  - 我：这里注意复习一下PQ声明排序的方式，我个人也推荐使用lamda表达式。小顶堆即默认的升序，使用lamda表达式为`PriorityQueue<ListNode> pq = new PriorityQueue<>((o1,o2)->(o1.val-o2.val));`，已测验通过。
    - 注意lamda表达式：两个小括号夹一个箭头，右边小括号没有return，表达式传入构造函数
    - 注意PQ的构造函数：这里`<>`中不需要声明o1和o2的类型，因为等号左边的引用已经声明了。
    - 我：Comparator的compare(o1,o2)方法返回值大于0（为true）时，交换o1和o2；保证“o1 o2”从左往右是递增的

复杂度分析：

![image-20230119205626245](lcof.assets/image-20230119205626245.png)

### [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

codetop==153

写之前的我：感觉这个是“翻转链表”的复杂版，“k个一组翻转链表“的简单版

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        /**
        思路类似k个一组翻转链表，把right断开，翻转后连上
         */

        //处理特殊情况
        if(head==null||head.next==null||left==right) return head;

         //给链表装上伪头结点，这样可以轻松让指针停在left切点的左边
         ListNode dummyHead=new ListNode();
         dummyHead.next=head;
         //左指针移动到左切点前一位
         ListNode leftLeft=dummyHead;
         while(--left>0)leftLeft=leftLeft.next;
         //右指针移动到右切点
         ListNode rightNode=dummyHead;
         while(right-->0)rightNode=rightNode.next;
         //保存右切点的右边
         ListNode rightRight=rightNode.next;
         //断开右切点的后面
         rightNode.next=null;
         //保存leftNode，因为翻转后的leftNode要连接上原链表的右边
         ListNode leftNode=leftLeft.next;
         //翻转目标链表段，并连接到原链表的左边
         leftLeft.next=reverse(leftNode);
         //把翻转后的链表和原链表的右边连起来
         leftNode.next=rightRight;

         return dummyHead.next;


    }

    public ListNode reverse(ListNode head){
        //处理特殊情况
        if(head==null||head.next==null) return head;

        //使用标准的三个指针翻转链表
        ListNode prev=null,cur=head,next=head;
        //翻转结束后，prev停在最后一个节点，cur和next为null
        while(cur!=null){
            //先标记下一个节点
            next=cur.next;
            //调转当前节点的指向
            cur.next=prev;
            //往后移动prev和cur，准备下一轮循环
            prev=cur;
            cur=next;
        }

        //翻转结束后，传入的head变成了最后的节点；我们返回prev作为逆转了的链表的头结点
        return prev;
    }
}
```

- 我：用的方法应该就是官方的方法一；要遍历两次链表。

#### 方法一：穿针引线

前言：

- 链表的操作问题，一般而言面试（机试）的时候不允许我们修改节点的值，而只能修改节点的指向操作。

- 思路通常都不难，写对链表问题的技巧是：一定要先想清楚思路，并且必要的时候在草稿纸上画图，理清「穿针引线」的先后步骤，然后再编码。

思想：

![image-20230125162455941](lcof.assets/image-20230125162455941.png)

算法步骤：

![image-20230125162600280](lcof.assets/image-20230125162600280.png)

代码：

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // 因为头节点有可能发生变化，使用虚拟头节点可以避免复杂的分类讨论
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = head;

        ListNode pre = dummyNode;
        // 第 1 步：从虚拟头节点走 left - 1 步，来到 left 节点的前一个节点
        // 建议写在 for 循环里，语义清晰
        for (int i = 0; i < left - 1; i++) {
            pre = pre.next;
        }

        // 第 2 步：从 pre 再走 right - left + 1 步，来到 right 节点
        ListNode rightNode = pre;
        for (int i = 0; i < right - left + 1; i++) {
            rightNode = rightNode.next;
        }

        // 第 3 步：切断出一个子链表（截取链表）
        ListNode leftNode = pre.next;
        ListNode curr = rightNode.next;

        // 注意：切断链接
        pre.next = null;
        rightNode.next = null;

        // 第 4 步：同第 206 题，反转链表的子区间
        reverseLinkedList(leftNode);

        // 第 5 步：接回到原来的链表中
        pre.next = rightNode;
        leftNode.next = curr;
        return dummyNode.next;
    }

    private void reverseLinkedList(ListNode head) {
        // 也可以使用递归反转一个链表
        ListNode pre = null;
        ListNode cur = head;

        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
    }
}
```

复杂度分析：

![image-20230125162845083](lcof.assets/image-20230125162845083.png)

#### 方法二：一次遍历「穿针引线」反转链表（头插法）

方法一的缺点：

![image-20230125163151858](lcof.assets/image-20230125163151858.png)

改进的思想：

![image-20230125163535620](lcof.assets/image-20230125163535620.png)



代码：

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // 设置 dummyNode 是这一类问题的一般做法
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = head;
        ListNode pre = dummyNode;
        for (int i = 0; i < left - 1; i++) {
            pre = pre.next;
        }
        ListNode cur = pre.next;
        ListNode next;
        for (int i = 0; i < right - left; i++) {
            next = cur.next;
            cur.next = next.next;
            next.next = pre.next;
            pre.next = next;
        }
        return dummyNode.next;
    }
}

```

复杂度分析：

![image-20230125163625922](lcof.assets/image-20230125163625922.png)

### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

#### 首战告捷-hash法

代码：

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        /**
        快慢指针可以知道有没有环，但是暂时想不到怎么用快慢指针定位入环点。本题就用简单的hash来做。,
         */

        //处理特殊情况
        if(head==null) return null;
        
        //题目只要求返回节点，所以用set存储就可以
        HashSet <ListNode> set= new HashSet<>();

        //遍历每个节点
        ListNode cur=head;
        while(cur!=null){
            //第一次碰到之前存过的节点，就代表是入环的位置
            if(set.contains(cur)){
                return cur;
            }

            //如果当前节点没被存过，就放到set中
            set.add(cur);

            //记住更新指针，否则死循环
            cur=cur.next;
        }

        //如果退出循环，就说明没有成环
        return null;
        
    }
}
```

hash法的性能不算好：

![image-20230127211807333](lcof.assets/image-20230127211807333.png)

#### [大佬-双指针法](https://leetcode.cn/problems/linked-list-cycle-ii/solution/linked-list-cycle-ii-kuai-man-zhi-zhen-shuang-zhi-/)

解题思路：

- 这类链表题目一般都是使用双指针法解决的，例如寻找距离尾部第 `K` 个节点、寻找环入口、寻找公共尾部入口等。

算法流程：

![image-20230127212007869](lcof.assets/image-20230127212007869.png)

- 网友：

  ```
  概括一下：
  
  根据：
  
  f=2s （快指针每次2步，路程刚好2倍）
  
  f = s + nb (相遇时，刚好多走了n圈）
  
  推出：s = nb
  
  从head结点走到入环点需要走 ： a + nb， 而slow已经走了nb，那么slow再走a步就是入环点了。
  
  如何知道slow刚好走了a步？ 从head开始，和slow指针一起走，相遇时刚好就是a步
  ```

- 网友：我觉得讲的很清楚了 get到的关键点是：

  - s走a+nb步一定是在环入口
  - f和s第一次相遇时慢指针已经走了nb步

- 我：看解析还不懂的可以看原回答，有图片流程图。

复杂度分析：

![image-20230127212056541](lcof.assets/image-20230127212056541.png)

代码：

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (true) {
            if (fast == null || fast.next == null) return null;
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) break;
        }
        fast = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
}
```

- 我：第二轮相遇时，fast速度和slow一样，还叫fast感觉就不合适了，不如新取一个名字slow2。



### [143. 重排链表](https://leetcode.cn/problems/reorder-list/)

#### 首战思路

我：时间有限，只写思路：

- 先切半，然后把后一半逆序，再把后一半一个个插入前一半
  - 官方的思路就是有这个！

#### 官方-存储

思想：

- 链表的缺点就是不能随机存储，当我们想取末尾元素的时候，只能从头遍历一遍，很耗费时间。第二次取末尾元素的时候，又得遍历一遍。

- 所以先来个简单粗暴的想法，把链表存储到线性表中，然后用双指针依次从头尾取元素即可。

代码：

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        //存到 list 中去
        List<ListNode> list = new ArrayList<>();
        while (head != null) {
            list.add(head);
            head = head.next;
        }
        //头尾指针依次取元素
        int i = 0, j = list.size() - 1;
        while (i < j) {
            //我：while体内就是把后部节点插入前部。以把B插入A->C，形成A->B->C为例。
            list.get(i).next = list.get(j);//我：这是为了实现A->B
            i++;
            //偶数个节点的情况，会提前相遇。相遇的话就不用实现B->C了,因为C是B本身，可以直接结束循环
            // if (i == j) {//我：但是保留这个代码对结果无影响，亲测注释掉这块也能提交成功；所以这句if是多此一举。
            //     break;
            // }
            list.get(j).next = list.get(i);//我：这是为了实现B->C
            j--;
        }
        list.get(i).next = null;//让链表最后一个元素指向null。这个不能放到break上一行的位置，因为不管奇数还是偶数个节点都要设置为null
    }
}
```

- 我

复杂度分析：

- 时间复杂度：O(N)，其中 N 是链表中的节点数。
- 空间复杂度：O(N)，其中 N 是链表中的节点数。主要为线性表的开销。

#### 官方-方法二：寻找链表中点 + 链表逆序 + 合并链表

> 思路：

注意到目标链表即为将原链表的左半端和反转后的右半端合并后的结果。

这样我们的任务即可划分为三步：

1. 找到原链表的中点（参考「876. 链表的中间结点」）。
   - 我们可以使用快慢指针来 O(N) 地找到链表的中间节点。
2. 将原链表的右半端反转（参考「206. 反转链表」）。
   - 我们可以使用迭代法实现链表的反转。
3. 将原链表的两端合并。
   - 因为两链表长度相差不超过 1，因此直接合并即可

> 代码：

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        ListNode mid = middleNode(head);
        ListNode l1 = head;
        ListNode l2 = mid.next;
        mid.next = null;
        l2 = reverseList(l2);
        mergeList(l1, l2);
    }

    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    public void mergeList(ListNode l1, ListNode l2) {
        ListNode l1_tmp;
        ListNode l2_tmp;
        while (l1 != null && l2 != null) {
            l1_tmp = l1.next;
            l2_tmp = l2.next;

            l1.next = l2;
            l1 = l1_tmp;

            l2.next = l1;
            l2 = l2_tmp;
        }
    }
}
```

- 我：这就是我首战的思路

复杂度分析

- 时间复杂度：*O*(*N*)，其中 *N* 是链表中的节点数。
- 空间复杂度：*O*(1)。

#### [大佬-递归法](https://leetcode.cn/problems/reorder-list/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-34/)

理解不是很容易，可以点击小标题链接查看

- 我：个人觉得现场写，更能写出官方方法二的思路，也是我的直觉思路。

#### 即时再战-半寄

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reorderList(ListNode head) {
        /**
        根据题意可知，重新排序后的链表可以如下得到
        1，链表切半。如果是奇数个节点，那么前半链表比后半多一个节点
        2，后半的链表逆序
        3，把逆序后的后半链表，逐个插入到前半链表中
         */
        
        //切半链表需要快慢指针来实现，快慢指针要保证链表至少有两个节点；所以这里先处理节点过短的特殊情况，注意题目说链表长度至少为1所以不需要考虑null
        if(head.next==null||head.next.next==null) return;//可以直接返回
        //能走到这说明链表至少有三个节点，有拆分需求

        //快慢指针切半链表=====================================
        ListNode f=head;
        ListNode s=head;
        /**
        1，f.next!=null是针对奇数个节点的链表的退出循环；f.next.next!=null是针对偶数个节点的链表的退出循环。这样可以保证退出循环是，s停留在链表中点前。
        2，！！！注意点：
        - 不要漏了先判断“f.next!=null”，否则链表数目是奇数时，要跳出循环时，f.next.next!=null会触发空指针异常
        - while中是用&&连接两个条件。必须满足严苛条件f.next.next!=null，这样s才能保证停在链表中点之前；如果允许f.next.next==null，那么偶数个节点的链表退出循环时，s在链表中点的后一位
         */
        while(f.next!=null&&f.next.next!=null){
            //System.out.println(f.val);//提交时要关闭调试，不然代码很慢。
            s=s.next;
            f=f.next.next;
        }
        //从s指针后面切开
        ListNode former=head;
        ListNode latter=s.next;
        s.next=null;

        //倒序latter===========================================
        ListNode prev=null,cur=latter,next;
        while(cur!=null){
            //记录断开前的next
            next=cur.next;
            //cur指向倒转
            cur.next=prev;//这样正好原首节点会指向null，称为合格的现为尾节点
            //后移prev和cur
            prev=cur;
            cur=next;
        }
        //最后prev停在的节点就是倒序后的头节点
        latter=prev;

        //把倒序后的latter插入former
        ListNode latterCur=latter,latterNext,formerCur=former;
        while(latterCur!=null){
            //保存lattercur的后一节点
            latterNext=latterCur.next;
            //让lattercur指向formercur的下一节点；相当于lattercur和formercur有相同的下一节点
            latterCur.next=formerCur.next;
            //连接上formercur和latterCur，就算缝合好了
            formerCur.next=latterCur;

            //移动指针
            formerCur=formerCur.next.next;
            latterCur=latterNext;
        }

        //把head指向重排序后的链表头部，这样oj就可以看head是否是正确的排序结果
        //head=former;//former本来就指向head，不需要这一步
        return;

    }
}
```

- 我：没有一次性跑通，做了一些debug跑通的。留心代码注释里写了“注意”的地方。
- 我：真实考试时，不能用sout来打印debug，我这种分了三步处理链表的方式，可能比较难判断错在哪。不过这种做法的时间和空间复杂度都是比较好的，时间是on，空间是o1。

### [148. 排序链表](https://leetcode.cn/problems/sort-list/)

#### 首战告捷-归并

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        /**
        因为单链表限制了元素遍历的方向，用快排会比较麻烦，而用堆排序就比较简单。
         */
        
        return mergeSort(head);


    }

    //返回排序后链表的头结点
    public ListNode mergeSort(ListNode head){
        //设置触底条件：至少有两个节点才会往下处理========================
        if(head==null||head.next==null) return head;

        //把元素切半。可以使用快慢指针=================================
        ListNode s=head,f=head;
        while(f.next!=null&&f.next.next!=null){//f.next!=null让奇数个节点的链表停止；f.next.next!=null让偶数个节点的链表停止
            s=s.next;
            f=f.next.next;
        }
        //偶数个节点时，s停在前半段的最后一个节点；奇数个节点时，s停在中间节点。我们让s和后面断开形成两个链表
        ListNode left=head,right=s.next;
        s.next=null;


        //让左右两半分别有序================================
        ListNode mergedLeft=mergeSort(left);
        ListNode mergedRight=mergeSort(right);

        //合并左右两半为一个更大的有序列表===================
        return merge(mergedLeft,mergedRight);
    }

    //返回合并两个有序链表后，得到的有序链表的头结点
    public ListNode merge(ListNode left,ListNode right){
        ListNode dummyHead=new ListNode();
        //合并两个有序链表
        ListNode prev=dummyHead;
        while(left!=null&&right!=null){
            if(left.val<right.val){
                prev.next=left;
                left=left.next;
                prev=prev.next;
            }else{
                prev.next=right;
                right=right.next;
                prev=prev.next;
            }
        }

        //把剩下的left或right直接拼接到结果链表的尾部
        if(left!=null) prev.next=left;
        if(right!=null) prev.next=right;

        return dummyHead.next;
    }
}
```

- 我：就用归并的思想，效率蛮不错：

  ![image-20230210132009549](lcof.assets/image-20230210132009549.png)

#### 大佬1-递归版归并

思路：

题目要求时间空间复杂度分别为 O(nlogn)O(nlogn) 和 O(1)O(1)，根据时间复杂度我们自然想到二分法，从而联想到归并排序；

对数组做归并排序的空间复杂度为 O(n)O(n)，分别由新开辟数组 O(n)O(n) 和递归函数调用 O(logn)O(logn) 组成，而根据链表特性：

- 数组额外空间：链表可以通过修改引用来更改节点顺序，无需像数组一样开辟额外空间；
- 递归额外空间：递归调用函数将带来 O(logn)O(logn) 的空间复杂度，因此若希望达到 O(1)O(1) 空间复杂度，则不能使用递归。

通过递归实现链表归并排序，有以下两个环节：

- 分割 cut 环节： 找到当前链表 中点，并从 中点 将链表断开（以便在下次递归 cut 时，链表片段拥有正确边界）；
  - 我们使用 fast,slow 快慢双指针法，奇数个节点找到中点，偶数个节点找到中心左边的节点。
  - 找到中点 slow 后，执行 slow.next = None 将链表切断。
  - 递归分割时，输入当前链表左端点 head 和中心节点 slow 的下一个节点 tmp(因为链表是从 slow 切断的)。
  - cut 递归终止条件： 当 head.next == None 时，说明只有一个节点了，直接返回此节点。
- 合并 merge 环节： 将两个排序链表合并，转化为一个排序链表。
  - 双指针法合并，建立辅助 ListNode h 作为头部。
  - 设置两指针 left, right 分别指向两链表头部，比较两指针处节点值大小，由小到大加入合并链表头部，指针交替前进，直至添加完两个链表。
  - 返回辅助ListNode h 作为头部的下个节点 h.next。
  - 时间复杂度 O(l + r)，l, r 分别代表两个链表长度。
- 当题目输入的 head == None 时，直接返回 None。

![Picture2.png](lcof.assets/8c47e58b6247676f3ef14e617a4686bc258cc573e36fcf67c1b0712fa7ed1699-Picture2.png)

代码：

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode fast = head.next, slow = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode tmp = slow.next;
        slow.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(tmp);
        ListNode h = new ListNode(0);
        ListNode res = h;
        while (left != null && right != null) {
            if (left.val < right.val) {
                h.next = left;
                left = left.next;
            } else {
                h.next = right;
                right = right.next;
            }
            h = h.next;
        }
        h.next = left != null ? left : right;
        return res.next;
    }
}

```

- 我：感觉代码可读性还不如我的“首战”

#### 大佬2-迭代版归并

对于非递归的归并排序，需要使用迭代的方式替换 cut 环节：

- 我们知道，cut 环节本质上是通过二分法得到链表最小节点单元，再通过多轮合并得到排序结果。
- 每一轮合并 merge 操作针对的单元都有固定长度 intv，例如：
  - 第一轮合并时 intv = 1，即将整个链表切分为多个长度为 1 的单元，并按顺序两两排序合并，合并完成的已排序单元长度为 2。
  - 第二轮合并时 intv = 2，即将整个链表切分为多个长度为 2 的单元，并按顺序两两排序合并，合并完成已排序单元长度为 4。
  - 以此类推，直到单元长度 intv >= 链表长度，代表已经排序完成。
- 根据以上推论，我们可以仅根据 intv 计算每个单元边界，并完成链表的每轮排序合并，例如:
  - 当 intv = 1 时，将链表第 1 和第 2 节点排序合并，第 3 和第 4 节点排序合并，……。
  - 当 intv = 2 时，将链表第 1-2 和第 3-4 节点排序合并，第 5-6 和第 7-8 节点排序合并，……。
  - 当 intv = 4 时，将链表第 1-4 和第 5-8 节点排序合并，第 9-12 和第 13-16 节点排序合并，……。
- 此方法时间复杂度 O(nlogn)O(nlogn)，空间复杂度 O(1)O(1)。

![Picture1.png](lcof.assets/c1d5347aa56648afdec22372ee0ed13cf4c25347bd2bb9727b09327ce04360c2-Picture1.png)

模拟上述的多轮排序合并：

- 统计链表长度 length，用于通过判断 intv < length 判定是否完成排序；
- 额外声明一个节点 res，作为头部后面接整个链表，用于：
  - intv *= 2 即切换到下一轮合并时，可通过 res.next 找到链表头部 h；
  - 执行排序合并时，需要一个辅助节点作为头部，而 res 则作为链表头部排序合并时的辅助头部 pre；后面的合并排序可以将上次合并排序的尾部 tail 用做辅助节点。
- 在每轮 intv 下的合并流程：
  1. 根据 intv 找到合并单元 1 和单元 2 的头部 h1, h2。由于链表长度可能不是 2^n，需要考虑边界条件：
     - 在找 h2 过程中，如果链表剩余元素个数少于 intv，则无需合并环节，直接 break，执行下一轮合并；
     - 若 h2 存在，但以 h2 为头部的剩余元素个数少于 intv，也执行合并环节，h2 单元的长度为 c2 = intv - i。
  2. 合并长度为 c1, c2 的 h1, h2 链表，其中：
     - 合并完后，需要修改新的合并单元的尾部 pre 指针指向下一个合并单元头部 h。（在寻找 h1, h2 环节中，h指针已经被移动到下一个单元头部）
     - 合并单元尾部同时也作为下次合并的辅助头部 pre。
  3. 当 h == None，代表此轮 intv 合并完成，跳出。
- 每轮合并完成后将单元长度 ×2，切换到下轮合并：intv *= 2。

代码：

```java
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        h, length, intv = head, 0, 1
        while h: h, length = h.next, length + 1
        res = ListNode(0)
        res.next = head
        # merge the list in different intv.
        while intv < length:
            pre, h = res, res.next
            while h:
                # get the two merge head `h1`, `h2`
                h1, i = h, intv
                while i and h: h, i = h.next, i - 1
                if i: break # no need to merge because the `h2` is None.
                h2, i = h, intv
                while i and h: h, i = h.next, i - 1
                c1, c2 = intv, intv - i # the `c2`: length of `h2` can be small than the `intv`.
                # merge the `h1` and `h2`.
                while c1 and c2:
                    if h1.val < h2.val: pre.next, h1, c1 = h1, h1.next, c1 - 1
                    else: pre.next, h2, c2 = h2, h2.next, c2 - 1
                    pre = pre.next
                pre.next = h1 if c1 else h2
                while c1 > 0 or c2 > 0: pre, c1, c2 = pre.next, c1 - 1, c2 - 1
                pre.next = h 
            intv *= 2
        return res.next


```

- 我：这种方法空间性能略好，但是可读性太差了。我还是用自己首战的方法吧。

#### 再战-快排

下次再算这道题，用同向指针的快排法来写。



### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        /**
        1，建立伪头节点，从而保存头结点，并可以删除一个节点的情况；
        2，双指针定位倒数，让f到末尾节点时，s停在要删掉的节点前面；
        3，然后做简单的节点删除
         */
        ListNode dummyHead=new ListNode();
        dummyHead.next=head;
        ListNode s=dummyHead,f=dummyHead;
        //让s和f之间相差n
        while(n>0){
            f=f.next;
            n--;
        }

        //让f和s同步移动，f到达最后一个节点时，s在要删除节点的前一位
        while(f.next!=null){
            f=f.next;
            s=s.next;
        }

        //删除节点：
        s.next=s.next.next;

        //return head;//如果链表只有[1]，那么return head就把被删掉的节点返回了；应该返回dummyHead.next才是真正的删除节点后链表头部
        return dummyHead.next;
    }
}
```

- 我补充：倒数的思想，就要想到“双指针定位”，或者“栈后进先出”。栈需要On的时间复杂度，所以这里用双指针是最佳做法。

#### 官方-栈

代码：

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        Deque<ListNode> stack = new LinkedList<ListNode>();
        ListNode cur = dummy;
        while (cur != null) {
            stack.push(cur);
            cur = cur.next;
        }
        for (int i = 0; i < n; ++i) {
            stack.pop();
        }
        ListNode prev = stack.peek();
        prev.next = prev.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
```



复杂度分析

- 时间复杂度：O(L)，其中 L是链表的长度。
- 空间复杂度：O(L)，其中 L是链表的长度。主要为栈的开销。



#### 官方-双指针

代码：

- 和我的一模一样，略

复杂度：

- 时间复杂度：O(L)，其中 L*L* 是链表的长度。
- 空间复杂度：O(1)。

### [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        /**
        因为链表是排序的，所以重复的节点都在一起，on的时间复杂度遍历删除
         */

        //处理特殊情况
        if(head==null||head.next==null)return head;
        
        //建立伪头节点，方便删除第一个元素
        ListNode dummyHead=new ListNode();
        dummyHead.next=head;

        ListNode cur=head,prev=dummyHead;
        int gap=0;
        //on时间复杂度遍历链表
        while(cur!=null&&cur.next!=null){//之前处理了链表长度为0和1的情况，所以走到这的链表长度至少为2//!!!注意：完成删除后cur可能是null，所以外循环得先保证cur不为null；外循环保证cur不为null后内循环就不用管cur是否为null了，因为内循环自己会在curnext为空的时候停下来
            //如果cur和curnext相同的话就一直略过，相同的段会被删除
            while(cur.next!=null&&cur.val==cur.next.val){//对cur.next取val前一定要保证cur.next非空，否则会空指针异常
                cur=cur.next;
                gap++;
            }
            //走到这说明cur和curnext不同
            if(gap>0){//如果cur滑过的gap大于0，调整prev的指向从而实现“prev的下一位是cur.next”
                prev.next=cur.next;
                cur=cur.next;//!!!注意，因为(prev,cur]的节点都被删除了，所以删除后cur得后移动一位才为prevnext
                gap=0;
            }else{//如果cur没有划过任何值，说明while执行前cur和curnext不同，prev和cur都后移一位
                prev=prev.next;
                cur=cur.next;
            }
        }
        //走到这说明cur是最后一个节点了，已经不需要和curnext比较了
        return dummyHead.next;

    }
}
```

- 我：提交不是一次成功的，修改了几个bug，留意注释里的“!!!注意”
- 网友：这种题就是迭代很难，相当于中等，递归才是简单。

#### 大佬-递归

- 我：看到这道题的时候第一时间就想到迭代的方法，但是迭代的边界处理是容易出错的；大佬的这种递归方法思路简单且不容易错。不过我迭代的空间复杂度o1比递归的on好。

链表和树的问题，一般都可以有递归和迭代两种写法。对于本题一定记住是有序链表，值相同的节点会在一起。

> 1.1 递归函数定义

递归最基本的是要明白递归函数的定义！ 我反复强调过这一点。

递归函数直接使用题目给出的函数 deleteDuplicates(head) ，它的含义是 删除以 head 作为开头的有序链表中，值出现重复的节点。

> 1.2 递归终止条件

终止条件就是能想到的基本的、不用继续递归处理的case。

- 如果 head 为空，那么肯定没有值出现重复的节点，直接返回 head；
- 如果 head.next 为空，那么说明链表中只有一个节点，也没有值出现重复的节点，也直接返回 head。

> 1.3 递归调用

什么时候需要递归呢？我们想一下这两种情况：

- 如果 head.val != head.next.val ，说明头节点的值不等于下一个节点的值，所以当前的 head 节点必须保留；但是 head.next 节点要不要保留呢？我们还不知道，需要对 head.next 进行递归，即对 head.next 作为头节点的链表，去除值重复的节点。所以 head.next = self.deleteDuplicates(head.next).
- 如果 head.val == head.next.val ，说明头节点的值等于下一个节点的值，所以当前的 head 节点必须删除，并且 head 之后所有与 head.val 相等的节点也都需要删除；删除到哪个节点为止呢？需要用 move 指针一直向后遍历寻找到与 head.val 不等的节点。此时 move 之前的节点都不保留了，因此返回 deleteDuplicates(move);

> 1.4 返回结果

题目让我们返回删除了值重复的节点后剩余的链表，结合上面两种递归调用的情况。

- 如果 head.val != head.next.val ，头结点需要保留，因此返回的是 head；
- 如果 head.val == head.next.val ，头结点需要删除，需要返回的是deleteDuplicates(move);。

> 图示

对链表 1 -> 2 -> 2 -> 3 递归的过程如下。

![82.001.jpeg](lcof.assets/1616650335-dUWxLB-82.001.jpeg)

>代码

```java
public ListNode deleteDuplicates(ListNode head) {
    // 没有节点或者只有一个节点，必然没有重复元素
    if (head == null || head.next == null) return head;

    // 当前节点和下一个节点，值不同，则head的值是需要保留的，对head.next继续递归
    if (head.val != head.next.val) {
        head.next = deleteDuplicates(head.next);
        return head;
    } else {
        // 当前节点与下一个节点的值重复了，重复的值都不能要。
        // 一直往下找，找到不重复的节点。返回对不重复节点的递归结果
        ListNode notDup = head.next.next;
        while (notDup != null && notDup.val == head.val) {
            notDup = notDup.next;
        }
        return deleteDuplicates(notDup);
    }
}
```

> 复杂度分析

- 时间复杂度：O(N)，每个节点访问了一次。
- 空间复杂度：O(N)，递归调用的时候会用到了系统的栈。

#### 大佬-迭代

- 我：和我的思路差不多

> 思路

这里说的一次遍历，是说**一边遍历、一边统计相邻节点的值是否相等，如果值相等就继续后移找到值不等的位置，然后删除值相等的这个区间**。

其实思路很简单，跟递归方法中的 **while 语句跳过所有值相等的节点**的思路是一样的：如果 cur.val == cur.next.val  说明两个相邻的节点值相等，所以继续后移，一直找到 cur.val != cur.next.val  ，此时的 cur.next  就是值不等的节点。

- 比如： 1 -> 2 -> 2 -> 2 -> 3，我们用一个 pre 指向 1；当 cur 指向第一个 2 的时候，发现 cur.val == cur.next.val  ，所以出现了值重复的节点啊，所以 cur 一直后移到最后一个 2 的时候，发现 cur.val != cur.next.val  ，此时 cur.next = 3 ，所以 pre.next = cur.next ，即让1 的 next 节点是 3，就把中间的所有 2 都删除了。

代码中用到了一个常用的技巧：**dummy 节点**，也叫做 哑节点。它在链表的迭代写法中非常常见，因为对于本题而言，我们可能会删除头结点 head，为了维护一个不变的头节点，所以我们添加了 dummy，让dummy.next = head，这样即使 head 被删了，那么会操作 dummy.next 指向新的链表头部，所以最终返回的也是 dummy.next。

> 代码

```java
class Solution {
   public ListNode deleteDuplicates(ListNode head) {
       if(head==null||head.next==null) return head;
		 //维护一个不变的结点guard
	ListNode guard=new ListNode();
	guard.next=head;
        ListNode pre=guard;
	ListNode cur=head;
	while(cur!=null){
            //如果出现重复的情况如下
			//相等的结点则跳过,走到相同值元素的最后一步
	    while(cur.next!=null&&cur.next.val==cur.val) cur=cur.next;
            //如果pre和cur之间没有重复节点，pre后移
            if(pre.next==cur)  pre=pre.next;
            //将cur的前指针向后移动一位
            else pre.next=cur.next;
			cur=cur.next;
	}
		 return guard.next;	 
    }
}
```

> 复杂度分析

- 时间复杂度：O(N)，对链表每个节点遍历了一次；
- 空间复杂度：O(1)，只使用了常量的空间。

## 字符串(简单)

### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

#### 首战告捷

```java
class Solution {
    public String replaceSpace(String s) {
        
        String result="";

        //对字符串的每个字符进行处理。字符串长度用length()，数组长度用length。size的大小包含空格
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



#### 官方1-遍历添加

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

- 我：` s.toCharArray()`可以把字符串转化为字符数组。

- 我：SB.append可以附加 char int String：

  ![image-20230222132454561](lcof.assets/image-20230222132454561.png)

#### 官方2-字符数组

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

- 获取字符串 s[n:] 切片和 s[:n] 切片，使用 "++" 运算符拼接并返回即可。

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

- 注意substring左闭右开！！



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
- StirngBuilder可以直接append字符，也可以append字符串



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

### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

#### 首战告捷

```java
class Solution {
    public boolean isValid(String s) {
        /**
        右半括号要和左半括号抵消
        
        更早出现的左半括号要和更晚出现的对应的右半括号抵消，这符合栈的后进先出的特点
         */
        
        //使用deque实现栈。从last进行存取
        Deque<Character> stack=new LinkedList<>();

        //把字符串转化为字符数组(分析字符串元素时的经典操作)
        char[] chs=s.toCharArray();
        //对字符数组处理
        for(char ch:chs){
            //如果是左半边括号，直接送入栈中
            if(ch=='('||ch=='['||ch=='{'){
                stack.offerLast(ch);
            }else{//如果是右半边括号，从栈顶拿出元素，和右半边括号对比看看匹配与否；如果匹配就继续遍历字符数组，如果不匹配就直接返回false；
                //!!!!别忽略一个条件，当输入右括号时，栈为空，则说明也不匹配。而且pollLast()返回的null会导致空指针异常；我猜测这个异常是因为尝试把null赋值给char，char不是对象，它类似int是有默认值的。
                if(stack.isEmpty())return false;

                //能走到这步说明stack不为空
                char curTop=stack.pollLast();
                if(ch==')'&&curTop!='(')return false;
                else if(ch==']'&&curTop!='[') return false;
                else if(ch=='}'&&curTop!='{') return false;
                else{
                    //当前匹配没问题，继续遍历。else什么都不做
                }

            }
        }

        //如果处理完字符数组后，stack为空说明所有左半边括号都被匹配了，那么说明是合格字符串，就返回true
        return stack.isEmpty();

    }
}
```

- 我：尝试把null赋值给char会导致编译时报错`java: 不兼容的类型: <nulltype>无法转换为char`
- 我：Deque接口本身就有push和pop，peek，可以直接做栈使用；不需要自己定义last为栈顶然后用offerlast和polllast

#### 大佬-栈（且避免判断边界）

1，我首战的写法类似官方的，但是我和官方都要判断边界条件，万一遗忘会导致错误。

2，大佬还是用了栈的思想，但是避免了额外判断边界条件：

```java
public boolean isValid(String s) {
    if(s.isEmpty())
        return true;
    Stack<Character> stack=new Stack<Character>();
    for(char c:s.toCharArray()){
        if(c=='(')
            stack.push(')');
        else if(c=='{')
            stack.push('}');
        else if(c=='[')
            stack.push(']');
        else if(stack.empty()||c!=stack.pop())//我：边界判断作为pop的前提，没有额外被处理。
            return false;
    }
    return stack.isEmpty();
}
```



### 相关题目

- 剑指 Offer 37. 序列化二叉树：解法有蛮多字符串的知识，尤其是StringBuilder的使用。



## 字符串(中等)

### [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

codetop==2

和主站[65. 有效数字](https://leetcode.cn/problems/valid-number/)相同。

#### 面试情况

我字节一面出了这原题。这题剑指和主站的频率都很低，，给我出，感觉是不想要我。

#### 首战寄

没有啥很好的思路：

- `s.trim().toCharArray();`，把字符串收尾空格去掉，然后转化成char数组处理 

- 分段递归判断

网友评价：

- 关键是题目中给的示例不详尽，提交一次，修改一次代码，修改的全是细节 做一次恶心一次
-  这题是个典型的面向测试用例编程的题目

#### 大佬-常规思路

个人感觉还是常规思路好理解= =，直接逐位遍历一遍，并做好标记。

代码：

```java
class Solution {
    public boolean isNumber(String s) {
        if(s == null || s.length() == 0) return false; // s为空对象或 s长度为0(空字符串)时, 不能表示数值
        boolean isNum = false, isDot = false, ise_or_E = false; // 标记是否遇到数位、小数点、‘e’或'E'
        char[] str = s.trim().toCharArray();  // 删除字符串头尾的空格，转为字符数组，方便遍历判断每个字符
        for(int i=0; i<str.length; i++) {
            if(str[i] >= '0' && str[i] <= '9') isNum = true; // 判断当前字符是否为 0~9 的数位
            else if(str[i] == '.') { // 遇到小数点
                if(isDot || ise_or_E) return false; // 小数点之前可以没有整数，但是不能重复出现小数点、或出现‘e’、'E'
                isDot = true; // 标记已经遇到小数点
            }
            else if(str[i] == 'e' || str[i] == 'E') { // 遇到‘e’或'E'
                if(!isNum || ise_or_E) return false; // ‘e’或'E'前面必须有整数，且前面不能重复出现‘e’或'E'
                ise_or_E = true; // 标记已经遇到‘e’或'E'
                isNum = false; // 重置isNum，因为‘e’或'E'之后也必须接上整数，防止出现 123e或者123e+的非法情况
            }
            else if(str[i] == '-' ||str[i] == '+') { 
                if(i!=0 && str[i-1] != 'e' && str[i-1] != 'E') return false; // 正负号只可能出现在第一个位置，或者出现在‘e’或'E'的后面一个位置
            }
            else return false; // 其它情况均为不合法字符
        }
        return isNum;
    }
}
```

#### 即时再战

题目不是啥好题，就不再战了。

### [剑指 Offer 67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

codetop 1

#### 首战半寄

过了1025 / 1079个测试案例，会有一些奇怪的判例出现，总是得修改代码，没考虑到哪些奇怪的情况，就不继续完善了。

字符串的题以后出现率不高的就不要太花时间了，感觉不值。。

```java
class Solution {
    public int strToInt(String str) {
        //特殊处理：如果字符串为空，直接返回0
        if(str==null)return 0;
        char[] chs=str.trim().toCharArray();
        //特殊处理：如果字符串只有空格，直接返回0
        if(chs.length==0)return 0;

        //lr夹住值
        int l=0,r=0;
        boolean alwaysZero=true;
        //只要出现不合法的情况就break，这样break时i的位置及之前的数都是合法的。由于不合法的情况比较多，抓住合法的来判断放行
        while(r<chs.length){
            if(chs[r]=='0'&&alwaysZero){//只有从头开始的连贯的0才能进入本if
                r++;
                l++;
            }else if(isInt(chs[r])){
                alwaysZero=false;
                r++;
            }else if(isSig(chs[r])&&r==0&&chs.length!=1){//chs.length!=1是为了防止'+'和'-'通过过滤
                r++;
            }else{
                break;
            }

            //特殊值处理，防止字符连long都超过
            if(r-l+1>11){//最大整数有10位，加上-+一位
                if(chs[0]=='-')return Integer.MIN_VALUE;
                else return Integer.MAX_VALUE;
            }
        }

        //System.out.println("截取到的结果为："+str.substring(0,i));
        //得到的整数范围为0~i-1，把字符数组处理成整数。
        int tens=1;
        //用long记录结果，在结果超过整数范围的时候直接返回对应的整数边界
        long result=0;
        //针对正数和负数做不同处理，避免对每个字节判断-
        if(chs[0]=='-'){
            //处理到1为止
            for(int j=r-1;j>=1;j--){
                // System.out.print("当前处理的字符为：");
                // System.out.println(chs[j]);
                long temp=Long.valueOf(String.valueOf(chs[j]))*tens;
                //System.out.println("temp为："+temp);
                result+=temp;
                if(-result<(long)Integer.MIN_VALUE)return Integer.MIN_VALUE;

                tens*=10;
            }
            return (int)-result;
        }else if(chs[0]=='+'){
            //处理到1为止
            for(int j=r-1;j>=1;j--){
                // System.out.print("当前处理的字符为：");
                // System.out.println(chs[j]);
                //System.out.println("Integer.valueOf(chs[j])："+Integer.valueOf(chs[j]));直接使用Integer.valueOf('2')会得到50，要先把字符转化为字符串处理
                long temp=Long.valueOf(String.valueOf(chs[j]))*tens;
                //System.out.println("temp为："+temp);
                result+=temp;
                if(result>(long)Integer.MAX_VALUE)return Integer.MAX_VALUE;

                tens*=10;
            }
            return (int)result;

        }else{
            //处理到0为止
            for(int j=r-1;j>=0;j--){
                System.out.print("当前处理的字符为：");
                System.out.println(chs[j]);
                //System.out.println("Integer.valueOf(chs[j])："+Integer.valueOf(chs[j]));直接使用Integer.valueOf('2')会得到50，要先把字符转化为字符串处理
                long temp=Long.valueOf(String.valueOf(chs[j]))*tens;
                System.out.println("temp为："+temp);
                result+=temp;
                if(result>(long)Integer.MAX_VALUE)return Integer.MAX_VALUE;

                tens*=10;
            }
            return (int)result;
        }

    }
    public boolean isInt(char ch){//可以用switch
        if(ch=='0'||ch=='1'||ch=='2'||ch=='3'||ch=='4'||ch=='5'||ch=='6'||ch=='7'||ch=='8'||ch=='9')return true;

        return false;
    }
    public boolean isSig(char ch){
        if(ch=='+'||ch=='-')return true;

        return false;
    }
}
```

#### 官方-数字越界处理

解题思路：

![image-20220416204636792](lcof.assets/image-20220416204636792.png)

![Picture1.png](lcof.assets/0be9098b13047fe3e07f3c4e51c612244ace01a023ed010bce43940408334f2a-Picture1.png)

数字越界处理：

![image-20220416204705051](lcof.assets/image-20220416204705051.png)

![Picture2.png](lcof.assets/d1b06a91801868af63f6e309da31bcfa01c7b6c385529fb974389a61e454cd12-Picture2.png)

复杂度分析：

![image-20220416204732055](lcof.assets/image-20220416204732055.png)

代码：

```java
class Solution {
    public int strToInt(String str) {
        char[] c = str.trim().toCharArray();
        if(c.length == 0) return 0;
        int res = 0, bndry = Integer.MAX_VALUE / 10;
        int i = 1, sign = 1;
        if(c[0] == '-') sign = -1;
        else if(c[0] != '+') i = 0;
        for(int j = i; j < c.length; j++) {
            if(c[j] < '0' || c[j] > '9') break;
            if(res > bndry || res == bndry && c[j] > '7') return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            res = res * 10 + (c[j] - '0');
        }
        return sign * res;
    }
}
```

- 我：字符相减，可以得到数字字符的差值

### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

#### 首战寄

没什么好思路

#### 官方-动态规划

思路与算法：

![image-20220522110207343](lcof.assets/image-20220522110207343.png)

代码：

```java
public class Solution {

    public String longestPalindrome(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        boolean[][] dp = new boolean[len][len];
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < len; i++) {
            dp[i][i] = true;
        }

        char[] charArray = s.toCharArray();
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= len; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些。因为就算j越界也会直接退出本轮循环
            for (int i = 0; i < len; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= len) {
                    break;
                }

                if (charArray[i] != charArray[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {//回文核心是单个字符时，j-i==2；回文核心是两个字符时，j-i==1；
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substring(begin, begin + maxLen);
    }
}
```

- 我：这里需要先知道更短的子串是否为回文，所以把按len遍历放在第一层for循环

复杂度分析：

![image-20220522110300711](lcof.assets/image-20220522110300711.png)

- 题目提示“s的最大长度为1000”，所以设计一个on2的算法是合理的。
- 我：这里是动态规划，就是一个状态可以由更小状态推测得出；想爬楼梯只需要两个变量来实现动态规划，但是这里需要On2个空间来实现动态规划。

#### 即时再战成功

```java
class Solution {
    public String longestPalindrome(String s) {
        /**
        回文的特点是，拿掉左右两侧的字符后，剩下的字符还是回文；这就是动态规划的特点：
        1. 针对不同的数组长度，以不同的字符开始，尝试判断当前字符串是否是回文
        2. 大回文串的确定依赖于小回文串的结果；小回文串的结果记录与数组中。
         */

        int len=s.length();
        
        //处理特殊情况
        if(len<=1)return s;

        //s至少有两个字符才会使用逻辑处理
        int maxLen=1;
        int begin=0;

        /**
        动态规划必须保留结果，根据小结果推出大结果；所以创建用于保存结果的数组。
        
        dp[i][j]表示字符串的[i,j]部分是不是回文

        由于i<j，所以二维数组的左下半不会被处理，会是默认的false。本函数只会处理对角线右上半。即会不断处理j比i大1，j比i大2....。
         */
        boolean[][] dp=new boolean[len][len];
        //由于动态规划依赖于触底，即依赖于最短的回文；所以显式设置最短回文[i,i]为true
        for(int i=0;i<len;i++){
            dp[i][i]=true;
        }

        //对每个回文长度做遍历。最外层遍历为len从小到大，保证了求较大len结果时总要较小len的结果做支撑。
        for(int tempLen=1;tempLen<=len;tempLen++){
            //在确定回文长度的前提下，让每个字符做起点来判断回文
            for(int i=0;i<len;i++){
                //根据i和templen可以求出j
                int j=i+tempLen-1;
                //如果j超过dp数组的边界，就直接退出对当前i的遍历
                if(j>=len)break;

                //判断dp[i][j]是否是回文字符串
                if(s.charAt(i)!=s.charAt(j)){//如果ij位置的字符不同，肯定不是回文
                    dp[i][j]=false;
                }else{
                    if(tempLen<=3){//如果ij位置字符相同，且当前子字符串长度为2或3，那么当前子字符串就是回文
                        dp[i][j]=true;
                    }else{//如果当前子字符串长度长于3，那么回文与否与[i+1,j-1]相同；同时因为子字符串长度长于3，所以dp[i+1][j-1]的时候仍在数组的右上半部(画图可看出)
                        dp[i][j]=dp[i+1][j-1];

                    }
                }

                //针对每个templen和i的组合，如果是回文的话，看看能否更新历史最大回文长度
                if(dp[i][j]&&tempLen>maxLen){
                    maxLen=tempLen;
                    begin=i;
                }
            }
        }

        //on2处理完成后，返回结果。注意substring是左闭右开的。
        return s.substring(begin,begin+maxLen);

    }
}
```

### [415. 字符串相加](https://leetcode.cn/problems/add-strings/)

#### 首战半寄

```java
class Solution {
    public String addStrings(String num1, String num2) {
        /**
        字符串的长度可以达到10000，说明最高位有1万位，超过了一般数据类型的范围；这里使用char数组手动相加。
         */
        //把两个字符串转化为字符数组
        char[] num1char=num1.toCharArray();//!!!改进：这里没必要转化成charArray，直接用charAt从字符串取值就好
        char[] num2char=num2.toCharArray();

        //从个位开始往前加，就是从数组的末尾开始往前处理
        int i=1;//i表示两个数组当前处理的是倒数第几位//！！！改进：这里用i就导致后面取字符都得用长度去减，这里应该像大佬一样取length
        //设置做加法时的进位
        int jinwei=0;
        //新建数组保存结果，长度是两个数组的长的长度那个加一；并记录结果数组的实际长度，防止预留位没被用上；用char[]而不是SB的初衷是不知道SB能否逆序，避免逆序操作。
        char[] res=new char[(num1char.length>num2char.length?num1char.length:num2char.length)+1];//!!!改进总归要用SB，这里就可以用；至于逆序问题，只需要使用SB.reverse()把顺序变成正序就好
        int resLength=0;
        while((num1char.length-i>=0)&&(num2char.length-i>=0)){//如果有一个数组被遍历完了就结束//!!!改进：我这里有三个while循环，有大量重复代码；使用大佬的方法“给超出边界的值赋值为1”，可以把三个循环合并成一个循环。
            //取出两个数组的当前位相加
            int num1curInt=num1char[num1char.length-i]-'0';//!!!字符直接相减，可以得到整数表示的字符间的差值
            int num2curInt=num2char[num2char.length-i]-'0';
            int curAdd=num1curInt+num2curInt+jinwei;
            //相加后先清空进位
            jinwei=0;
            
            //处理相加的结果
            if(curAdd>9){//更新进位，和更新要被放入结果数组的值//!!!改进：这里用除法得到十位，用求余得到个位，可以避免if判断，使代码更简洁
                jinwei=1;
                curAdd-=10;
            }
            //字符可以通过ascii直接转化为数字；数字不能直接住哪胡为字符，会用到charat，参考http://t.csdn.cn/443FM
            String curAddStr=Integer.toString(curAdd);//!!!改进：SB的好处就是可以直接append一个整数，不用像这里用三行费力地把int变为char，再把char存入char[]
            char curAddChar=curAddStr.charAt(0);
            res[res.length-i]=curAddChar;
            resLength++;
            
            //！！！一定不要忘记改变循环的关键i，否则会死循环
            i++;
        }

        //处理到这里后，有一个列表处理完了，把剩下的那个处理完毕
        while(num1char.length-i>=0){
            //相加
            int num1curInt=num1char[num1char.length-i]-'0';
            int curAdd=num1curInt+jinwei;
            //相加后先清空进位
            jinwei=0;

            //保存结果
            if(curAdd>9){//更新进位，和更新要被放入结果数组的值
                jinwei=1;
                curAdd-=10;
            }
            String curAddStr=Integer.toString(curAdd);
            char curAddChar=curAddStr.charAt(0);
            res[res.length-i]=curAddChar;
            resLength++;
            i++;
        }
        while(num2char.length-i>=0){
            //相加
            int num2curInt=num2char[num2char.length-i]-'0';
            int curAdd=num2curInt+jinwei;
            //相加后先清空进位
            jinwei=0;

            //保存结果
            if(curAdd>9){//更新进位，和更新要被放入结果数组的值
                jinwei=1;
                curAdd-=10;
            }
            String curAddStr=Integer.toString(curAdd);
            char curAddChar=curAddStr.charAt(0);
            res[res.length-i]=curAddChar;
            resLength++;
            i++;
        }

        //如果两个列表都处理完了还有进位，就把进位放在res的第一个位置也就是唯一还空着的位置
        if(jinwei!=0){
            res[0]='1';
        }

        //把字符数组转化为字符串
        StringBuilder sb=new StringBuilder();
        for(char curCharOfRes:res){
            sb.append(curCharOfRes);
        }
        String resString=sb.toString();
        return jinwei==0?resString.substring(1,res.length):resString;//如果最后没有进位就截断第一位
    }
}
```

- 我：本题独立按照思路写出来了，但是while中忘记更新i，导致死循环超时无法运行；后面借助idea来debug，修正后成功提交。
- 我：代码里有“改进”字样的，就是我根据大佬代码改进的

#### 官方

算法流程：

![image-20230125202857103](lcof.assets/image-20230125202857103.png)

复杂度分析：

![image-20230125202917858](lcof.assets/image-20230125202917858.png)

代码：

```java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder res = new StringBuilder("");
        int i = num1.length() - 1, j = num2.length() - 1, carry = 0;
        while(i >= 0 || j >= 0){
            int n1 = i >= 0 ? num1.charAt(i) - '0' : 0;
            int n2 = j >= 0 ? num2.charAt(j) - '0' : 0;
            int tmp = n1 + n2 + carry;
            carry = tmp / 10;
            res.append(tmp % 10);
            i--; j--;
        }
        if(carry == 1) res.append(1);
        return res.reverse().toString();
    }
}
```

- 我：思路和我的首战是基本一致的；但是代码量比我的少很多，写法值得学习。
  - 我：我阅读大佬代码后，在自己的首战代码中增加了批注，表示和大佬差距在哪。

## 查找算法(简单)

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
  - int的取值范围为： -2^31——2^31-1，即-2147483648——2147483647，这约是2*10^9。
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
- 我：关于equals和hashcode
  - 基本数据类型的包装类构成的集合set，可以放心使用`contains()`来判断数据是否已经存在，因为equals和hashcode都已经重写了；[参考](http://t.csdn.cn/b9Pbu)
  - 如果是非基本数据类型，就注意重写equals()后要重写hashcode()。[参考](https://blog.csdn.net/violet_echo_0908/article/details/50152915)

- 我：hashset常用的是add(obj)和remove(obj)和contains(obj)；其他用法[参考](http://t.csdn.cn/RuVg0)

复杂性分析：

- 时间复杂度：O(n)。
  - 遍历数组一遍。使用哈希集合（HashSet），添加元素的时间复杂度为 O(1)，故总的时间复杂度是 O(n)。
- 空间复杂度：O(n)。不重复的每个元素都可能存入集合，因此占用 O(n)额外空间。



### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

codetop==16

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

- 可将 nums[m] = target情况合并至其他两种情况中。

- 我：这里可以参考二分查找的模板，模板的写法可以定位到最左边的可重复数字，晒微改下便能定位到最右边的可重复数字。

#### 20230126再战-二分法成功

再战前问：其实没有问题，只是有时间重写下本题的二分法，比较有意义

```java
class Solution {
    public int search(int[] nums, int target) {
        // 搜索右边界 right
        int i = 0, j = nums.length;
        while(i < j) {
            int m = (i + j) / 2;
            if(nums[m] <= target) i = m + 1;
            else j = m;
        }
        int right = i-1;//i是第一个比target大的数，i-1的位置就是最后一个不比target大的数
        //System.out.println("right:"+right);//打印会严重拖慢执行时间
        //若right超过数组边界直接返回；因为right是index-1得到的，所以要小心；right不用担心超过右边界，所以不用判断右边界
        if(right<0)return 0;
        // 若数组中无 target ，则提前返回；上一行已经判断了right >= 0，所以这里可以方形使用right，而不担心超过数组边界
        if(nums[right] != target) return 0;
        // 搜索左边界 right
        i = 0; j = nums.length;
        while(i < j) {
            int m = (i + j) / 2;
            if(nums[m] < target) i = m + 1;
            else j = m;
        }
        int left = j;//经典二分拿到的就是第一个不小于target的数，就是左边界
        //System.out.println("left:"+left);
        return right - left + 1;

    }
}
```

- 我：核心思想是，经典二分模板找的左边界；"target和nums[m]的比较"允许等于号时，找的是右边界的右边一位。

### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

codetop==5

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

- 注意：这里说了排序数组就想到二分！

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

#### 再战成功-二分法

20220428：

```java
class Solution {
    public int missingNumber(int[] nums) {
        /**
        排序数组就用二分法，使用左闭右开模板
         */
        int left=0,right=nums.length;

        while(left<right){
            int mid=left+(right-left)/2;
            if(nums[mid]==mid){//缺失值在mid右边
                left=mid+1;//加一体现左闭右开
            }else {
                right=mid;//不加一也体现了左闭右开
            }
        }

        //退出时，left和right重合在空缺数字的后一个数字,两者的坐标正好就是空缺的数字本身
        return left;
    }
}
```



## 查找算法(中等)

### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

codetop==21

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

codetop==15

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

- 我：算法都是那m和j比，而不是和i比；我觉得是因为最小值在靠近j的半段，所以选择和j比较。后面也有官方的解答。

- 我：j=j-1后，最小值还在ij之间，因为：
  - 如果j是最小值，那么原本numsm==numsj，让j--后，至少m位置有最小值
  - **j--相当于是为重复元素去重**。
- 网友点评j--：你的方法比剑指offer上的还要好！太赞了！`numbers[m] == numbers[j] `的情况一直就想不到点上。`j=j−1 只需证明每次执行此操作后，旋转点 x 仍在 [i, j]区间内即可` 这句话醍醐灌顶

正确性证明：

![image-20220327224758158](lcof.assets/image-20220327224758158.png)

补充思考： 为什么本题二分法不用 nums[m]和 nums[i]作比较？

![image-20220327224829837](lcof.assets/image-20220327224829837.png)

- 我：官方说“（此示例只有右排序数组）”，我觉得也可以理解为这个例子只有左排序数组

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
- 我：感觉剑指的查找算法，基本都是用二分解决。

#### 20230126再战-二分法成功

```java
class Solution {
    public int minArray(int[] numbers) {
        /**
        官方答案没有使用左闭右开的二分写法，我这里使用左闭右开的二分模板来实现
         */
        int i=0,j=numbers.length-1;//因为比较的是j位置的数值，所以j不能超过数组边界。虽然左开右闭的原则看似把最后一位是旋转点的情况排除了，但是循环退出时i和j都停留在最后一位，结果没错。一般二分查找的右界都是在数组外的，这里是根据题目实际情况做微调。
        while(i<j){
            int mid=i+(j-i)/2;
            if(numbers[mid]>numbers[j])i=mid+1;//mid在左半边，说明旋转点在mid和j之间；左闭右开所以mid+1才是i
            else if(numbers[mid]<numbers[j]) j=mid;//mid在右半边，说明旋转点在i和mid之间
            else j--;//numbers[mid]==numbers[j]时，无法确定mid在左边还是右边，可以缩小查找范围
        }

        //退出的时候，i==j且落在旋转点上
        return numbers[i];

    }
}
```



### [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

codetop==7

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

### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

#### 首战告捷

使用了java的快排工具，对数组进行了快速排序。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {

        Arrays.sort(nums);
        for(int i=nums.length-1;i>=0;i--){
            if(k==1){
                return nums[i];
            }
            k--;
        }

        //这个return永远不会被执行
        return -1;
    }
}
```

- 我：我这暴力还不如官方的暴力。

#### 官方1-暴力

说明：本题解于 2021 年 8 月 6 日重写。

这道题据说是面试的高频考题，同时也是基础排序算法的应用。因为篇幅的原因，没有办法把每一个知识点都介绍得很详细，大家可以自行学习相关知识点以理解题解中的内容。

本题解分为四个部分：

- 理解题意，做任何一道算法问题都需要搞清楚题目的意思，这部分是很重要的，只有搞清楚题目的意思，才能找到解决问题的算法与数据结构；
- 方法一：暴力解法，这是根据题目意思能得到的最直接的解法，时间复杂度较高；
- 方法二：通过 partition 减治，这是快速排序 partition 的应用；
- 方法三：动态求出最值元素，是「优先队列」的应用。

方法二必须把所有的数据读入内存中，方法三在数据量很大的时候，可以实现「在线算法」，不用一下子把所有数据读入内存。

![image.png](lcof.assets/97186c31a9c3a4af8654c52e5e15e1073d623a57fb4d7fc04b32cc914b906c12-image.png)

题意分析：

![image-20220508205159104](lcof.assets/image-20220508205159104.png)

暴力解法代码：

```java
import java.util.Arrays;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        Arrays.sort(nums);
        return nums[len - k];
    }
}
```

- 我：

复杂度分析：

![image-20220508205309036](lcof.assets/image-20220508205309036.png)

其实这道问题考察了「快速排序」的 `partition` 操作和数据结构「堆」的应用，方法二和方法三分别介绍了这两种方法。

#### 官方2-减而治之

（逐渐缩小问题规模）

注意：

![image-20220508205602643](lcof.assets/image-20220508205602643.png)

不随机化pivot的代码：

```java
public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        int left = 0;
        int right = len - 1;

        // 转换一下，第 k 大元素的下标是 len - k
        int target = len - k;

        while (true) {
            int index = partition(nums, left, right);
            if (index == target) {
                return nums[index];
            } else if (index < target) {
                left = index + 1;
            } else {
                right = index - 1;
            }
        }
    }

    /**
     * 对数组 nums 的子区间 [left..right] 执行 partition 操作，返回 nums[left] 排序以后应该在的位置
     * 在遍历过程中保持循环不变量的定义：
     * nums[left + 1..j] < nums[left]
     * nums(j..i) >= nums[left]
     *
     * @param nums
     * @param left
     * @param right
     * @return
     */
    public int partition(int[] nums, int left, int right) {
        int pivot = nums[left];
        int j = left;
        for (int i = left + 1; i <= right; i++) {
            if (nums[i] < pivot) {
                // j 的初值为 left，先右移，再交换，小于 pivot 的元素都被交换到前面
                j++;
                swap(nums, j, i);
            }
        }
        // 在之前遍历的过程中，满足 nums[left + 1..j] < pivot，并且 nums(j..i) >= pivot
        swap(nums, j, left);
        // 交换以后 nums[left..j - 1] < pivot, nums[j] = pivot, nums[j + 1..right] >= pivot
        return j;
    }

    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
}
```

- 注意：本题必须随机初始化 pivot 元素，否则通过时间会很慢，因为测试用例中有极端测试用例。为了应对极端测试用例，使得递归树加深，可以在循环一开始的时候，随机交换第 1个元素与它后面的任意 1 个元素的位置；

随机化pivot的代码：

```java
import java.util.Random;

public class Solution {

    private static Random random = new Random(System.currentTimeMillis());

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        int target = len - k;
        int left = 0;
        int right = len - 1;
        while (true) {
            int index = partition(nums, left, right);
            if (index < target) {
                left = index + 1;
            } else if (index > target) {
                right = index - 1;
            } else {
                return nums[index];
            }
        }
    }

    // 在区间 nums[left..right] 区间执行 partition 操作
    private int partition(int[] nums, int left, int right) {
        // 在区间随机选择一个元素作为标定点
        if (right > left) {
            int randomIndex = left + 1 + random.nextInt(right - left);
            swap(nums, left, randomIndex);
        }

        int pivot = nums[left];
        int j = left;
        for (int i = left + 1; i <= right; i++) {
            if (nums[i] < pivot) {
                j++;
                swap(nums, j, i);
            }
        }
        swap(nums, left, j);
        return j;
    }

    private void swap(int[] nums, int index1, int index2) {
        int temp = nums[index1];
        nums[index1] = nums[index2];
        nums[index2] = temp;
    }
} 
```

- 网友：很详细，感谢，补充一点：partition的pivot选择，有一个更好办法。就是选择 nums[i], nums[j], nums[(i+j)/2] 的中位数，这样比随机要靠谱一点。
  - 大佬：这种方法叫做med_of_3，也是GCC内置非稳定排序的快排的一个部分。partition的做法我也是这样做的，不过最难受的就是这样做的标准做法是把中位数换到j-1的位置，然后对i+1~j-2所有元素做"划分"，这里的做法是双指针对撞，然后把pivot送到i的位置。这个过程看上去简单，但是细节，边界，数组越界情况简直比二分查找的边界还魔鬼。

复杂度分析：

![image-20220508205914329](lcof.assets/image-20220508205914329.png)

> 说明：可能有一部分朋友看到这道题有「递归」的写法，但是本题解采用的是在 while (true) 循环中，通过 left 与 right 向中间靠拢的方式逐步缩小搜索区间，因此没有使用递归调用栈（也无须使用递归调用栈），因此空间复杂度是 O(1)。
>

#### 官方3-优先队列

优先队列的思路是很朴素的。由于找第 K 大元素，其实就是整个数组排序以后后半部分最小的那个元素。因此，我们可以维护一个有 K 个元素的最小堆：

- 如果当前堆不满，直接添加；
- 堆满的时候，如果新读到的数小于等于堆顶，肯定不是我们要找的元素，只有新遍历到的数大于堆顶的时候，才将堆顶拿出，然后放入新读到的数，进而让堆自己去调整内部结构。

说明：这里最合适的操作其实是 replace()，即直接把新读进来的元素放在堆顶，然后执行下沉（siftDown()）操作。Java 当中的 PriorityQueue 没有提供这个操作，只好先 poll() 再 offer()。

优先队列的写法就很多了，这里只例举一个有代表性的，其它的写法大同小异，没有本质差别。

代码：

```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 k 个元素的最小堆，PriorityQueue 底层是动态数组，为了防止数组扩容产生消耗，可以先指定数组的长度
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k, Comparator.comparingInt(a -> a));
        // Java 里没有 heapify ，因此我们逐个将前 k 个元素添加到 minHeap 里
        for (int i = 0; i < k; i++) {
            minHeap.offer(nums[i]);
        }

        for (int i = k; i < len; i++) {
            // 看一眼，不拿出，因为有可能没有必要替换
            Integer topElement = minHeap.peek();
            // 只要当前遍历的元素比堆顶元素大，堆顶弹出，遍历的元素进去
            if (nums[i] > topElement) {
                // Java 没有 replace()，所以得先 poll() 出来，然后再放回去
                minHeap.poll();
                minHeap.offer(nums[i]);
            }
        }
        return minHeap.peek();
    }
}
```

复杂度分析：

![image-20220508210229493](lcof.assets/image-20220508210229493.png)

#### 再战成功

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        /**
        使用快排的partition，减少排序操作

        目标是排序后的倒数第k个元素，正好用上了快排的性质，快排每次getMid都会固定一个元素的位置

        时间复杂度on
         */
        int left=0,right=nums.length-1;
        while(true){
            int mid=getMid(nums,left,right);
            if(mid<nums.length-k){
                left=mid+1;
            }else if(mid>nums.length-k){
                right=mid-1;
            }else{
                return nums[mid];
            }
        }

    }

    //交换i和j位置元素
    public void swap(int[]nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }

    //把nums在left和right之间元素排序，使得pivot左边的都小于pivot，pivot右边的都大于pivot；返回pivot
    public int getMid(int[]nums,int left,int right){
        //为了避免极端案例（升序或降序），要随机化选取位置作为pivot。创建random的方法：https://cloud.tencent.com/developer/article/1332907
        Random random=new Random();
        int randomIndex=random.nextInt(right-left+1)+left;
        swap(nums,left,randomIndex);

        //让pivot的左边小于pivot，pivot的右边大于pivot。target即为pivot处的值
        int target=nums[left];
        while(left<right){
            //注意！！！while(target<=nums[right])可能会在移动left和right的时候超过边界
            while(left<right&&target<=nums[right])right--;
            swap(nums,left,right);
            while(left<right&&target>=nums[left])left++;
            swap(nums,left,right);
        }

        //离开循环后，left==right，且left左边的数都小于left处的位置，left右边的数都大于left处的位置
        return left;

    }
}
```

- 反思：
  - 快排在while交换位置时，要记住逼近要比较taregt，还要防止left right交叉。
  - random的用法不熟练。[参考](https://cloud.tencent.com/developer/article/1332907)，`java.util.Random`下的，`random.nextInt(x)`可以获取`[0,x-1]`的整数，范围相当于是[0,x)。



### [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

#### 首战寄

没有很好的思路

#### 官方-二分查找

思路和算法：

![image-20220515182422390](lcof.assets/image-20220515182422390.png)

![fig1](lcof.assets/33_fig1.png)

需要注意的是，二分的写法有很多种，所以在判断 `target` 大小与有序部分的关系的时候可能会出现细节上的差别。

代码：

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = left + ((right - left) >> 1);
            if(target == nums[mid])return mid;
            if(nums[mid] < nums[right]){//有序部分为后半段，在后半段判断
                if(target > nums[mid] && target <= nums[right]){
                    left = mid + 1;
                }else{//target大于右边界 or target小于nums[mid]
                    right = mid - 1;
                }
            }else{//前半部分有序，在前半段判断
                if(target >= nums[left] && target < nums[mid]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }
        }
        return nums[left] == target ? left : -1;
    }
}
```

- 网友：评论区两拨人理解的都是对的，但是说的不是一个问题：一些人说的“只在有序的半边二分”指的是以mid为分界的；如果以“旋转点”来分两半的话，两半确实都是有序的！但是想解决该题，就必须在以mid为界的两半中去找单调的那一半持续二分，直至循环结束或找到target。该题的**思路其实挺简单**：始终去有序的那个范围内去找target，不在就去另一半无序的找，但是要将无序再次划分成有序然后再找...

复杂度分析：

![image-20220515182646056](lcof.assets/image-20220515182646056.png)

#### 即时再战成功

```java
class Solution {
    public int search(int[] nums, int target) {
        /**
        使用双指针，用left和right确定mid，mid的一边是有序的。
        1.如果target在mid的有序的一边的话，在有序这边二分
        2.如果target不在mid有序的这一边的话，说明target在mid两侧的无序的那边，便在那边继续二分
         */
        
        //定义双指针。使用左闭右开模板，所以right为nums.lengt而不减少1
        int left=0,right=nums.length;

        //迭代查找
        while(left<right){
            //计算mid，注意要防止整数范围溢出
            int mid=left+(right-left)/2;//！！！注意left+(right-left)/2的mid可以和left重合；right-(right-left)/2的mid可以和right重合；一般左闭右开的写法都是允许mid和left重合

            //先查看mid的位置是不是target，是的话直接返回index
            if(nums[mid]==target)return mid;
            
            //如果有序区在mid的左边
            if(nums[mid]>=nums[left]){//!!!注意一定要等于，因为[0,1)时，left==mid==0，right==1，所以nums[mid]的值应该允许等于nums[left]。//！！！230126更新：我把判断中的"="去掉后程序还是能通过，说明对于只剩下一个元素时，把这个元素归为在左边有序区或在右边有序区都可以
                //如果target在有序区中，往有序区中缩小查找范围
                if(nums[left]<=target&&target<nums[mid]){//因为之前判断过nums[mid]不等于target，所以这里target小于nums[mid]即可
                    right=mid;//因为mid已经在之前确定不是target的位置，所以让right==mid，体现了左闭右开
                }else{//如果target不在有序区，那么在无序区中缩小查找对象
                    left=mid+1;//因为mid已经在之前确定不是target的位置，所以让left==mid+1，体现了左闭右开
                }

            }else{//如果有序区在mid的右边
                //如果target在有序区中，往有序区中缩小查找范围
                if(nums[right-1]>=target&&target>nums[mid]){//target不需要等于nums[mid]，是因为之前判断过；right需要-1是因为right本身不在考虑的数据范围内（左闭右开）
                    left=mid+1;

                }else{
                    right=mid;
                }

            }
            
        }

        //退出时left==right说明[left,right)区间没有值，直接返回-1
        return -1;
    }
}
```

- 我：核心就是要掌握二分查找左闭右开的思想！
- 反思：
  - 求mid的防止溢出的写法推荐为`mid=left+(right-left)/2;`，因为这样mid可以和left重合，左闭右开中基本都这么做。

### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

#### 首战寄

看到要求时间复杂度是log(m+n)就想到要使用二分查找，但是后续没有思路

#### 官方

hard难度的先略



## 搜索与回溯算法(简单)

### 知识点

1，搜索算法里有bfs和dfs，以树为例

- 树的dfs算法有：前序 中序 后序遍历。用栈实现，这个栈可以是递归函数调用过的形式，也可以是Deque集合
- 树的bfs算法有：层序遍历。用队列实现

### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

codetop==2

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
- 我：不要用Queue和Stack，直接用Deque实现两者。

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

- 时间复杂度 O(N) ： N 为二叉树的节点数量，即 BFS 需循环 N 次。
- 空间复杂度 O(N) ： 最差情况下，即当树为平衡二叉树时，最多有 N/2个树节点同时在 queue 中，使用 O(N) 大小的额外空间。

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

codetop==2

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

codetop==13

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
  - 我：linkedList和deque都支持“offerFirst/offerLast和pollFirst/pollLast”，[参考](https://www.jianshu.com/p/ae28d514003c)

- 注意：我` for(int i = 0; i <queue.size(); i++) `，会导致往队列加元素后队列长度变化，导致的无法按层输出。但是题解中` for(int i = queue.size(); i > 0; i--)`的写法，就保证一层级的节点数不会被后续加入队列的节点所影响。
  - 求size的时间复杂度，[参考](https://blog.csdn.net/Kevin___________/article/details/108681594)
- 我：linkedlist是可以存null的，但是这里没存，没有用上这个性质


#### 官方2-层序遍历 + 双端队列（奇偶层逻辑分离）
- 方法一代码简短、容易实现；但需要判断每个节点的所在层奇偶性，即冗余了 N 次判断。
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
- 我：打印奇数层和偶数层中，在加入子节点后，都是让left在靠近first的地方，right在靠近last的地方。





### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

codetop==12

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

- 我和网友：helper专门用来判断左右子树，看左右子树是否正在完美贴合；isSubStructure的作用就是按照程序的定义，看B是不是A的子结构。
- 我：root2==null，则root1不管是不是null，都表示这条遍历成功；root2！=null，但root1为null，说明两个子树在此位置不匹配，返回false。



### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

codetop==25

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

- 我：这里写的recur没有意义，因为可以和题目已有的函数mirrorTree重合；我这写recur应该是受到offer26的影响；正确的递归写法应该是“官方-递归”这样子，不需要recur做辅助函数。

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

- 我之前的错误思想：官方思路和我一样，不过我自建了一个recur函数，让recur专门负责子节点的处理，让逻辑更清晰。
  - 20230127我：这个recur的代码和mirrorTree代码基本一致，是无效的创造函数，造成代码冗余；还是应该官方更好。

- 我：交换二叉树每个节点的左右子树，即可得到二叉树的镜像。

复杂度分析:

- 时间复杂度：O(N)，其中 N  为二叉树节点的数目。我们会遍历二叉树中的每一个节点，对每个节点而言，我们在常数时间内交换其两棵子树。

- 空间复杂度：O(N) 。使用的空间由递归栈的深度决定，它等于当前节点在二叉树中的高度。在平均情况下，二叉树的高度与节点个数为对数关系，即 O(logN)。而在最坏情况下，树形成链状，空间复杂度为 O(N)。

### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

codetop==2

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
### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

和剑指32相同

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        /**
        层序遍历就得用到bfs，可以使用deque实现队列
        
        想知道什么时候一层打印完，就得在打印这一层之前计算本层节点数
         */
        
        /*
        处理特殊情况。
        
        ！！！注意之前任务root==null时要返回null，但是其实要返回的是空列表[]。而且root为null时return root返回的是null，但是编译无法通过，说TreeNode类型的root无法作为List<List<Integer>>类型的返回值。

        所以还是得返回一个空数组
         */
        if(root==null)return new ArrayList<>();
        
        //结果列表，记录每一层的节点情况
        List<List<Integer>> res=new ArrayList<>();
        //实施bfs
        Deque<TreeNode> queue=new LinkedList<>();
        //从last给节点，从first拿节点
        queue.offerLast(root);

        //尽量使用isEmpty判空，保证o1的时间复杂度；用size==0在juc类下可能会有on的时间复杂度
        while(!queue.isEmpty()){
            //记录每一层的元素
            List<Integer> temp=new ArrayList<>();
            //i在开始遍历本层之间就记录本层的节点数
            for(int i=queue.size();i>=1;i--){
                //从本层中拿到节点
                TreeNode cur=queue.pollFirst();
                temp.add(cur.val);
                if(cur.left!=null)queue.offerLast(cur.left);//linkedlist是可以存入null的（https://www.jianshu.com/p/c32e192e371d），但是本例情景不能往queue中存入null
                if(cur.right!=null)queue.offerLast(cur.right);
            }
            //把当前层的节点情况存入结果列表。temp中的元素不会被修改就会重新分配内存，所以这里的temp不需要重新包裹一层ArrayList才给res
            res.add(temp);
        }

        return res;

        

    }
}
```



### [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)

和[剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)类似，但是不相同；剑指说的是返回结果值锯齿状就行，那么就可以用双端队列cheat；但是主站这道题小括号中描述的就是以锯齿状方式遍历，可能就是推荐遍历的时候就是要用锯齿的方式。

#### 首战成功

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        /**
        使用两个队列，一个是当前层队列，一个是下一层队列：
        1. 构成奇数层的队列时，从first开始放，先放右子节点，再放左子节点
        2. 构成偶数层的队列时，从first开始放，先放左子节点，在放右子节点
         */
        //处理特殊情况
        if(root==null)return new LinkedList<>();


        List<List<Integer>> res=new LinkedList<>();
        //实现bfs就要用队列，规定从last入队，从first出队
        Deque<TreeNode> cur=new LinkedList<>();
        cur.offerLast(root);

        //bfs。从已有的第一层开始处理，即从第二层开始构建
        int curLevel=1;
        while(!cur.isEmpty()){//只要当前队列不为空就要处理
            //建立本层专属的临时集合，在遍历的时候按顺序存入数据
            List<Integer> temp=new ArrayList<>();
            //每次层前清空存储next层的队列
            Deque<TreeNode> next=new LinkedList<>();

            //处理当前层的每个节点。！！！i至少为1才有节点
            for(int i=cur.size();i>0;i--){
                TreeNode curNode=cur.pollFirst();
                //当前层temp加入节点的值
                temp.add(curNode.val);
                //根据构建的是奇数层还是偶数层来决定先加右节点还是左节点
                if((curLevel&1)==1){//如果当前层是奇数层，那么构建的就是偶数层
                    //从first，先加入右子节点，再加入左子节点
                    if(curNode.left!=null)next.offerFirst(curNode.left);
                    if(curNode.right!=null)next.offerFirst(curNode.right);

                }else{//如果当前层是偶数层，那么构建的就是寄数层
                    //从first，先加入右子节点，再加入左子节点。！！！注意，节点不为空才加入列表
                    if(curNode.right!=null)next.offerFirst(curNode.right);
                    if(curNode.left!=null)next.offerFirst(curNode.left);
                }

            }
            //把当前层结果存入结果列表
            res.add(temp);
            //层数加一
            curLevel++;
            //next作为当前层
            cur=next;
            
        }

        //处理完节点后，返回
        return res;


    }
}
```

- 20230127我：参考剑指26的官方就好，我这里自己写的首战代码，在一年多后看不是很容易看懂。

## 搜索与回溯算法(中等)

### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

codetop==7

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

codetop==3

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

- 我：我这个时间复杂度也是omn，因为所有单元格只用处理一次。

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

codetop==15

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

codetop==39

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

codetop==36

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
2. 递归到第 k个节点时，应记录结果 res ；
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

### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

codetop==3

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

- 20230129我：这递归思想好啊，我重做的时候，本来想直接普通dfs，每深一层就让变量+1，然后用全局变量保存最深的地方；所以深度相关的都可以考虑这种递归的写法。

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

codetop==67+2

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
  - 20230129我：有时候用后续遍历，可以用到之前的计算结果，就可以避免重复计算。


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
- java.lang.Math包的常用函数：`max,min,abs`
- 20230224我：先看左子树是不是平衡二叉树，再看右子树是不是平衡二叉树，最后看当前节点为根节点的树是不是平衡二叉树。高度为-1（自底向上出现了不平衡）或真实高度（自底向上一直平衡）

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

codetop==2

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

codetop==2

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
  - 得在`right=recur(root.right,p,q);`之前也加个剪枝，避免在left找到目标后还进入right。本来想其实进入right的瞬间就是执行`if(root==null)return false;`，也就返回了，所以可以不加剪枝；但是不加剪枝的话，总归是有一次函数的瞬间入栈出栈，还是消耗了资源的，所以最好加上本剪枝。
  - 后序遍历+剪枝，是树的常见优化dfs方案。

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
- 20230224我：当前节点第一次出现在pq之间时，当前就是最近公共祖先的位置。而且不需要考虑触底条件，因为从根节点往下二叉搜索寻找的时候，一定能在到达根节点之前找到目标公共祖先（题目说pq都存在）

### [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

codetop==162+1

#### 首战告捷

直接使用“剑指 Offer 68 - I. 二叉搜索树的最近公共祖先”中“首战告捷”的方法即可：

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

#### 官方-dfs

解题思路：

![image-20220430173329452](lcof.assets/image-20220430173329452.png)

![image-20220430173340903](lcof.assets/image-20220430173340903.png)

递归解析：

![image-20220430173409116](lcof.assets/image-20220430173409116.png)

复杂度分析：

![image-20220430173443681](lcof.assets/image-20220430173443681.png)

代码：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null && right == null) return null; // 1.
        if(left == null) return right; // 3.
        if(right == null) return left; // 4.
        return root; // 2. if(left != null and right != null)
    }
}
```

- 我：官方也是后续遍历+剪枝的思路，先拿到左右子树的结果，在做本节点的处理。

高赞网友评论版：

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //1.如果找不到p或q就返回null,如果找的到就返回该点 ps:我觉得return root设计的很巧妙
        if(root == null || root == p || root == q) return root; 
        //2.如果左子树中有p或q,那么就会返回找到的点。或者p和q都有，就返回pq的公共点。或者p或q都没有就返回null。
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        //3.如果右子树中有p||q,那么就会返回找到的点。或者p和q都有，就返回pq的公共点。或者p或q都没有就返回null。
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //4.left==null 说明：左子树中没有p或q(往上看最开始的判断语句：找不到就返回null)
        // 5.直接返回right的原因，需要结合递归的思想多方面思考。
        if(left == null) return right;
        //6.运行到此处说明左子树中找到了p或q点。
        if(right == null) return left;
        //7.运行到此处说明p和q都找到了，root为最近的公共祖先。(该语句只会运行一次)
        return root;
    }
}
```

- 我：他这个省掉了官方的`if(left == null && right == null) return null;`，其实也合理，因为right不为空left为空返回right，right为空left为空返回的null也正是right的值。
- 我：感觉不如我首战告捷的好理解，全局变量yyds！！！
- 20230129我：感觉还是官方代码比网友代码更好理解。



### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

与[剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)相同

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
    //记录全局变量，保证找到结果后不会继续递归，节约时间。不过这会影响整个代码逻辑，就先不优化了。
    //boolean gotten=false;
    
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        /**
        树问题一般可以考虑采用后续遍历：先获取左子树的情况，再获取右子树的情况；然后结合左右子树和当前节点的情况处理当前节点。后续遍历也能实现自底向上的处理，避免重复计算。
         
        根据题意，一开始传入的树不会是空树
         */

        //设置递归触底返回条件
        if(root==null) return null;

        //如果当前递归层传入的节点不为空，进行真正的处理
        //获取左右子树的结果
        TreeNode left=lowestCommonAncestor(root.left,p,q);
        TreeNode right=lowestCommonAncestor(root.right,p,q);
        
        //结合左右子树和当前节点的情况，求出当前节点的返回值。！！返回值只可能是“null p q 非pq的目标节点”
        if(left!=null&&right!=null){//如果左右都有非空节点返回，那么说明当前节点就是目标节点
            return root;
        }else if(left!=null&&right==null){//如果left不为空，right为空，那么就结合当前节点的情况处理
            if(left!=p&&left!=q){//如果左子树的返回不是p或q，那么就是目标节点，直接返回
                return left;
            }else if(root==p||root==q){//能走到这步说明左子树返回的是pq之一；如果左子树有pq之一，本节点也是pq之一，那么本节点就是目标节点，直接返回
                return root;
            }else{//如果左子树是pq之一，本节点不是pq之一，那么直接返回left表示本支线的有效值
                return left;
            }

        }else if(left==null&&right!=null){//如果left为空，right不为空；那么就结合当前节点的情况处理
            if(right!=p&&right!=q){
                return right;
            }else if(root==p||root==q){
                return root;
            }else{
                return right;
            }

        }else{//对应left和right都为空的情况，判断当前节点是否为pq
            if(root==p||root==q){
                return root;
            }else{//之前把有效情况处理了，最后剩下的就是无效情况；一般这么处理比较不会漏情况。
                return null;//如果左右子树和本节点都没有用信息，返回null表示本支线没目标信息
            }

        }

    }
}
```

- 我：看了自己在剑指的写法，感觉我全局变量+新增recur的写法还是不错的；全局变量在保存res的情况下经常有用！



## 搜索与回溯算法(困难)

### [剑指 Offer 37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

codetop 0

#### 首战寄

没有思路

#### 官方-bfs

解题思路：

![image-20220420160538622](lcof.assets/image-20220420160538622.png)

![Picture1.png](lcof.assets/1603117385-ehAGsP-Picture1.png)

上图规律可总结为下表：

![image-20220420160632130](lcof.assets/image-20220420160632130.png)

![image-20220420160648368](lcof.assets/image-20220420160648368.png)

序列化 算法流程：

![image-20220420160711028](lcof.assets/image-20220420160711028.png)

复杂度分析：

![image-20220420160726530](lcof.assets/image-20220420160726530.png)

反序列化 Deserialize ：

![image-20220420160818756](lcof.assets/image-20220420160818756.png)

算法流程：

![image-20220420160843615](lcof.assets/image-20220420160843615.png)

复杂度分析：

![image-20220420160859162](lcof.assets/image-20220420160859162.png)

代码：

```java
public class Codec {
    public String serialize(TreeNode root) {
        if(root == null) return "[]";
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left);
                queue.add(node.right);
            }
            else res.append("null,");
        }
        //我：这个应该是为了删除null最后的逗号
        res.deleteCharAt(res.length() - 1);
        res.append("]");
        return res.toString();
    }

    public TreeNode deserialize(String data) {
        if(data.equals("[]")) return null;
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        int i = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            //如果子树为null，就不赋值了，也不需要再处理他的子节点
            if(!vals[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            if(!vals[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
}
```

- 注意：
  - linkedList实现的接口Queue，允许队列存入null，这是本题序列化的关键；[参考](https://www.jianshu.com/p/7a86c56c632b)
  - 本题涉及众多SB关于string的处理：
    - deleteCharAt,SetCharAt是比较好用，
  - 我理解：一般bfs不需要记录null，导致无法反序列化；这里把null记录下，相当于标记了叶子节点，也就能反序列化了。

- 建议：
  - 给个意见，res.append(node.val + ","); 这个点可以优化成 res.append(node.val).append(",");。因为拼接字符串可能增大消耗。

- 问题：
  - 网友问：看懂k神的思路了，但是这里好像并没有用到开始讲到的相关地推公式「node.left的索引2(n−m)+1、node.right的索引2(n−m)+2」
    - 网友答：解答中定义的i，就是列表索引2(n−m)+1和2(n−m)+2 任意节点非空，那它的两个儿子（不管是不是null）肯定要列表中占位，做两次i+=1。

#### 即时再战

因为本题没上codetop，理解思路即可，暂时不再战

### [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

codetop 8+7=15 

#### 首战告捷

```java
class Solution {
    //不能用List！！！本来想用全局string列表保存结果，列表最后会被ls.toArray(new String[length])转化为数组
    //List<String> resultList=new ArrayList<>();
    //注意因为可能输入"aab"这种字符串，所以要用set去重（但其实在构建重复String的时候做了很多无用计算，不过目前没有好方法优化），而不能用list
    Set<String> resultSet=new HashSet<>();
    public String[] permutation(String s) {
        /**
        递归的思想，当前层用一个字符，传到下一层递归的时候就不能再本字符
        所有可用字符在本层递归依次使用
         */
        //把字符串转化成sb，好对字符做处理。比如使用过的字符就从本层专属sb中删掉deletecharat再传给下层,递归完毕后用append把sb还原
        StringBuilder sb=new StringBuilder();
        for(int i=0;i<s.length();i++){
            sb.append(s.charAt(i));
        }

        //对每个字符，轮流称为本层的主角
        recur(sb,new StringBuilder());

        //return resultList.toArray(new String[resultList.size()]);不能用list，我就只能用iterator构建数组返回了
        //!!!注意iterator的使用
        Iterator<String> iterator=resultSet.iterator();
        String[] result=new String[resultSet.size()];
        int i=0;
        while(iterator.hasNext()){
            result[i]=iterator.next();
            i++;
        }
        return result;

    }
    //任何层的recur中，availableChars减少1，stringUnderBuilding就会增加1
    public void recur(StringBuilder availableChars, StringBuilder stringUnderBuilding){
        //availableChars为空的话就不会执行for，而在保存完当前stringUnderBuilding的结果后退栈
        if(availableChars.length()==0){
            resultSet.add(stringUnderBuilding.toString());
        }

        //sb可以用length()得到长度,用charAt得到某位置字符，和String中的用法基本一致!!!
        for(int i=0;i<availableChars.length();i++){
            //保存本for循环层处理的char
            char temp=availableChars.charAt(i);
            
            //递归调用。在递归完后，要恢复本层的availabelChars和stringUnderBuilding，以便进行下一轮for循环，for循环中又会有递归
            stringUnderBuilding.append(availableChars.charAt(i));
            availableChars.deleteCharAt(i);
            recur(availableChars,stringUnderBuilding);
            availableChars.insert(i,temp);
            stringUnderBuilding.deleteCharAt(stringUnderBuilding.length()-1);

        }

    }
}
```

- 反思：
  - 最开始没有考虑到字符串中有重复字符如"abb"，使用list来存储结果，导致返回结果把两个相同的a当做a1和a2处理了，和预期不一致。后面改为用set进行结果去重，不过程序处理时还是会把a当做a1和a2处理造成无效计算最后又利用set唯一性舍弃。
    - 可以效仿官方的方法，在for那使用set做剪枝（判断`availableChars.charAt(i);`是否出现过），这样就不会有无效计算了。
  - iterator，iterator(),hasNext(),next()这套使用不熟练。不过可以直接用foreach写法遍历。
  - StringBuilder使用不熟练。实际上String有的方法基本上StringBuilder都有，但是StringBuilder还有跟多的方法如append，**deleteCharAt，insert**等；要熟练掌握StringBuilder的方法。

- 赞：
  - 递归思想处理的不错，尤其是传递地址的变量在递归后要修改回来这点做的不错。
  - 想到了用Set来去重

#### 大佬-回溯法+交换

解题思路：

![image-20220420202407877](lcof.assets/image-20220420202407877.png)

排列方案的生成：

![image-20220420202432627](lcof.assets/image-20220420202432627.png)

![Picture1.png](lcof.assets/1599403497-KXKQcp-Picture1.png)

重复排列方案与剪枝：

当字符串存在重复字符时，排列方案中也存在重复的排列方案。为排除重复方案，需在固定某位字符时，保证 “每种字符只在此位固定一次” ，即遇到重复字符时不交换，直接跳过。从 DFS 角度看，此操作称为 “剪枝” 。

![Picture2.png](lcof.assets/1599403497-GATdFr-Picture2.png)

递归解析：

![image-20220420202538716](lcof.assets/image-20220420202538716.png)

视频：可以[看看](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/mian-shi-ti-38-zi-fu-chuan-de-pai-lie-hui-su-fa-by/)

复杂度分析：

![image-20220420202654493](lcof.assets/image-20220420202654493.png)

代码：

```java
class Solution {
    //为了让递归函数添加结果方便，定义到函数之外，这样无需带到递归函数的参数列表中
    List<String> list = new ArrayList<>();
    //同；但是其赋值依赖c，定义声明分开
    char[] c;
    public String[] permutation(String s) {
        c = s.toCharArray();
        //从第一层开始递归
        dfs(0);
        //将字符串数组ArrayList转化为String类型数组
        return list.toArray(new String[list.size()]);
    }

    private void dfs(int x) {数到达第三层，就返回，因为此时第二第三个位置已经发生了交换
        if (x == c.length - 1) {
            //将字符数组转换为字符串
            list.add(String.valueOf(c));
            return;
        }
        //为了防止同一层递归出现重复元素
        HashSet<Character> set = new HashSet<>();
        //这里就很巧妙了,第一层可以是a,b,c那么就有三种情况，这里i = x,正巧dfs(0)，正好i = 0开始
        // 当第二层只有两种情况，dfs(1）i = 1开始
        for (int i = x; i < c.length; i++){
            //发生剪枝，当包含这个元素的时候，直接跳过
            if (set.contains(c[i])){
                continue;
            }
            set.add(c[i]);
            //交换元素，这里很是巧妙，当在第二层dfs(1),x = 1,那么i = 1或者 2， 不是交换1和1，要就是交换1和2
            swap(i,x);
            //进入下一层递归
            dfs(x + 1);
            //返回时交换回来，这样保证到达第1层的时候，一直都是abc。这里捋顺一下，开始一直都是abc，那么第一位置总共就3个交换
            //分别是a与a交换，这个就相当于 x = 0, i = 0;
            //     a与b交换            x = 0, i = 1;
            //     a与c交换            x = 0, i = 2;
            //就相当于上图中开始的三条路径
            //第一个元素固定后，每个引出两条路径,
            //     b与b交换            x = 1, i = 1;
            //     b与c交换            x = 1, i = 2;
            //所以，结合上图，在每条路径上标注上i的值，就会非常容易好理解了
            swap(i,x);
        }
    }

    private void swap(int i, int x) {
        char temp = c[i];
        c[i] = c[x];
        c[x] = temp;
    }
}
```

- 网友点评：通过交换来固定某个位置的元素这个思路真的太棒了，就 abc 这个字符串来说，第一个位置可以放 a 或者 b 或者 c，但是如果确定要放某个字符，比如第一个位置放 a，那么第二个位置就只能放 b 或者 c；如果第一个位置放 b，那么第二个位置就只能放 a 或者 c；如果第一个位置放 c，那么第二个位置就只能放 a 或者 b；当把某个字符移动到第一位以后，暂时第一位的字符就**固定住**了，这时再去确定第二个位置的元素，并且此时第一个位置的元素不会再出现在后面的位置上，依次类推直到确定所有位置的元素，再往前回溯确定每个位置上其他可能出现的元素。

- 网友解释递归中必须恢复字符串的原因：

  ```
  好像有几个同学对恢复交换有疑惑 我来举个调用例子帮大家解解惑
  
  比如 abc 注意abc是基准字符串
  
  一开始第一层 第一次循环 有 a 和自己交换 得到abc 进入第二层 b和自己交换 得到abc 进入最后一层 结束 得到排列 abc
  
  返回上一层 也就是第二层 b和c交换 进入第三层 结束 得到排列acb 返回到第二层 第二层两重循环也已经结束 返回到第一层
  
  注意 因为没有恢复交换 基准字符串变成了acb 接下来将对acb进行交换操作
  
  现在是第一层 第二次循环 a和c 交换 c没有使用过 得到cab 同前一样 分别可得到 cab 和 cba 然后返回第一层
  
  注意 同样因为没有恢复交换 此时基准字符串变成了cba 接下来对cba进行交换操作
  
  现在又是第一层 第三次循环 进行c和a交换 但是if发现 这个a我们最开始已经使用过 所以这个分枝就over了 第三次循环结束
  
  到此整个调用结束 丢失了两个排列
  
  究其原因就是因为没有恢复字符串 导致基准字符串发生了两次改变 因此每一层调用的基准字符串都必须恢复原形才能保证完整性 不能中途更换基准字符串
  ```

#### 另一大佬-回溯模板

大佬的思路太猛了，我还是套用回溯算法模板吧！

```java
class Solution {
    
    /**
        该题类似于 全排列2，本题使用set来去除重复元素
        除了使用set去重外，还可以对数组进行排序，使用visited数组进行剪枝！

    */
    Set<String> res = new HashSet();
    public String[] permutation(String s) {

        backtrack(s.toCharArray(),new StringBuilder(), new boolean[s.length()]);
        return res.toArray(new String[0]); 

    }
    
    // 回溯函数
    public void backtrack(char[] ch,StringBuilder sb, boolean[] visitid){
        // 终止条件
        if(sb.length() == ch.length){
            res.add(sb.toString());
            return;
        }
        // 选择列表
        for(int i = 0; i < ch.length; i++){
            // 剪枝，如果当前位置的元素已经使用过，则跳过进入下一个位置
            if(visitid[i]) continue;
            // 做出选择
            sb.append(ch[i]);
            // 更新标记
            visitid[i] = true;
            // 进入下层回溯
            backtrack(ch,sb,visitid);
            // 撤销选择
            sb.deleteCharAt(sb.length()-1);
            visitid[i] = false;
            
        }
    }
}
```

- 我：感觉这个方法更好理解；思路和我首战自己写的基本一样，但是优雅很多！

- 我：注意`collection.toArray(new T[0])`可以把集合转化为数组，toArray方法来自于collection接口所以下面这些实现了collection接口的类都能使用toArray方法：

  ![image-20230224131359793](lcof.assets/image-20230224131359793.png)

### [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

#### 首战寄

没什么好思路

#### 官方-dfs遍历数组

1，在 LeetCode 中，「岛屿问题」是一个系列系列问题，比如：

```
L200. 岛屿数量 （Easy）
463. 岛屿的周长 （Easy）
695. 岛屿的最大面积 （Medium）
827. 最大人工岛 （Hard）
```

我们所熟悉的 DFS（深度优先搜索）问题通常是在树或者图结构上进行的。而我们今天要讨论的 DFS 问题，是在一种「网格」结构中进行的。岛屿问题是这类网格 DFS 问题的典型代表。网格结构遍历起来要比二叉树复杂一些，如果没有掌握一定的方法，DFS 代码容易写得冗长繁杂。

本文将以岛屿问题为例，展示网格类问题 DFS 通用思路，以及如何让代码变得简洁。

2，网格类问题的 DFS 遍历方法-网格问题的基本概念：

我们首先明确一下岛屿问题中的网格结构是如何定义的，以方便我们后面的讨论。

网格问题是由 m ×n 个小方格组成一个网格，每个小方格与其上下左右四个方格认为是相邻的，要在这样的网格上进行某种搜索。

岛屿问题是一类典型的网格问题。每个格子中的数字可能是 0 或者 1。我们把数字为 0 的格子看成海洋格子，数字为 1 的格子看成陆地格子，这样相邻的陆地格子就连接成一个岛屿。

![岛屿问题示例](lcof.assets/c36f9ee4aa60007f02ff4298bc355fd6160aa2b0d628c3607c9281ce864b75a2.jpg)

在这样一个设定下，就出现了各种岛屿问题的变种，包括岛屿的数量、面积、周长等。不过这些问题，基本都可以用 DFS 遍历来解决。

3，DFS 的基本结构：

网格结构要比二叉树结构稍微复杂一些，它其实是一种简化版的图结构。要写好网格上的 DFS 遍历，我们首先要理解二叉树上的 DFS 遍历方法，再类比写出网格结构上的 DFS 遍历。我们写的二叉树 DFS 遍历一般是这样的：

```java
void traverse(TreeNode root) {
    // 判断 base case
    if (root == null) {
        return;
    }
    // 访问两个相邻结点：左子结点、右子结点
    traverse(root.left);
    traverse(root.right);
}
```

可以看到，二叉树的 DFS 有两个要素：「访问相邻结点」和「判断 base case」。

第一个要素是**访问相邻结点**。二叉树的相邻结点非常简单，只有左子结点和右子结点两个。二叉树本身就是一个递归定义的结构：一棵二叉树，它的左子树和右子树也是一棵二叉树。那么我们的 DFS 遍历只需要递归调用左子树和右子树即可。

第二个要素是 **判断 base case**。一般来说，二叉树遍历的 base case 是 root == null。这样一个条件判断其实有两个含义：一方面，这表示 root 指向的子树为空，不需要再往下遍历了。另一方面，在 root == null 的时候及时返回，可以让后面的 root.left 和 root.right 操作不会出现空指针异常。

对于网格上的 DFS，我们完全可以参考二叉树的 DFS，写出网格 DFS 的两个要素：

首先，网格结构中的格子有多少相邻结点？答案是上下左右四个。对于格子 (r, c) 来说（r 和 c 分别代表行坐标和列坐标），四个相邻的格子分别是 (r-1, c)、(r+1, c)、(r, c-1)、(r, c+1)。换句话说，网格结构是「四叉」的。

![网格结构中四个相邻的格子](lcof.assets/63f5803e9452ccecf92fa64f54c887ed0e4e4c3434b9fb246bf2b410e4424555.jpg)

其次，网格 DFS 中的 base case 是什么？从二叉树的 base case 对应过来，应该是网格中不需要继续遍历、grid[r][c] 会出现数组下标越界异常的格子，也就是那些超出网格范围的格子。

![网格 DFS 的 base case](lcof.assets/5a91ec351bcbe8e631e7e3e44e062794d6e53af95f6a5c778de369365b9d994e.jpg)

这一点稍微有些反直觉，坐标竟然可以临时超出网格的范围？这种方法我称为「先污染后治理」—— 甭管当前是在哪个格子，先往四个方向走一步再说，如果发现走出了网格范围再赶紧返回。这跟二叉树的遍历方法是一样的，先递归调用，发现 root == null 再返回。

这样，我们得到了网格 DFS 遍历的框架代码：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    // 如果坐标 (r, c) 超出了网格范围，直接返回
    if (!inArea(grid, r, c)) {
        return;
    }
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

- 我：这个inarea函数还蛮优雅的

4,如何避免重复遍历:

网格结构的 DFS 与二叉树的 DFS 最大的不同之处在于，遍历中可能遇到遍历过的结点。这是因为，网格结构本质上是一个「图」，我们可以把每个格子看成图中的结点，每个结点有向上下左右的四条边。在图中遍历时，自然可能遇到重复遍历结点。

这时候，DFS 可能会不停地「兜圈子」，永远停不下来，如下图所示：

![DFS 遍历可能会兜圈子（动图）](lcof.assets/7fec64afe8ab72c5df17d6a41a9cc9ba3879f58beec54a8791cbf108b9fd0685.gif)

如何避免这样的重复遍历呢？答案是标记已经遍历过的格子。以岛屿问题为例，我们需要在所有值为 1 的陆地格子上做 DFS 遍历。每走过一个陆地格子，就把格子的值改为 2，这样当我们遇到 2 的时候，就知道这是遍历过的格子了。也就是说，每个格子可能取三个值：

- 0 —— 海洋格子
- 1 —— 陆地格子（未遍历过）
- 2 —— 陆地格子（已遍历过）

我们在框架代码中加入避免重复遍历的语句：

```java
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    if (!inArea(grid, r, c)) {
        return;
    }
    // 如果这个格子不是岛屿，直接返回
    if (grid[r][c] != 1) {
        return;
    }
    grid[r][c] = 2; // 将格子标记为「已遍历过」
    
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

![标记已遍历的格子](lcof.assets/20fe202fb5e5fc5048e140c29310c1bcbb17661860d2441e8a3feb1236a2e44d.gif)

这样，我们就得到了一个岛屿问题、乃至各种网格问题的通用 DFS 遍历方法。以下所讲的几个例题，其实都只需要在 DFS 遍历框架上稍加修改而已。

> 小贴士：
>
> 在一些题解中，可能会把「已遍历过的陆地格子」标记为和海洋格子一样的 0，美其名曰「陆地沉没方法」，即遍历完一个陆地格子就让陆地「沉没」为海洋。这种方法看似很巧妙，但实际上有很大隐患，因为这样我们就无法区分「海洋格子」和「已遍历过的陆地格子」了。如果题目更复杂一点，这很容易出 bug。

5，代码：

```java
class Solution {
    private int res;
    public int numIslands(char[][] grid) {
        res = 0;
        for (int i = 0; i < grid.length; i ++) {
            for (int j = 0; j < grid[0].length; j ++) {
                if (grid[i][j] == '1') {//处理每一个还没被处理的岛屿
                    dfsGrid(grid, i, j);//从当前节点登录岛屿，把岛屿的每个方格标记为2，避免重复处理节点
                    res ++;//每遍历到一个还没被处理的岛屿就把结果加一
                }
            }
        }
        return res;
    }

    //本函数的功能主要从岛屿的“登录节点”开始把整个岛屿标记为2
    private void dfsGrid(char[][] grid, int row, int col) {
        if (row >= grid.length || col >= grid[0].length || row < 0 || col < 0) {//先污染后治理，如果发现坐标超过数组边界就退回
            return;
        }

        if (grid[row][col] != '1') {//如果当前节点不是可处理的节点，就退回
            return;
        }

        grid[row][col] = '2';
        dfsGrid(grid, row - 1, col);//递归地把当前节点所属岛屿的所有节点标记为2
        dfsGrid(grid, row + 1, col);
        dfsGrid(grid, row, col - 1);
        dfsGrid(grid, row, col + 1);
    }
}
```

#### 即时再战成功

```java
class Solution {
    int res=0;
    public int numIslands(char[][] grid) {
        /**
        经典的数组构成的岛屿问题。使用dfs处理：
        1. 遍历节点，一旦发现1就在1处登录该岛屿，并用dfs把整个岛屿标记为2
        2. 每遍历到一个1个岛屿，就给岛屿计数加一
         */
        
        //遍历每个节点
        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){
                //如果当前节点是一个岛屿的登录点，登录！并且增加计数
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    res++;
                }
                //如果不是登录点就查看下一个遍历元素
            }
        }

        return res;
    }

    public void dfs(char[][] grid,int i,int j){
        //判断是否超过了数组边界,超过了就返回。避免后续出现超过数组边界的异常。
        if(i<0||i>=grid.length||j<0||j>=grid[0].length){
            return;
        }

        //判断当前节点是否为可处理的1，如果不是1则不用处理直接返回
        if(grid[i][j]!='1')return;

        //处理本节点：标记本节点为2表示处理过+dfs对四个方向递归
        grid[i][j]='2';
        dfs(grid,i-1,j);
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);
    }
}
```

### [46. 全排列](https://leetcode.cn/problems/permutations/)

与[剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)类似。

#### 首战告捷

```java
class Solution {
    Integer[] globalNums;//!!!!注意必须要设置为Integer，不然Arrays.toList(array)时会把整个int[]看做一个对象；参考：https://www.cnblogs.com/tina-smile/p/5056174.html
    List<List<Integer>> res;
    public List<List<Integer>> permute(int[] nums) {
        /**
        使用剑指38的回溯法
         */
        
        //让传入的数组可以全局使用
        globalNums=new Integer[nums.length];//一定要先给globalNums内存，不然把nums的值赋值给globalNums时会空指针异常
        for(int i=0;i<nums.length;i++){
            globalNums[i]=nums[i];//自动装箱
        }
        //构建用于存储结果的列表
        res=new ArrayList<>();

        //深度递归查找组合
        dfs(0);

        return res;

    }

    //深度递归
    public void dfs(int x){
        //设置触底，即什么时候把单条结果存入res
        if(x==globalNums.length-1){
            /**
            Arrays.asList返回的不是真正的List，一般最好用new ArrayList包裹一下；并且包裹的时候最好用泛型<>指定类型。

            ！！！注意:
            字符串转化为字符数组：string.toCharArray();
            列表转化为数组：list.toArray(new T[0]);
            数组转化为列表：Arrays.asList(array);
             */
            //System.out.println("此时得到的结果是：");
            //printNums();
            res.add(new ArrayList<Integer>(Arrays.asList(globalNums)));

            return;
        }

        //如果还没触底，说明globalnums从前往后固定还在进行中

        //把globalnums的“x位置及之后”的每个数字都依次放到x的位置固定住
        for(int i=x;i<globalNums.length;i++){
            //交换globalnums的第i和x个元素，即固定globalnums的第x位元素
            swap(i,x);
            
            //固定好第x层后，去固定第x+1层
            dfs(x+1);

            //dfs后要swap回来，这样不会影响本层的其他深度递归
            swap(i,x);
        }
    }

    public void swap(int i,int x){
        int temp=globalNums[i];
        globalNums[i]=globalNums[x];
        globalNums[x]=temp;
    }

    // public void printNums(){
    //     for(int num:globalNums){
    //         System.out.print(num);
    //     }
    //     System.out.println("");
    // }
}
```

- ！！注意点（踩坑）：

  - Arrays.toList(array)的array必须是对象数组，否则如果array是int[]的话会把整个int[]看做一个对象；所以要把传入的int[]转化成Integer[]参考：https://www.cnblogs.com/tina-smile/p/5056174.html

  - Arrays.asList返回的不是真正的List，一般最好用new ArrayList包裹一下；并且包裹的时候最好用泛型<>指定类型

  - 常见转换：

    ```java
    字符串转化为字符数组：string.toCharArray();
    列表转化为数组：list.toArray(new T[0]);
    数组转化为列表：Arrays.asList(array);
    ```

    

### [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)

#### 首战寄

没有很好的思路

#### [大佬-递归](https://leetcode.cn/problems/binary-tree-maximum-path-sum/solution/shou-hui-tu-jie-hen-you-ya-de-yi-dao-dfsti-by-hyj8/)

思路：

- 路径每到一个节点，有 3 种选择：1. 停在当前节点。2. 走到左子节点。3. 走到右子节点。
- 走到子节点，又面临这 3 种选择，**递归适合处理这种规模不同的同一问题**。
- 注意，不能走进一个分支又掉头回来走另一个分支，路径会重叠，不符合题目要求。

![image.png](lcof.assets/9e5f5aee5b5138fdc54eb050860c99462bdf6ee4ed422854370ddc475a5e333e-image.png)

定义递归函数：

- 对于一个父节点，它关心自己走入一个子树，从中捞取的最大收益，不关心具体怎么走。

- 定义dfs函数：返回当前子树能向父节点“**提供**”的最大路径和。即，一条**从父节点延伸下来的路径**，能在当前子树中捞取的最大收益。分三种情况：
  1. 路径停在当前子树的根节点，在当前子树的最大收益：root.val
  2. 走入左子树，在当前子树的最大收益：root.val + dfs(root.left)
  3. 走入右子树，在当前子树的最大收益：root.val + dfs(root.right)
- 这对应了前面所说的三种选择，最大收益取三者最大：root.val+max(0, dfs(root.left), dfs(root.right))

- 再次提醒: 一条**从父节点延伸下来的路径**，不能走入左子树又掉头走右子树，**不能两头收益**。
  - 我：这点很重要。

- 当遍历到null节点时，null 子树提供不了收益，返回 0。

- 如果某个子树 dfs 结果为负，走入它，收益不增反减，该子树就没用，需杜绝走入，像对待 null 一样让它返回 0（壮士断腕）。

![image.png](lcof.assets/514872d02158bbf8b5c60afd3afc3c5b2be597af31d9ab2ffd3add8e2db91c36-image.png)

子树中的内部路径要包含根节点：

- 题意可知，最大路径和，是可能产生于其中一个子树中的，就好比下图左一。

- 所以每递归一个子树，都求一下当前子树内部的最大路径和，见下图右一的绿字，从中比较出最大的。

- 注意: 一个子树内部的路径，要包含当前子树的根节点。如果不包含，那还算什么属于当前子树的路径，那就是当前子树的子树的内部路径了。

- 所以，一个子树内部的最大路径和 = 左子树提供的最大路径和 + 根节点值 + 右子树提供的最大路径和。即`dfs(root.left)+root.val+dfs(root.right)`

![image.png](lcof.assets/11183fd31f145655dc6d730e5355e60117c1248881b674af6e5108caf0bee312-image.png)

复杂度：

- 时间复杂度 O(*N*)，每个节点都要遍历；空间复杂度是 O*(*H)，递归树的深度。

代码：

```java
class Solution {
    int result = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        dfs(root);
        return result;
    }

    // 函数功能：返回当前节点能为父亲提供的贡献，需要结合上面的图来看！
    private int dfs(TreeNode root) {
        // 如果当前节点为叶子节点，那么对父亲贡献为 0
        if(root == null) return 0;
        // 如果不是叶子节点，计算当前节点的左右孩子对自身的贡献left和right
        int left = dfs(root.left);
        int right = dfs(root.right);
        // 更新最大值，就是当前节点的val 加上左右节点的贡献。
        result = Math.max(result, root.val + left + right);
        // 计算当前节点能为父亲提供的最大贡献，必须是把 val 加上！
        int max = Math.max(root.val + left, root.val + right);
        // 如果贡献小于0的话，直接返回0即可！
        return max < 0 ? 0 : max;
    }
}
```

- 我：这应该算后续遍历，自底向上的传递max值，传递到root后就完成了全部计算。不会有多余重复的递归。
- 高赞网友：三个关键点~某子树内部最大值，某子树向外部提供的最大值、全局最大值。

思路梳理：

- 每个子树内部的最大路径和是我想求的，要找出最大的
- 这个内部路径肯定是要走这个子树的root的，而且是要参考左右子树所提供的最大和的
- 想捞取子树所提供的最大和，只能走其中一个分支，因为从root伸进去子树的路径，不能拐来拐去，不能占两路便宜
- 只能在子树里选一条分支走，那就得判断哪个分支提供的路径和更大
- 所以每个递归调用都要返回出【提供给父节点的最大路径和】，它用于计算每个递归调用都要算一下的内部最大路径和

复盘总结:

- 递归一个树，会对每个子树做同样的事（你写的处理逻辑），所以你需要思考要对每个子树做什么事，即思考子问题是什么，大问题怎么拆解成子问题。
- 通过求出每个子树对外提供的最大路径和（返回出来给父节点），从递归树底部向上，不断求出了每个子树内部的最大路径和，后者是求解的目标，它的求解需要前者，搞清楚二者的关系。
- 每个子树的内部最大路径和，都挑战一下最大纪录，递归结束时，最大纪录就有了。
- 思考递归问题，别纠结细节实现，内部细节是子递归帮你去做的，应结合求解的目标，自顶而下、屏蔽细节地思考，思考递归子问题的定义。随着递归出栈，子问题自下而上地解决，最后解决了整个问题。
- 要做的只是写好递归的处理逻辑，怎么处理当前子树？需要返回什么吗？怎么设置递归的出口？
- 没有思路的时候，试着画画递归树找思路。就算做对了，画图也能加深对递归算法的理解。

网友补充的他思考的注释版代码：

```java
    int result = Integer.MIN_VALUE;
    /**
     * 求出以root为根节点的树的最大路径和
     */
    public int maxPathSum(TreeNode root) {
        //因为这棵树的最大路径和 并不一定过传入的这个root结点，所以这里需要个辅助函数遍历每一个结点
        dfs(root);
        return result;
    }

    /**
     * @return 对父节点最大贡献
     */
    private int dfs(TreeNode root) {
        if (root == null) return 0;
        /*1、当前树本身可能就是我们要求的路径和最大的树，
        当前树的最大路径和 = 左子树的最大路径和 + 右子树的最大路径和 + 根节点值*/
        int left = dfs(root.left);
        int right = dfs(root.right);
        //挑战已知的最大路径和
        result = Math.max(result, root.val + left + right);

        /*2、当前树很有可能是其他树的子树的一部分，后面代码要求出的其实就是能对父节点做出的最大贡献
        当前树是属于要求的最大路径和经过的一部分的话，那么首先他一定不能有分支，路径是不能走回头路的！！！题目精髓所在，细品
        当前树的最大贡献值 = max{左子树的最大贡献 , 右子树的最大贡献} + 根节点值 */
        int contribution = root.val + Math.max(left, right);
        //小细节：如果贡献的是个负数，则返回一个0，跟null一样处理，当作没有这条分支可以选择
        return contribution > 0 ? contribution : 0;
    }
```

#### 第二天再战-成功

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
    //把结果初始化为最小整数，这样树中任意节点值都会比它大，就不会漏掉节点
    int res=Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        /**
        思路：
        1，因为只需要路径和，不用把路径打印出来，所以可以考虑函数做成递归的样子。定义函数dfs：返回以输入节点为根节点的子树能提供的最大路径和，注意这里的路径必须经过根节点
        2，考虑使用后序遍历，这样可以自底向上计算最大路径和，避免重复计算
         */
        
        //因为题目说了节点至少有一个，所以不用处理null的特殊情况

        //递归查找最大路径和
        dfs(root);

        return res;
        

    }

    /**
    返回以传入节点为起点的路径的最大路径和，所以左右子树只能去一边。
    1，因为原函数返回值是最大路径和，而我需要每次递归返回子树能贡献的最大路径和，函数定义不一致，所以得自己定义新函数dfs
     */
    public int dfs(TreeNode root){
        //设置递归触底条件。空节点是不会参与贡献最大路径和的
        if(root==null)return 0;

        //因为要后序遍历，所以先拿到左右子树中最大
        int left=dfs(root.left);
        int right=dfs(root.right);
        
        //判断以root为根节点的树中的最大路径和能否更新全局结果res
        int temp=left+root.val+right;//这里求经过根节点的最大路径和，所以root.val就算是负数也要计算进来。left和right都是非负的，这点的实现在后面代码可以看到。
        res= res>temp?res:temp;//!!!反思：官方用了mathmax函数，我用了三目运算符；这两种方式都可以更新res。但我感觉这种基于原值res的更新，用三目运算符的可读性更好；但是官方的写法可以不用新建temp变量，这么看mathmax有利于代码简洁。

        //返回“从当前子树根节点出发的路径”能贡献的最大和
        int tempContribute=root.val+Math.max(left,right);
        return tempContribute>0?tempContribute:0;//当前子树根节点出发的路径最大和大于0才返回；如果是负数就是副作用就不返回，并返回0告知上层本子树不会参与计算最大路径和

    }
}
```

- 我：对照官方代码，写了反思的注释，注释以“！！！反思”开头
- 我：因为dfs定义是“求以输入节点为起点的最大路径和”，和官方函数maxPathSum的定义不一样；所以虽然两者输入输出类型是一样的，但是定义不一样就是不能合并成一个函数。



### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

#### 首战递归略

- 递归算法很简单，不写了。
- 题目优化说用迭代的方式写，用迭代在树遍历领域的两个应用场景：
  - 层序遍历：使用队列。不断迭代，直到队列为空
  - 深度遍历：使用栈（Deque）。迭代的时候不断压栈弹栈，直到队列为空
    - 这种一般是**先序**遍历，即先处理节点值，然后把左右子节点压入栈。

#### 首战迭代寄

一般都是用栈实现先序遍历。

但是实现中序遍历就碰到问题了：

- 节点第一次被访问的时候，左子树还没被加入结果，那么不能把当前节点的内容放进结果。那么当前结果该扔掉吗还是怎么处理呢？
- 又尝试思路把结果先放入结果，然后处理左子树的时候把结果从list头部add，处理右子树的时候把结果从list尾部add。但是这样很乱，实现起来十分困难。

失败的代码：

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
    public List<Integer> inorderTraversal(TreeNode root) {
        /**
        挑战一下迭代算法：
        1，使用栈存储数据
        2，不断压栈弹栈，直到队列为空就遍历完了
         */

        //处理特殊情况
        if(root==null) return null;

        List<Integer> res=new LinkedList<>();
        Deque<TreeNode> stack=new LinkedList<>();//双向队列Deque接口是支持push pop peek这些栈的函数的

        //把首节点压入栈，便于迭代处理
        stack.push(root);

        while(!stack.isEmpty()){//list接口有isempty方法，deque的实现类linkedlist也实现了list接口
            TreeNode cur=stack.pop();
            //中序遍历先处理左子树
            if(cur.left!=null)stack.push(cur.left);
            
        }

        


    }
}
```

解决访问到当前节点时不能立刻存入res的问题：

- 大佬的方法就用颜色区分了节点是第1次，还是第2次访问到当前节点。如果是第一次访问，就把当前节点和左右节点入栈，先处理左子树把左子树上的节点先放入res；如果是第二次访问，说明左子树的节点都被处理完了，就可以把当前节点加入res。

#### 大佬-颜色标记法

> 思路：

官方题解中介绍了三种方法来完成树的中序遍历，包括：

- 递归
- 借助栈的迭代方法
- 莫里斯遍历

在树的深度优先遍历中（包括前序、中序、后序遍历），递归方法最为直观易懂，但**考虑到效率，我们通常不推荐使用递归**。

栈迭代方法虽然提高了效率，但其嵌套循环却非常烧脑，不易理解，容易造成 “一看就懂，一写就废” 的窘况。而且对于不同的遍历顺序（前序、中序、后序），循环结构差异很大，更增加了记忆负担。

因此，我在这里介绍一种 “颜色标记法” （瞎起的名字……），兼具栈迭代方法的高效，又像递归方法一样简洁易懂，更重要的是，这种方法对于前序、中序、后序遍历，能够写出完全一致的代码。

其核心思想如下：

- 使用颜色标记节点的状态，新节点为白色，已访问的节点为灰色。
- 如果遇到的节点为白色，则将其标记为灰色，然后将其右子节点、自身、左子节点依次入栈。
- 如果遇到的节点为灰色，则将节点的值输出。

> 解释一下为什么需要“右子节点、自身、左子节点依次入栈”

我们有一棵二叉树：

```undefined
      			中
              /  \
             左   右
```

前序遍历：中，左，右
中序遍历：左，中，右
后序遍历：左，右，中

本题需要中序遍历。

栈是一种 `先进后出`的结构，出栈顺序为`左，中，右`
那么入栈顺序必须调整为倒序，也就是`右，中，左`

同理，如果是前序遍历，入栈顺序为 `右，左，中`；后序遍历，入栈顺序`中，右，左`

> 代码：

使用这种方法实现的中序遍历如下：

```java
class Solution {
    class ColorNode {
        TreeNode node;
        String color;
        
        public ColorNode(TreeNode node,String color){
            this.node = node;
            this.color = color;
        }
    }
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null) return new ArrayList<Integer>();
            
        List<Integer> res = new ArrayList<>();
        Stack<ColorNode> stack = new Stack<>();
        stack.push(new ColorNode(root,"white"));
        
        while(!stack.empty()){
            ColorNode cn = stack.pop();
            
            if(cn.color.equals("white")){
                if(cn.node.right != null) stack.push(new ColorNode(cn.node.right,"white"));
                stack.push(new ColorNode(cn.node,"gray"));
                if(cn.node.left != null)stack.push(new ColorNode(cn.node.left,"white"));
            }else{
                res.add(cn.node.val);
            }
        }
        
        return res;
    }
}
```

- 我：这种做法没有把null值存入stack，因为stack类不像linkedlist类支持元素为null。

```java
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        WHITE, GRAY = 0, 1
        res = []
        stack = [(WHITE, root)]
        while stack:
            color, node = stack.pop()
            if node is None: continue
            if color == WHITE:
                stack.append((WHITE, node.right))
                stack.append((GRAY, node))
                stack.append((WHITE, node.left))
            else:
                res.append(node.val)
        return res
```

- 我：另一个网友用python写法实现了，他这里把null也存入栈了。java用deque接口去引用linkedlist类，也可以存null；但是没必要存null，存了还得在取的时候判断一下，更麻烦。

### [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

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
    public List<Integer> rightSideView(TreeNode root) {

        //处理特殊情况
        if(root==null)return new ArrayList();

        //能走到这说明至少有一个节点
        Deque<TreeNode> queue=new LinkedList<>();
        List<Integer> res=new ArrayList<>();//保存层序遍历的每层最后一个节点
        //从last进，从first出
        queue.offerLast(root);
        while(!queue.isEmpty()){//collection类都尽量用isempty判断空而不是size
            for(int i=queue.size();i>0;i--){//对每层分层处理，只拿每层的最后一个节点
                //先对当前节点进行处理，再把当前节点的左右子节点入队
                TreeNode cur=queue.pollFirst();
                if(i==1)res.add(cur.val);//该层的最后一个节点值会被放入res
                if(cur.left!=null)queue.offerLast(cur.left);
                if(cur.right!=null)queue.offerLast(cur.right);
            }
        }

        return res;


    }
}
```

- 我：右视图其实就是每一层的展示，自然就想到层序遍历

性能还不错：

```
执行用时：
1 ms
, 在所有 Java 提交中击败了
81.62%
的用户
内存消耗：
40.3 MB
, 在所有 Java 提交中击败了
45.59%
的用户
通过测试用例：
216 / 216
```

#### 官方-bfs

和我一模一样

#### 官方-dfs

思路： 我们按照 「根结点 -> 右子树 -> 左子树」 的顺序访问，就可以保证每层都是最先访问最右边的节点的。（与先序遍历 「根结点 -> 左子树 -> 右子树」 正好相反，先序遍历每层最先访问的是最左边的节点）

时间复杂度： O(N)，每个节点都访问了 1 次。
空间复杂度： O(N)，因为这不是一棵平衡二叉树，二叉树的深度最少是 logN, 最坏的情况下会退化成一条链表，深度就是 N，因此递归时使用的栈空间是 O(N)的。

代码：

```java
class Solution {
    List<Integer> res = new ArrayList<>();

    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0); // 从根节点开始访问，根节点深度是0
        return res;
    }

    private void dfs(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        // 先访问 当前节点，再递归地访问 右子树 和 左子树。
        if (depth == res.size()) {   // 如果当前节点所在深度还没有出现在res里，说明在该深度下当前节点是第一个被访问的节点，因此将当前节点加入res中。
            res.add(root.val);
        }
        depth++;
        dfs(root.right, depth);
        dfs(root.left, depth);
    }
}

```

- 我：感觉bfs更好理解



## 动态规划(简单)

### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

codetop==30

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

> Python 由于语言特性可以省去 sum*s**u**m* 辅助变量和大数越界处理,java也就只给了不处理大数越界的版本

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
- 20230224我：pre1和pre2不需要在计算的时候求余，因为除了第一次计算，其他时候pre1和pre2本身就是求余后的值。



### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

codetop==30

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

codetop==5

笔试遇见：好大夫

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
            profit = Math.max(profit, price - cost);
        }
        return profit;
    }
}
```

- 注意这里java工具类方法`Math.min`和`Math.max`的用法。

### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

和剑指63相同

```java
class Solution {
    public int maxProfit(int[] prices) {
        /**
        遍历数组，记录下最小股票价格；比较最小股票价格买入当前卖出赚的钱vs历史最大赚的钱
         */
        //没有收益的话返回0，所以maxProfit初始化为0
        int maxProfit=0;
        // prices[i] 的最小值为0，所以初始化他为prices[0]
        int min=prices[0];
        for(int i=0;i<prices.length;i++){
            if(prices[i]<min) min=prices[i];
            if(prices[i]-min>maxProfit) maxProfit=prices[i]-min;
        }

        return maxProfit;
    }
}
```

### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

与剑指 Offer 10- II. 青蛙跳台阶问题，相同。

略。

## 动态规划(中等)

### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

codetop==24

#### 首战寄

#### 官方-动态规划

解题思路：

![image-20220331195848158](lcof.assets/image-20220331195848158.png)

动态规划解析：

![image-20220331195938823](lcof.assets/image-20220331195938823.png)

- 我：注意这里dp[i]的定义，不是[0,i]区间内存在的连续子数组最大和，而是以nums[i]结尾的连续子数组最大和。原因就像解析里说的，要兼顾“保证子数组的连续性”和”dp递推“

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

codetop==2

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

codetop==11

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
  - 我：如果实战不放心直接比大小，也可以`charAt-'char'`，如果值大于0，说明charAt取到的字符代表的值更大。


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

1，自画思路图：

![image-20220401181306013](lcof.assets/image-20220401181306013.png)

- 我：即最后的12，如果整体翻译，就是以4为左结尾的翻译可能数（12    4，共一种情况）；如果拆开翻译，即是以2为结尾的返回数（1   2   4，1    24，  共两种情况）；又因为既能翻译为1又能翻译为12，所以是两种翻译情况的种数的和。
  - 比如324，最后分析32，不能作为翻译单元，所以只能拆开3和2；有3   24，3    2    4两种情况，即相当于以2结尾的翻译可能数。
- 我：其实核心思想就是，从右往左处理时，最后的翻译单元是1 还是12；最后的翻译单元是1则对应一个1xxxx翻译，最后的翻译单元是12则对应一个12xxxx翻译，xxxx区域相当于就是动态规划的子问题，共两种情况。如果数字更长的话，也是同理。

2，20220429再看，已倒序处理为例，214可以视为21 【4】也可以视为2 【14】；但是264 只能翻译为2 6 【 4】，中括号【】表示可以继续切分的段。

### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

codetop==14

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

- Java 的  getOrDefault(key, default)， 代表当哈希表包含键 key时返回对应 value ，不包含时返回默认值 default ；可以用containsKey做ifelse判断实现相同功能。

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

### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

codetop==422

和剑指48重复

#### 首战告捷-hashmap+双指针

`uniqueLeft`和`i`构成了双指针。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        /**
        动态规划思想，新最长子串的长度=max(老最长子串的长度，以当前节点为结尾的子串的最大长度)
         */

        int maxL=0;
        //把字符和index加入hash表，这样可以知道每个字符上一次出现是什么时候
        Map<Character,Integer> mymap=new HashMap<>();
        //维护一个左边界，左边界到当前元素中间没有重复字符串
        int uniqueLeft=0;
        for(int i=0;i<s.length();i++){
            char curChar=s.charAt(i);

            //如果字符出现过，获取上次出现的位置
            if(mymap.containsKey(curChar)){
                //如果字符上次出现的位置在unique左边，说明上次出现的位置不在[uniqueLeft,i-1]中（这个区间对应i-1位结尾的最大子串长度），那么i结尾的最大子串范围就是[uniqueLeft,i]
                int lastIndex=mymap.get(curChar);
                if(lastIndex<uniqueLeft){
                    //比较[uniqueLeft,i]和历史最大子串的长度，并更新为大的那个
                    maxL=Math.max(i-uniqueLeft+1,maxL);
                }else{//如果字符上次出现的位置在uniqueLeft及右边，那么更新uniqueLeft为当前字符上次出现的位置+1。肯定不会比i-1结尾的子字符串的长度长，所以不用比较更新了
                    uniqueLeft=lastIndex+1;
                }

                //并更新本字符对应的index
                mymap.put(curChar,i);
            }else{//如果当前字符没出现过，尝试更新maxL并把当前字符加入hashmap
                maxL=Math.max(i-uniqueLeft+1,maxL);
                mymap.put(curChar,i);
            }
            
        }

        //on遍历一遍后，得到最终的maxL
        return maxL;
    }
}
```


### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

和[剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)相同。

#### 首战告捷

```java
class Solution {
    public int maxSubArray(int[] nums) {
        /**
        动态规划思想，以当前数组元素结尾的最大和，与前一元素结尾的最大和有关
         */
        
        int maxHistory=nums[0];
        int preMax=nums[0];

        //遍历数组
        for(int i=1;i<nums.length;i++){
            int curMax=0;
            //如果以前一个元素为结尾的数组的最大和小于0，说明其对以当前元素为结尾的数组的最大和起副作用，忽略preMax
            if(preMax<=0){
                curMax=nums[i];
                if(curMax>maxHistory) maxHistory=curMax;
            }else{
                curMax=preMax+nums[i];
                if(curMax>maxHistory) maxHistory=curMax;
            }

            preMax=curMax;
        }

        return maxHistory;

    }
}
```



## 动态规划(困难)

### [剑指 Offer 19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

codetop==5+18=23

#### 首战寄

没有思路

#### 官方-dp

解题思路：

![image-20220421202317959](lcof.assets/image-20220421202317959.png)

转移方程：

![image-20220421202334746](lcof.assets/image-20220421202334746.png)

初始条件：

![image-20220421202354680](lcof.assets/image-20220421202354680.png)

结果：

![image-20220421202408135](lcof.assets/image-20220421202408135.png)

代码：

```java
class Solution {
    public boolean isMatch(String A, String B) {
        int n = A.length();
        int m = B.length();
        boolean[][] f = new boolean[n + 1][m + 1];

        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                //分成空正则和非空正则两种
                if (j == 0) {
                    f[i][j] = i == 0;
                } else {
                    //非空正则分为两种情况 * 和 非*
                    if (B.charAt(j - 1) != '*') {
                        if (i > 0 && (A.charAt(i - 1) == B.charAt(j - 1) || B.charAt(j - 1) == '.')) {
                            f[i][j] = f[i - 1][j - 1];
                        }
                    } else {
                        //碰到 * 了，分为看和不看两种情况
                        //不看
                        if (j >= 2) {
                            f[i][j] |= f[i][j - 2];
                        }
                        //看
                        if (i >= 1 && j >= 2 && (A.charAt(i - 1) == B.charAt(j - 2) || B.charAt(j - 2) == '.')) {
                            f[i][j] |= f[i - 1][j];
                        }
                    }
                }
            }
        }
        return f[n][m];
    }
}
```

#### 官方2-递归

分析和dp法相同，但递归写法更好理解：

```java
class Solution {
    public boolean isMatch(String A, String B) {
        // 如果字符串长度为0，需要检测下正则串
        if (A.length() == 0) {
            // 如果正则串长度为奇数，必定不匹配，比如 "."、"ab*",必须是 a*b*这种形式，*在奇数位上
            if (B.length() % 2 != 0) return false;
            int i = 1;
            while (i < B.length()) {
                if (B.charAt(i) != '*') return false;
                i += 2;
            }
            return true;
        }
        // 如果字符串长度不为0，但是正则串没了，return false
        if (B.length() == 0) return false;
        // c1 和 c2 分别是两个串的当前位，c3是正则串当前位的后一位，如果存在的话，就更新一下
        char c1 = A.charAt(0), c2 = B.charAt(0), c3 = 'a';
        if (B.length() > 1) {
            c3 = B.charAt(1);
        }
        // 和dp一样，后一位分为是 '*' 和不是 '*' 两种情况
        if (c3 != '*') {
            // 如果该位字符一样，或是正则串该位是 '.',也就是能匹配任意字符，就可以往后走
            if (c1 == c2 || c2 == '.') {
                return isMatch(A.substring(1), B.substring(1));
            } else {
                // 否则不匹配
                return false;
            }
        } else {
            // 如果该位字符一样，或是正则串该位是 '.'，和dp一样，有看和不看两种情况
            if (c1 == c2 || c2 == '.') {
                return isMatch(A.substring(1), B) || isMatch(A, B.substring(2));
            } else {
                // 不一样，那么正则串这两位就废了，直接往后走
                return isMatch(A, B.substring(2));
            }
        }
    }
}
```

#### 即时再战

本题是hard，先看懂思路，再战先缓缓

### [剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

codetop 2+1=3

#### 首战寄

没有很好的思路

#### 官方-dp

> 官方的讲的不好，附上高赞网友的评价讲解吧。

我的一点理解： 在已有的丑数序列上每一个数都必须乘2， 乘3， 乘5， 这样才不会漏掉某些丑数。假设已有的丑数序列为[1, 2, 3, ..., n1, n2], 如果单纯的让每个丑数乘2， 乘3， 乘5顺序排列的话肯定会有问题，

比如如果按照这样的顺序排列下去肯定有问题`[1*2, 1*3, 1*5, 2*2, 2*3, 2*5, 3*2, 3*3, 3*5, ... , n1 *2, n1 * 3, n1 * 5, n2 * 2, n3* 3, n2 * 5]`，因为后面乘2的数据可能会比前面乘3乘5的数据要小，那这个乘2的数应该排在他们的前面， 后面乘3的数据也可能比前面乘5的数据要小，那这个乘3的数应该排在他们的前面。

那怎么办呢，每个数都必须乘2， 乘3， 乘5这样才能保证求出所有的丑数，而且还要保证丑数的顺序，这个改如何同时实现呢？

通过观察网上的各个题解，终于找到了办法，那就是记录每个丑数是否已经被乘2， 乘3， 乘5了， 具体的做法是

设置3个索引a, b, c，分别记录前几个数已经被乘2， 乘3， 乘5了，比如a表示前(a-1)个数都已经乘过一次2了，下次应该乘2的是第a个数；b表示前(b-1)个数都已经乘过一次3了，下次应该乘3的是第b个数；c表示前(c-1)个数都已经乘过一次5了，下次应该乘5的是第c个数；

对于某个状态下的丑数序列，我们知道此时第a个数还没有乘2(有没有乘3或者乘5不知道）， 第b个数还没有乘3(有没有乘2或者乘5不知道），第c个数还没有乘5(有没有乘2或者乘3不知道), 下一个丑数一定是从第a丑数乘2， 第b个数乘3， 第c个数乘5中获得，他们三者最小的那个就是下个丑数。

求得下个丑数后就得判断这个丑数是谁，是某个数通过乘2得到的，还是某个数乘3得到的，又或是说某个数通过乘5得到的。我们可以比较一下这个新的丑数等于究竟是等于第a个丑数乘2, 还是第b个数乘3， 还是第c个数乘5， 通过比较我们肯定可以知道这个新的丑数到底是哪个数通过乘哪个数得到的。假设这个新的丑数是通过第a个数乘2得到的，说明此时第a个数已经通过乘2得到了一个新的丑数，那下个通过乘2得到一个新的丑数的数应该是第(a+1)个数，此时我们可以说前 a 个数都已经乘过一次2了，下次应该乘2的是第 （a+1） 个数, 所以a++；如果新的丑数是通过第b个数乘3得到的, 说明此时第 b个数已经通过乘3得到了一个新的丑数，那下个需要通过乘3得到一个新的丑数的数应该是第(b+1)个数，此时我们可以说前 b 个数都已经乘过一次3了，下次应该乘3的是第 （b+1） 个数, 所以 b++；同理，如果这个这个新的丑数是通过第c个数乘5得到的, 那么c++;

但是注意，如果第a个数乘2后等于第b个数乘3，或者等于第c个数乘5， 说明这个新的丑数是有两种或者三种方式可以得到，这时应该给得到这个新丑数的组合对应的索引都加一，比如新丑数是第a个数乘2后和第b个数乘3得到的，那么 a 和 b都应该加一， 因为此时第a个数已经通过乘2得到了一个新的丑数，第b个数已经通过乘3得到了一个新的丑数, 只不过这两个数相等而已。所以我们给计数器加一的时候不能使用 if else else if， 而应该使用if, if, if, 这样才不会把应该加一的计数器漏掉

经过n次循环，就能得到第n 个丑数了。

代码：

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];  // 使用dp数组来存储丑数序列
        dp[0] = 1;  // dp[0]已知为1
        int a = 0, b = 0, c = 0;    // 下个应该通过乘2来获得新丑数的数据是第a个， 同理b, c

        for(int i = 1; i < n; i++){
            // 第a丑数个数需要通过乘2来得到下个丑数，第b丑数个数需要通过乘2(我：这应该是作者笔误，应该是乘3)来得到下个丑数，同理第c个数
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;
            dp[i] = Math.min(Math.min(n2, n3), n5);
            if(dp[i] == n2){
                a++; // 第a个数已经通过乘2得到了一个新的丑数，那下个需要通过乘2得到一个新的丑数的数应该是第(a+1)个数
            }
            if(dp[i] == n3){
                b++; // 第 b个数已经通过乘3得到了一个新的丑数，那下个需要通过乘3得到一个新的丑数的数应该是第(b+1)个数
            }
            if(dp[i] == n5){
                c++; // 第 c个数已经通过乘5得到了一个新的丑数，那下个需要通过乘5得到一个新的丑数的数应该是第(c+1)个数
            }
        }
        return dp[n-1];
    }
}
```

- 20230201我：a指向的是一个丑数，并且尝试将该丑数乘以2来构建新丑数；bc同理。

#### 即时再战成功

```java
class Solution {
    public int nthUglyNumber(int n) {
        /**
        动态规定思想：每个丑数都是由一个更小丑数*三个基本丑数（2 3 5）得到的
        较小丑数可以乘基本丑数得到新丑数，如果想要得到有序的丑数生成序列，就得时刻比较乘2或3或5得到的丑数的数的大小，把较小的放在当前位置
         */
        //处理特殊情况
        if(n==1)return 1;

        //三个指针都从dp数组的index=0出发，分别乘2 3 5，得到新丑数，并放入记忆数组（实现动态规划）
        int a=0,b=0,c=0;
        //n不超过1690，应该是对应了整数边界，不然本函数无法返回
        int []res=new int[n];
        res[0]=1;
        //从第二个丑数开始处理
        for(int i=1;i<n;i++){
            int atemp=res[a]*2;
            int btemp=res[b]*3;
            int ctemp=res[c]*5;
            //abc三个指针求得的新丑数中，最小的那个作为新丑数，同时把该指针右移一位，准备给下一个整数做*2或3或5的操作。如果两个指针求到的新丑数相同，说明两个指针都得++，所以不能用ifelse而得用三个if
            if(atemp<=btemp&&atemp<=ctemp){
                res[i]=atemp;
                a++;
            }
            if(btemp<=atemp&&btemp<=ctemp){
                res[i]=btemp;
                b++;
            }
            if(ctemp<=atemp&&ctemp<=btemp){
                res[i]=ctemp;
                c++;
            }
        }

        return res[n-1];

    }
}
```

- 反思：我的再战可以参考官方哪个两个Math.min嵌套判断谁最小，这样避免对dp[i]的同一位置多次赋值同一值。

### [剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

codetop=3

#### 首战寄

概率论全忘了，哎

#### 官方-暴力

![image-20220422160224067](lcof.assets/image-20220422160224067.png)

![Picture1.png](lcof.assets/1615223242-EMOnIR-Picture1.png)

![image-20220422161242489](lcof.assets/image-20220422161242489.png)



#### 官方-dp

![image-20220422162715568](lcof.assets/image-20220422162715568.png)

![Picture2.png](lcof.assets/1614960989-tpJNRQ-Picture2.png)

- 我：f(n)中第一个元素是f(2,2)的概率，即两个骰子的点数和为2的情况；不存在两个骰子的点数和为1的情况，所以数组第一位为点数和为2的概率；加粗的6/36也就是f(2,7)了。
- 我：(1/6)\*(1/6)+(1/6)\*(1/6)+...六个积相加...+(1/6)*(1/6)=6/36
  - (1/6)\*(1/6)中，左边的1/6是f(1,7-i)的概率，即一个骰子各个面朝上的概率；右边的(1/6)是第二颗骰子投相应互补点数的概率（比如要两个骰子的和为7，那么第一个骰子投了1，第二个骰子就得为6）。

![image-20220422163035641](lcof.assets/image-20220422163035641.png)

![Picture3.png](lcof.assets/1614960989-mMonMs-Picture3.png)

- 我：正向递推中，f(n-1)的加粗的1/6，可以理解为投一个筛子得到2的概率；f(n)的加粗的2/36，可以理解为投两个筛子得到3的概率；
- 我：正向递推中，f(n-1)的加粗的1/6无法贡献给f(n)的第一个元素，因为，投两个筛子的点数和最少为2，第一个骰子点数为2的话是无法贡献给两个骰子和为2的情况的。

![image-20220422165153903](lcof.assets/image-20220422165153903.png)

复杂度分析：

![image-20220422165235524](lcof.assets/image-20220422165235524.png)

代码：

![image-20220422165334994](lcof.assets/image-20220422165334994.png)

```java
public double[] dicesProbability(int n) {
        //因为最后的结果只与前一个动态转移数组有关，所以这里只需要设置一个一维的动态转移数组
        //原本dp[i][j]表示的是前i个骰子的点数之和为j的概率，现在只需要最后的状态的数组，所以就只用一个一维数组dp[j]表示n个骰子下每个结果的概率。
        //初始是1个骰子情况下的点数之和情况，就只有6个结果，所以用dp的初始化的size是6个
        double[] dp = new double[6];
        //只有一个数组
        Arrays.fill(dp,1.0/6.0);
        //从第2个骰子开始，这里n表示n个骰子，先从第二个的情况算起，然后再逐步求3个、4个···n个的情况
        //i表示当总共i个骰子时的结果
        for(int i=2;i<=n;i++){
        //每次的点数之和范围会有点变化，点数之和的值最大是i*6，最小是i*1，i之前的结果值是不会出现的；
        //比如i=3个骰子时，最小就是3了，不可能是2和1，所以点数之和的值的个数是6*i-(i-1)，化简：5*i+1
            //当有i个骰子时的点数之和的值数组先假定是temp
            double[] temp = new double[5*i+1];
            //从i-1个骰子的点数之和的值数组入手，计算i个骰子的点数之和数组的值
            //先拿i-1个骰子的点数之和数组的第j个值，它所影响的是i个骰子时的temp[j+k]的值
            for(int j=0;j<dp.length;j++){
            //比如只有1个骰子时，dp[1]是代表当骰子点数之和为2时的概率，它会对当有2个骰子时的点数之和为3、4、5、6、7、8产生影响，因为当有一个骰子的值为2时，另一个骰子的值可以为1~6，产生的点数之和相应的就是3~8；比如dp[2]代表点数之和为3，它会对有2个骰子时的点数之和为4、5、6、7、8、9产生影响；所以k在这里就是对应着第i个骰子出现时可能出现六种情况，这里可能画一个K神那样的动态规划逆推的图就好理解很多
                for(int k=0;k<6;k++){
                    //这里记得是加上dp数组值与1/6的乘积，1/6是第i个骰子投出某个值的概率
                    temp[j+k]+=dp[j]*(1.0/6.0);
                }
            }
            //i个骰子的点数之和全都算出来后，要将temp数组移交给dp数组，dp数组就会代表i个骰子时的可能出现的点数之和的概率；用于计算i+1个骰子时的点数之和的概率
            dp = temp;
        }
        return dp;
    }   
```

#### 即时再战成功

理解的不是100%清晰，不过能有思路并做出来；本题写出来有一定背题的因素。

```java
class Solution {
    public double[] dicesProbability(int n) {
        /**
        设dp[]为n个骰子时，“点数和”对应的概率数组，那么n-1个骰子的对应数组为dppre[]
        dppre[i]在新增骰子的值分别为1 2 3 4 5 6 时，分别根据自己的“点数和”更新dp[]
         */

        //初始化dp数组，对应的是一个骰子时，各个“点数和”对应的概率：投出1的概率是1/6，，，投出6的概率是1/6
        double[]dp=new double[]{1.0/6.0,1.0/6.0,1.0/6.0,1.0/6.0,1.0/6.0,1.0/6.0};

        //动态规划，不断接近目标的n个骰子的情况
        for(int i=2;i<=n;i++){
            //新建temp数组，这个数组是加一颗骰子后对应的各个“点数和”的概率。数组的大小==可能的点数和的个数==6*i-1*i+1
            double[] temp=new double[5*i+1];

            //把dp[]的每个元素，针对新增骰子投出为1 2 3 4 5 6 的情况，把自己的的概率贡献添加到temp中
            for(int j=0;j<dp.length;j++){
                //针对新增骰子投出为1 2 3 4 5 6 的情况，把自己的的概率贡献添加到temp中
                for(int k=0;k<6;k++){
                    temp[j+k]+=dp[j]*1.0/6.0;
                }
                
            }

            //i个骰子的dp计算完毕后，用temp更新dp，准备求i+1个骰子的情况
            dp=temp;
        }

        return dp;
    }
}
```

### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

#### 首战寄

- 我：[0,n]的最长严格递增子序列，可以由[0,n-1]的最长严格递增子序列推得。可以考虑使用二维数组，用`num[i][j]`表示从第i个元素到第j个元素的最长严格递增序列的长度。核心是[0,n-1]的最长严格递增子序列的最后一位，最好要比第n位的数据小，这样才能延长序列。
- 我：题目要求nlogn的时间复杂度（logn是二分查找的时间复杂度），结合题目找的是递增，可以往二分查找的思路去想。

没啥思路

#### 大佬解法一-动态规划

解题思路：

![image-20230129165431674](lcof.assets/image-20230129165431674.png)

复杂度分析：

![image-20230129165453071](lcof.assets/image-20230129165453071.png)

代码：

```java
// Dynamic programming.
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0) return 0;
        int[] dp = new int[nums.length];
        int res = 0;
        Arrays.fill(dp, 1);
        for(int i = 0; i < nums.length; i++) {
            for(int j = 0; j < i; j++) {
                if(nums[j] < nums[i]) dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```

- 我：推荐用的大佬的方案1。其实直觉就能想到这个方案，但是因为时间复杂度是on2，所以放弃了；但是要记住dp不意味着时间复杂度一定低，一些题就算用dp也得on2的时间复杂度。

#### [大佬解法二：动态规划 + 二分查找](https://leetcode.cn/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/)

这个可以用二分查找把时间复杂度优化到onlogn，但是感觉重写比较困难，暂略。想看可以点击本标题的链接去看。

#### 即时再战成功

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        /**
        这种长度相关的题，就可以往dp去想。因为当前长度是由“过去长度和当前元素”所决定的。
        针对nums[i]都遍历一遍nums[0]-nums[i-1];来构建dp序列
         */
        
        //处理特殊情况
        if(nums.length==0) return 0;
        
        //初始化用于保存最长长度的变量；因为走到这表示数组至少有一个元素，那最长长度是从1开始的
        int maxL=1;
        
        //初始化动态规划中用于保存过往状态的数组。因为nums每个元素长度为1，所以初始化每个dp的元素值就是1。
        int[] dp= new int[nums.length];
        for(int i=0;i<dp.length;i++)dp[i]=1;

        //获取每个dp[i]
        for(int i=0;i<nums.length;i++){
            //获取[0,i)的nums值和对应的dp值，用来尝试更新dp
            for(int j=0;j<i;j++){
                //如果nums[j]<nums[i]，那么dp[j]+1就是dp[i]；如果新的dpi比旧的dpi大，就更新为当前dpi
                if(nums[j]<nums[i]) {
                    dp[i]=Math.max(dp[i],dp[j]+1);
                    //只要dpi有可能被更新，就和历史最大dpi相比较
                    maxL=Math.max(maxL,dp[i]);
                }
            }
        }
        
        return maxL;
    }
}
```

### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

困难

#### 首战寄

无好的思路，困难题。。

#### 大佬-动态规划

> 思路：

`dp[i][j]` 代表 `word1` 前 `i `个字母转换成` word2 `前` j `个字母需要最少步数

所以，

当 `word1[i] == word2[j]`，`dp[i][j] = dp[i-1][j-1]`；

当 `word1[i] != word2[j]`，`dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1`

其中，`dp[i-1][j-1]` 表示替换操作，`dp[i-1][j]` 表示删除操作，`dp[i][j-1]` 表示插入操作。

- 对`dp[i-1][j-1] 表示替换操作，dp[i-1][j] 表示删除操作，dp[i][j-1] 表示插入操作。`的补充理解：

  ```
  以 word1 为 "horse"，word2 为 "ros"，且 dp[5][3] 为例，即要将 word1的前 5 个字符转换为 word2的前 3 个字符，也就是将 horse 转换为 ros，因此有：
  
  (1) dp[i-1][j-1]，即先将 word1 的前 4 个字符 hors 转换为 word2 的前 2 个字符 ro，!!!然后将第五个字符 word1[4]（因为下标基数以 0 开始） 由 e 替换为 s（即替换为 word2 的第三个字符，word2[2]）
  
  (2) dp[i][j-1]，即先将 word1 的前 5 个字符 horse 转换为 word2 的前 2 个字符 ro，!!!然后在末尾补充一个 s，即插入操作
  
  (3) dp[i-1][j]，即先将 word1 的前 4 个字符 hors 转换为 word2 的前 3 个字符 ros，!!!然后删除 word1 的第 5 个字符
  ```

  - 我：上面文段中"!!!然后"的操作，就代表了表达式中min后`+1`的操作。

注意，针对第一行，第一列要单独考虑，我们引入 `''` 下图所示：

![Snipaste_2019-05-29_15-28-02.png](lcof.assets/76574ab7ff2877d63b80a2d4f8496fab3c441065552edc562f62d5809e75e97e-Snipaste_2019-05-29_15-28-02.png)

第一行，是 `word1 `为空变成` word2` 最少步数，就是插入操作

第一列，是 `word1`变成空的`word2` 需要的最少步数，就是删除操作

> 解释为什么要引入空元素，即dp必须使n+1的大小

```
  r 
h 1
o 2
r 3
```

如上面的例子，如果不引入空字符串的话，word1到word2递推的话，r行对应的不该是3，而应该是2（删除ho）；这时候第一列和第一行的更新逻辑也不能一味+1了，也得参考当前对应值是否相等。

修改代码为如下：

```
class Solution {
    public int minDistance(String word1, String word2) {
        /**
        `dp[i][j]` 代表 `word1` 到 `i `位置转换成` word2 `到` j `位置需要最少步数；位置即index，范围为[0,String.length()-1]
         */
        int n1 = word1.length();
        int n2 = word2.length();
        //处理特殊空字符串的特殊情况
        if(n1==0||n2==0) return n1==0?n2:n1;

        int[][] dp = new int[n1][n2];
        //最左上角的元素
        dp[0][0]=word1.charAt(0)==word2.charAt(0)?0:1;//如果
        // 第一行
        for (int j = 1; j <n2; j++) {
            if(word1.charAt(0)==word2.charAt(j)){
                dp[0][j] = dp[0][j - 1];
            }else{
                dp[0][j]=dp[0][j - 1]+1;
            }
        }
        // 第一列
        for (int i = 1; i <n1; i++) {
            if(word1.charAt(i)==word2.charAt(0)){
                dp[i][0] = dp[i-1][0];
            }else{
                dp[i][0]=dp[i-1][0]+1;
            }
        }

        for (int i = 1; i < n1; i++) {
            for (int j = 1; j < n2; j++) {
                if (word1.charAt(i) == word2.charAt(j)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
            }
        }
        return dp[n1-1][n2-1];  
    }
}

```

这个代码通过了1143 / 1146个记录，但是无法通过下面这个案例：

```
"pneumonoultramicroscopicsilicovolcanoconiosis"
"ultramicroscopically"
```

- 问问问：为什么无法通过？

所以还是老实按照官方的思路，构建dp为前n个位置的转移，而不是index n及之前的转移。

> 代码-自底向上:

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n1 = word1.length();
        int n2 = word2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        // 第一行
        for (int j = 1; j <= n2; j++) dp[0][j] = dp[0][j - 1] + 1;
        // 第一列
        for (int i = 1; i <= n1; i++) dp[i][0] = dp[i - 1][0] + 1;

        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i][j - 1]), dp[i - 1][j]) + 1;
            }
        }
        return dp[n1][n2];  
    }
}
```

- 注意题目要求里有说：字符串长度可能为0，即可能输入空字符串。





## 双指针(简单)

### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

codetop==4

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

codetop==85

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

- 问答问：我n层递归，空间复杂度是不是就是on了，那性能就不如官方的了。
  - 20230201我：内存确实是on，不如官方

- 我：看到倒序，就可以想到栈后进先出的原则，也能实现倒序。本题也可以把节点都压入Deque，然后弹出的第k个节点就是目标节点。
  - 我：看到倒序也可以像官方那样用双指针控制倒数的距离。


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

- 本题没有 k 大于链表长度的 case ，因此不用考虑越界问题。

- 我：看来双指针可以这样控制距离，牛逼！



### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

codetop==6

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

codetop==18

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

codetop==29

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

考虑定义双指针 i , j 分列数组左右两端，循环执行：

1. 指针 i 从左向右寻找偶数；
2. 指针 j 从右向左寻找奇数；
3. 将 偶数 nums[i]和 奇数 nums[j] 交换。

可始终保证： 指针 i 左边都是奇数，指针 j 右边都是偶数 。

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
            //我：走到这里就意味着“i位置不是奇数，j位置不是偶数”，需要交换两者的值
            tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```

- 我：本代码左右同时动，我是一个去找另一个，时间复杂度一样；我的代码没有双循环，甚至更优雅些。
  - 我：我的方法和大佬的方法，都需要遍历所有元素，时间复杂度都是on。

- 网友：本题蕴含快速排序的思想！

### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

codetop==2

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

codetop==5

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



### [15. 三数之和](https://leetcode.cn/problems/3sum/)

#### 首战寄

没什么好思路

#### 官方-排序+双指针

解题思路：

![image-20220509224340355](lcof.assets/image-20220509224340355.png)

时间复杂度分析：

![image-20220509224356038](lcof.assets/image-20220509224356038.png)

- 我：由于“时间复杂度是总运算次数表达式中受n的变化影响最大的那一项(不含系数)”，所以快排占用的onlogn的时间复杂度被双指针排序时的N^2排挤忽略了，所以总的时间复杂度是N^2。

代码：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);//排序，nums变成递增数组
        List<List<Integer>> res = new ArrayList<>();
        //k < nums.length - 2是为了保证后面还能存在两个数字i j
        for(int k = 0; k < nums.length - 2; k++){
            if(nums[k] > 0) break;//若nums[k]大于0，则后面的数字也是大于零（排序后是递增的）
            if(k > 0 && nums[k] == nums[k - 1]) continue;//nums[k]值重复了，去重
            int i = k + 1, j = nums.length - 1;//定义左右指针
            while(i < j){
                int sum = nums[k] + nums[i] + nums[j];
                if(sum < 0){
                    while(i < j && nums[i] == nums[++i]);//左指针前进并去重
                } else if (sum > 0) {
                    while(i < j && nums[j] == nums[--j]);//右指针后退并去重
                } else {
                    res.add(new ArrayList<Integer>(Arrays.asList(nums[k], nums[i], nums[j])));
                    while(i < j && nums[i] == nums[++i]);//左指针前进并去重
                    while(i < j && nums[j] == nums[--j]);//右指针后退并去重
                }
            }
        }
        return res;
    }
}
```

- 我：尝试`Arrays.sort(nums,(o1,o2)->(o1-o2));`显式指定升序排序会报错`Arrays.<T#1>sort(T#1[],Comparator<? super T#1>) is not applicable (inference`。
  - 原因：[参考](https://stackoverflow.com/questions/63963247/no-suitable-method-found-for-sort)，`public static <T> void Arrays.sort(T[] a, Comparator<? super T> c)`。You can't sort `int[]` array with comparator. Only `Integer[]`. So, your snippet could be rewritten with:`Arrays.sort(Arrays.stream(intIdx).boxed().toArray(), (a, b) -> intervals[a][0] - intervals[b][0]);`。即不能对int[]使用Comparator来排序，必须要对Integer！
  - 修改：对基本类型数组的排序可以[参考](http://t.csdn.cn/GhGNP)

#### 即时再战成功

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        /**
        由于不能包括重复的三元组，所以可以排序数组来解决重复元素的问题（碰到重复的元素就跳过）

        固定一个元素，另外两个元素双指针往中间去找合适的三元组
         */
        List<List<Integer>> res=new LinkedList<>();
        
        //排序数组，默认是升序的
        Arrays.sort(nums);

        //数组用三指针 i j k；遍历i，使用双指针找j k；因为i后面有jk，所以i最大为nums.length-3
        for(int i=0;i<nums.length-2;i++){
            //如果nums[i]>0，那么肯定不存在nums[i]+nums[j]+nums[k]==0。避免重复计算。
            if(nums[i]>0)break;
            //避免处理相同的i，从而避免了重复的三元组
            if(i>0&&nums[i]==nums[i-1])continue;

            //双指针查找针对当前i合适的三元组
            int j=i+1,k=nums.length-1;
            while(j<k){
                int sum=nums[i]+nums[j]+nums[k];
                if(sum>0){//如果sum>0，那么左移k，并且相同的k都要被移
                    do{
                        k--;
                    }while(j<k&&nums[k]==nums[k+1]);//!!!!注意，dowhile的while后面要加上";"
                }else if(sum<0){//如果sum<0，那么右移j，并且相同的j都要被移
                    do{
                        j++;
                    }while(j<k&&nums[j]==nums[j-1]);
                }else{//如果sum=0，那么记录此时三个指针指向的元素。ij移动，且相同的ij都要被移
                    res.add(new LinkedList(Arrays.asList(nums[i],nums[j],nums[k])));//这里的第二层列表用LinkedList或ArrayList都可以。
                    do{
                        k--;
                    }while(j<k&&nums[k]==nums[k+1]);
                    do{
                        j++;
                    }while(j<k&&nums[j]==nums[j-1]);

                }
            }

        }

        return res;

    }
}
```

- 我：无论何时使用Arrays.asList，建议都用LinkedList或ArrayList包裹一层再使用；因为Arrays.asList返回的结果不是真正的list而是一个内部类。

### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

和剑指25相同

#### 首战告捷

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        /**
        双指针分别指向两个链表进行合并
         */

        ListNode l1=list1,l2=list2;
        ListNode dummyHead=new ListNode(-1);
        ListNode cur=dummyHead;

        //合并两个链表，直到一个链表完全进入结果链表
        while(l1!=null&&l2!=null){
            if(l1.val<l2.val){
                cur.next=l1;

                cur=cur.next;
                l1=l1.next;
            }else{
                cur.next=l2;

                cur=cur.next;
                l2=l2.next; 
            }
        }

        //如果l1还有剩余，把l1放入结果链表
        while(l1!=null){
            cur.next=l1;

            cur=cur.next;
            l1=l1.next;
        }
        //如果l2还有剩余，把l2放入结果链表
        while(l2!=null){
            cur.next=l2;

            cur=cur.next;
            l2=l2.next;
        }

        return dummyHead.next;

    }
}
```

### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

#### 首战寄

代码：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        /**
        排序数组后用双指针

        时间复杂度应该是ologn，以快排的时间复杂度为准
         */
        Arrays.sort(nums);

        int left=0,right=nums.length-1;

        //至少有两个数的时候才比较
        while(left<right){
            int temp=nums[left]+nums[right];
            if(temp<target){
                left++;
                //把相同的数也跳过。不需要考虑left<right，因为本题总有输出
                while(left>0&&nums[left]==nums[left-1])left++;
            }else if(temp>target){
                right--;
                //把相同的数也跳过
                while((right<nums.length-1)&&nums[right]==nums[right+1])right--;
            }else{
                //找到目标，直接构建数组+数组内容并返回！！！错误，这么返回的是排序后的坐标，又因为可能有重复元素所以用hashmap记录坐标只会记录最后的那个数字，像[3,3]这种情况就无法处理。
                return new int[]{left,right};
            }
        }

        //这个返回值永远不会使用
        return new int[0];
    }
}
```

- 三数之和返回的是元素，所以可以排序；但是这里需要返回坐标，坐标在排序后就丢失了，用hash记录坐标和值又存在一个值对应多个坐标的问题。所以没有什么好思路，用暴力解没什么意思。
  - 可以先复制一份原数组，然后拿着求得的两个元素回到原序的复制数组里，找到对应的index（如果index2==index1，说明拿到的两个元素相等，则index2放弃当前index找下一个index）

- 我：hash法一次性存储确实会导致index覆盖，但是如果是边处理边存储，就不会覆盖了。

#### 官方1-排序后折半查找

[链接](https://leetcode.cn/problems/two-sum/solution/liang-shu-zhi-he-de-si-chong-jie-fa-pai-6vatw/)

算法描述：

- 先将元素nums拷贝一份为copiedNums，然后将原数组排序，接着遍历排序后的数组，以遍历到的数字之后的数列执行折半查找，查找目标为target - nums[i]。因为原数组排序后丢失原下标信息，因此执行折半查找得到n0和n1(n0 + n1 = target)后，再遍历两次copiedNums分别得到n0和n1在copiedNums中的下标(原下标)。排序和折半查找可以调用Arrays静态方法Arrays.sort()和Arrays.binarySearch()，也可以自己实现。代码示例中排序用Arrays.sort()，折半查找用自己实现的版本。

复杂度：

- 排序耗时O(nlogn)(假设采用O(nlogn)的排序算法)，拷贝原数组耗时O(n)，排序后的折半查找耗时为O(logn)，最后找原下标的两个for均耗时O(n)，所以总体时间复杂度为O(nlogn)。

- 空间复杂度为O(n)。

代码：

```java
public int[] twoSumSortBinarySearch(int[] nums, int target) {
    int[] res = new int[2];
    int[] resVal = new int[2];
    int[] copiedNums = Arrays.copyOf(nums, nums.length);
    Arrays.sort(nums);
    int n1 = -1;
    for (int i = 0; i < nums.length; i++) {
        resVal[0] = nums[i];
        resVal[1] = target - resVal[0];
        // 也可以用Arrays自带的折半查找方法Arrays.binarySearch()，
        // 但要注意判断返回值的地方要做相应修改。
        // n1 = Arrays.binarySearch(nums, i + 1, nums.length -1 , resVal[1]);
        n1 = binarySearchBasic(nums, i + 1, resVal[1]);
        if(n1 != -1) {
            break;
        }
    }
    if(n1 == -1) {
        return new int[] {};
    }
    for (int j = 0; j < copiedNums.length; j++) {
        if(copiedNums[j] == resVal[0]) {
            res[0] = j;
            break;
        }
    }
    for (int k = 0; k < copiedNums.length; k++) {
        // 注意不能是同一个元素，需加上 k != res[0] 条件
        if(copiedNums[k] == resVal[1] && k != res[0]) {
            res[1] = k;
            break;
        }
    }
    return res;
}

private int binarySearchBasic(int[] arr, int low, int target) {
    int high = arr.length - 1;
    while(low <= high) {
        int center = (low + high) / 2;
        if(target == arr[center]) {
            return center;
        }
        else if(target < arr[center]) {
            high = center - 1;
        }
        else {
            low = center + 1;
        }
    }
    return -1;
}
```

- 高赞网友：

  - 有序：对撞指针或二分

  - 无序：哈希表

#### （我推荐）官方2-哈希表

思路及算法：

![image-20220512191245196](lcof.assets/image-20220512191245196.png)

- 方法一即on^2的暴力方法

代码：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}
```

- 我：因为是两数之和，在遍历到数组的第二个数的时候，第一个数必定已经被放入hashmap；遍历到第一个数的时候，由于第二个数没有被放入hashmap，所以只是把第一个数放入hashmap，这样遍历到第二个数的时候就可以找到第一个数了。

#### 即时再战成功

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        /**
        使用hashmap记录元素和index。target有两个整数相加得到，当遍历到第二个整数时，第一个整数已经在hashmap中了。

        不用担心重复元素，因为任何一个重复元素的坐标都符合题意
         */
        Map<Integer,Integer> mymap=new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(mymap.containsKey(target-nums[i])){
                return new int[]{mymap.get(target-nums[i]),i};
            }

            //添加或更新hashmap的键值对
            mymap.put(nums[i],i);
        }

        //这个return不会被使用
        return new int[0];
    }
}
```



### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

- 注意：本题“非递减顺序排列”，表示`可重复的递增`，参考](http://t.csdn.cn/uAtq0)

#### 首战寄

题目推荐用O(m+n)来实现，没有很好的思路。

我本来是想两个指针，把前m和n两个数组里最小的前m个数放到nums1的前m个数字处，然后把nums2中有序的n个数挪到nums1的末尾；但是发现没法实现。

#### 官方-双指针

解题思路：

1. 几个月后重刷此题，感觉还是有点进步的
2. 思路的重点一个是从后往前确定两组中该用哪个数字
3. 另一个是结束条件以第二个数组全都插入进去为止

画图理解：

![image-20220515214934953](lcof.assets/image-20220515214934953.png)

![image-20220515214946356](lcof.assets/image-20220515214946356.png)

![image-20220515215007979](lcof.assets/image-20220515215007979.png)

![image-20220515215028186](lcof.assets/image-20220515215028186.png)

![image-20220515215045421](lcof.assets/image-20220515215045421.png)

![image-20220515215057051](lcof.assets/image-20220515215057051.png)

![image-20220515215107875](lcof.assets/image-20220515215107875.png)

![image-20220515215119011](lcof.assets/image-20220515215119011.png)

代码：

```java
/** 
     楼主的解法，比官方的逆向双指针解法更高效  -- nums2都塞到nums1之后，就不用再继续处理了。<br/>
     但是“交换”感觉没有  “比较两个数组，选择一个最大的塞到nums1的尾部”  好理解。<br/>
     下面[@秋明杉](/u/qiu-ming-shan-w/) 的if else去掉了内层while循环，感觉又简化了一些.不过nums1[(m+n+1)]的写法不如单独定义一个i变量 <br/>
     又参考了下面 @Quaint 的 nums1[--i]，替换掉nums[i--]
  */
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
         int i = nums1.length ;

        while (n > 0) {
            if (m > 0 && nums1[m-1] > nums2[n-1]) {
                nums1[--i] = nums1[--m]; 
                //替代swap，参考官方题解“逆向双指针解法”的公式
            }else{
                nums1[--i] = nums2[--n]; 
                //替代swap，参考官方题解“逆向双指针解法”的公式
            }
          
        }
    }
    
}
```

- 我：nums1数组的后部分是空的，所以可以直接覆盖不影响结果。
- 我：i和m之前相差的数总是n的值；所以不用担心在nums1的后面的i位置填入值时，会覆盖到m位置及之前的值；因为i和m重合时，说明m==0，那么也就没有数需要填了，也就不会被覆盖！！

我：复杂度：

- 空间复杂度：O(1)。只有变量i占用了额外的空间。
- 时间复杂度：O(m+n)。最多把m+n个数复制到nums1的末尾的i处

#### 即时再战成功

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        /**
        因为nuns1数组的后面是空的，所以可以把nums1和nums2的数值区最后的的较大的数放入nums1的0区，然后指针前移动
        因为m和nums1.length的差别就是n，所以n为0的时候表示nums2完全合并进了nums1
         */
        
        //因为m和n都是给定的长度，所以需要减一后才是真正的坐标；因此i也设置为-1后才为真正坐标的值；那么使用坐标时就得先“减”再用
        int i=nums1.length;

        while(n>=1){//n最小为1，表示允许处理的nums2的最小坐标为0
            /**
            如果nums1的待排序数的最后一个大于nums2的待排序数的最后一个。
            
            ！！注意while进来时只控制了n>=1，没控制m是否>=1，所以如果m=0，即nums1的待排序数为0，但是nums2待排序树不为0的话，尝试找nums1[m-1]会导致超过数组边界。所以要先判断m是否>=1，如果不是则直接把nums2的n处的值移入i。
             */
            if(m>=1&&nums1[m-1]>nums2[n-1]){
                nums1[--i]=nums1[--m];
            }else{
                nums1[--i]=nums2[--n];
            }
        }

    }
}
```

### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

和剑指52相同

#### 首战成功

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
        你走过我来时的路！！！
        
        因为skipA和akipB不一定相等，所以一条链表走完后要走另一条链表，让两者路径相等，这样才有机会让两个指针相交于同一个节点
         */
        //如果没有找到res就返回null。这里要强制初始化为null，否则编译不通过。。
        ListNode res=null;
        
        //初始化指针
        ListNode a=headA,b=headB;
        //两个指针同时为null的话说明遍历完毕了,只要有一个不是null就可以执行循环
        while(a!=null||b!=null){
            //当一个指针走到结尾，另一个指针在链表中时，就把当前指针指向另一条队列的队头
            if(a==null)a=headB;//不用担心移动的时候多走一步，因为另一个指针移动到头节点也会多走一步，就抵消了。
            if(b==null)b=headA;

            //走到这一步时，可以保证两个节点都不是空的
            
            //判断左右指针是否指向相等节点
            if(a==b){
                res=a;
                break;
            }

            //当前两个指针互不相等的话，移动指针
            a=a.next;
            b=b.next;

        }

        return res;
        
    }
}
```



## 排序(简单)

### [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

codetop==17

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
        //排序字符串数组//我：让升序；x+y<y+x,则x<y；那么如下比较，在x+y大于y+x（即x>y）时，就交换x和y的顺序，所以保证了x<y
        Arrays.sort(strs, (x, y) -> (x + y).compareTo(y + x));
        //合并字符串数组
        StringBuilder res = new StringBuilder();
        for(String s : strs)
            res.append(s);
        return res.toString();
    }
}
```

- Arrays.sort方法使用，[参考](https://blog.csdn.net/ted_cs/article/details/82713706)；本例用的是Comparator接口做外部比较器，并用lamda表达式表达了comparator接口。
  - 我看官方的sort接口是传入Comparator接口的实现类，我还是安装java官方的来吧，即自己传Comparator。
  - 我：字符串形式的数字大小比较,`A.compareTo(B)`的正负性，与`Integer.parseInt(A)-Integer.parseInt(B)`相同，[参考](http://t.csdn.cn/RBmoj)
  - 我：注意Arrays.sort(T[],Comparator)不能排序基本数据类型数组，比如int[]；想排序的话得转化为Integer数组再排序
    - Arrays.sort(T[])是可以排序基本数据类型数组的，但是只能是升序，不能通过Comparator降序。
  
- [Java中java.util.Comparator用法](https://www.cnblogs.com/XDU-Lakers/p/13985258.html)
  - 重写方法`public int compare(String o1, String o2) `，凡是返回1或者大于0的正数的时候就要交换位置
  - Comparator是一个函数式接口，所以能用lamda表达式表示。
    - 我：lamda表达式中`->`右边的数，应该就是函数式接口的唯一抽象函数的返回值

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
                //防止超过整数边界//20230203我：
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

codetop==17

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
- 我：快速排序Arrays.sort的时间复杂度是nlogn

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

- 我：这个算法意思是，在没重复元素（0除外）的情况下，max和min的差距小于5，就是顺子。

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

## 1111111排序(中等)

### [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

codetop==29

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

- 反省：忘记了数组切割的函数；Arrays.copyOf(arr, k)有截取数组的功能，k表示截取前k个数。
  - 20230203我：忘了这个用法就忘了吧，用for循环实现也好，毕竟copyof函数不常用，别用错了。


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
- 20230203我：虽然这个快排模板看起来简介，但是这个模板的可解释性不如我在“基础知识-排序”处的快排模板（我推荐使用），那个模板的挖坑填坑思想更容易记忆，那个模板解释了：
  - 为什么选left为哨兵后要先移动right处的指针
  - 最后为什么要把target放到双指针重合处

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

codetop==5

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
      - 20230203我：我还是喜欢Comparator一点，因为Comparator是外部排序器作为参数传入sort；这一点和Arrays.sort(nums,Comparator)一样；虽然list排序只需要传入Comparator，而不需要传入list本身。
    
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

- 我：针对我首战用的暴力做法来分析一下，插入的时间复杂度是nlogn（因为要快排），取数的时间复杂度是o1，综合时间复杂度是ologn。时间复杂度不如使用堆排序（即PQ）的方法！

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
  - Comparator自定义排序的话，返回-1表示o1和o2的顺序是默认的升序，即o1<o2，所以不用交换两者顺序；
    - 我：Comparator的抽象函数返回值大于0的话表示要交换x(即o1)和y（即o2）的顺序。比如本例，初始状态下x排在y的前面,但是Comparator的抽象函数返回的是y-x，那么y>x的时候（即y-x>0）会交换x和y的位置即让较大的y放在前面，这就实现了降序，在PQ里即为大顶堆。
    - 我：可以发现，构造函数传入的比较器，都是作为外部比较器的Comparator，并且基本都可以用lamda表达式来简化表达。

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

- 我：官方允许小顶堆多一个元素，但是其实也可以允许大顶堆多一个元素。这点已经被我在力扣验证。

- 错误点：

  - 求中位数时，误用poll取数，导致更改了堆影响后续操作；应该用peek。
  - poll的是堆顶，小顶堆是递增，那么poll出的就是最小值；反之大顶堆poll出来的就是最大值。

- 问答问：为什么都用了堆排序，我的效率比官方低这么多？（红框是我，绿框是官方）

  <img src="lcof.assets/image-20220409010404047.png" alt="image-20220409010404047" style="zoom:50%;" />

  - 经过使用官方的lamda表达式决定大顶堆，但是线效率没变，说明是其他部位导致的。

  - 自答：发现代码遗留了debug时设置的System.out.println()，删掉之后效率和官方一致；看来输出很耗时啊！

    <img src="lcof.assets/image-20220409010830512.png" alt="image-20220409010830512" style="zoom:50%;" />

### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

#### 首战寄

思路：把interval按照第一个元素升序，然后把interval内部数组两两合并

```
class Solution {
    public int[][] merge(int[][] intervals) {
        //把基本数据类型转化为包装类
        Arrays.sort(intervals,(o1,o2)->(o1[0]-o2[0]));//把二维数组转化为按照内维第一个元素升序排列
        //System.out.println(intervals[0][0]);
        List<int[]>res=new ArrayList<>();
        int i=0;
        while(i<intervals.length-1){//保证至少有两个元素没被处理
            //合并i和i+1的数组，直到无法再合并
            while(i<intervals.length-1){
                if(intervals[i][1]<=intervals[i+1][0]){//合并两者
                    intervals[i+1][0]=intervals[i][0];
                    intervals[i+1][1]=Math.max(intervals[i][1],intervals[i+1][1]);
                    i++;
                }else{
                    //当前i位置对应的数组可以放入res
                    res.add(intervals[i]);
                    i++;
                    break;//不能合并就退出当前死循环
                }
            }
            
        }
        int[][] res1=new int[res.size()][2];
        for(int j=0;j<res.size();j++){
            res1[j][0]=res.get(j)[0];
            res1[j][1]=res.get(j)[1];
        }
        return res1;
    }
}
```

- 我表扬：能自定义排序。虽然基本数据类型数组`int[]`是不能排序，但是`int[][]`是可以自定义排序的，因为`int[][]`的元素是`int[]`并不是基础数据类型。
- 我反思：结尾尝试用`return res.toArray(new int[0])`来直接返回，但是toArray输入的应该是最终的格式即`new int[0][]`；toArray中输入的数组不需要体现完整的长度。
- 我反思：我这里合并的思路是混乱的，官方解答的合并思路很清晰；看官方的吧。

测试的有问题

```
输入
[[8,10],[2,6],[1,3],[15,18]]
输出
[[1,3]]
预期结果
[[1,6],[8,10],[15,18]]
```

#### 官方-排序

> 思路

如果我们按照区间的左端点排序，那么在排完序的列表中，可以合并的区间一定是连续的。如下图所示，标记为蓝色、黄色和绿色的区间分别可以合并成一个大区间，它们在排完序的列表中是连续的：

![56-2.png](lcof.assets/50417462969bd13230276c0847726c0909873d22135775ef4022e806475d763e-56-2.png)

> 算法

我们用数组 merged 存储最终的答案。

首先，我们将列表中的区间按照左端点升序排序。然后我们将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：

- 如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；

- 否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。

> 正确性证明

![image-20230228214116863](lcof.assets/image-20230228214116863.png)

> 代码

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) {
            return new int[0][2];
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] interval1, int[] interval2) {
                return interval1[0] - interval2[0];
            }
        });
        List<int[]> merged = new ArrayList<int[]>();
        for (int i = 0; i < intervals.length; ++i) {//针对每个intervel处理，要不直接加入merged，要不合并进merged的最近加入的区间
            int L = intervals[i][0], R = intervals[i][1];
            if (merged.size() == 0 || merged.get(merged.size() - 1)[1] < L) {
                merged.add(new int[]{L, R});
            } else {
                merged.get(merged.size() - 1)[1] = Math.max(merged.get(merged.size() - 1)[1], R);
            }
        }
        return merged.toArray(new int[merged.size()][]);//我：亲测，这里toArray中不用输入完整长度的数组，用new int[0][]即可
    }
}
```

- 我：comparator接口建议像我“首战”一样写成lamda格式
- 我：注意这里toArray要写成列表被转化成的最终格式，而不是写成list的每个元素被塞入什么结构中；并且要在列表的维度（本例中列表的维度对应数组的第一维）给数组赋予象征性的内存。在本例中，list中的int[]会被塞入int[]中形成`int[][]`；所以要输入`int[0][]`

> 复杂度分析

- 时间复杂度：O(nlogn)，其中 n为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 O(nlogn)。

- 空间复杂度：O(logn)，其中 n 为区间的数量。这里计算的是存储答案之外，使用的额外空间。O(logn) 即为排序所需要的空间复杂度。

### [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

#### 首战寄

思路：

1. 先获取当前数组的全排列（[剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)），把全排列的各个结果加入一个数组列表（`List<int[]>`）
2. 然后把列表中的每个数组都转化为字符串，得到一个字符串列表（`List<String>`）
3. 把字符串列表按从小到大排序（[剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)）
   - 注意，因为字符串长度最大为`100*3==300`位远超过常用基本数据类型的范围，这里要使用字符串的s1.compareTo(s2)去判断两个数值型字符串的大小（按字典顺序比较两个字符串，正负性等价于(int)s1-(int)s2），而不能用Integer.valueOf()把字符串转化为整数后再比较了
4. 把输入的nums转化为字符串，遍历排序后的字符串列表，就可以找到指定nums的字符串格式的下一个排列
5. 把字符串转化为数组

但这思路不可行，因为：

1. 数组最长为100，要求长为100的数组的全排列是很耗时，且占用内存的，因为全排列很多。剑指38能全排列是因为限制字符串长度为8
2. 对全排列（A100 100）个字符串做排序，也是耗时的

综上我这思路不是最好的思路，也不是可行的思路

#### 官方

> 问题描述

这道题是 LeetCode 31题。

“下一个排列” 的定义是：给定数字序列的字典序中下一个更大的排列。如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

我们可以将该问题形式化地描述为：给定若干个数字，将其组合为一个整数。如何将这些数字重新排列，以得到下一个更大的整数。如 123 下一个更大的数为 132。如果没有更大的整数，则输出最小的整数。

以 1,2,3,4,5,6 为例，其排列依次为：

```
123456
123465
123546
...
654321
```

可以看到有这样的关系：`123456 < 123465 < 123546 < ... < 654321`。

> 算法推导

如何得到这样的排列顺序？这是本文的重点。我们可以这样来分析：

1. 我们希望下一个数 比当前数大，这样才满足 “下一个排列” 的定义。因此只需要 将后面的「大数」与前面的「小数」交换，就能得到一个更大的数。比如 123456，将 5 和 6 交换就能得到一个更大的数 123465。
2. 我们还希望下一个数 增加的幅度尽可能的小，这样才满足“下一个排列与当前排列紧邻“的要求。为了满足这个要求，我们需要：
   1. 在 尽可能靠右的低位 进行交换，需要 从后向前 查找
   2. 将一个 尽可能小的「大数」 与前面的「小数」交换。比如 123465，下一个排列应该把 5 和 4 交换而不是把 6 和 4 交换
   3. 将「大数」换到前面后，需要将「大数」后面的所有数 重置为升序，升序排列就是最小的排列。以 123465 为例：首先按照上一步，交换 5 和 4，得到 123564；然后需要将 5 之后的数重置为升序，得到 123546。显然 123546 比 123564 更小，123546 就是 123465 的下一个排列

以上就是求 “下一个排列” 的分析过程。

> 可视化

以求 `12385764` 的下一个排列为例：

![image.png](lcof.assets/6e8c9822771be77c6f34cd3086153984eec386fb8376e09e36132ac36bb9cd6f-image.png)

首先从后向前查找第一个相邻升序的元素对 `(i,j)`。这里 `i=4`，`j=5`，对应的值为 `5`，`7`：

![image.png](lcof.assets/d7acefea4f7d4e2f19fb5eaa269c448a3098eee53656926a0ab592c564dde150-image.png)

然后在 `[j,end)` 从后向前查找第一个大于 `A[i]` 的值 `A[k]`。这里 `A[i]` 是 `5`，故 `A[k]` 是 `6`：

![image.png](lcof.assets/061cf291c237e6f5bcd0554192f894cd0c3e361b4564aa542aabe96e644afbf1-image.png)

将 `A[i]` 与 `A[k]` 交换。这里交换 `5`、`6`：

![image.png](lcof.assets/eb1470fd9942da6d2ab4855d13dfadcb715b629b4ea9cba0edfe2d1298744186-image.png)

这时 `[j,end)` 必然是降序，逆置 `[j,end)`，使其升序。这里逆置 `[7,5,4]`：

![image.png](lcof.assets/9d627a4ffda635bbf0c4fcdb7b1359c557db8e1c300ab54383a0bc89f6763c18-image.png)

因此，`12385764` 的下一个排列就是 `12386457`。

最后再可视化地对比一下这两个相邻的排列（橙色是蓝色的下一个排列）：

![image.png](lcof.assets/e56a66ed318d1761cd8c8f9d1521f82a30c71ecc84f551912b90d8fe254c8f3d-image.png)

> 算法过程

标准的 “下一个排列” 算法可以描述为：

1. 从后向前 查找第一个 相邻升序 的元素对 (i,j)，满足 A[i] < A[j]。此时 [j,end) 必然是降序
2. 在 [j,end) 从后向前 查找第一个满足 A[i] < A[k] 的 k。A[i]、A[k] 分别就是上文所说的「小数」、「大数」
3. 将 A[i] 与 A[k] 交换
4. 可以断定这时 [j,end) 必然是降序，逆置 [j,end)，使其升序
5. 如果在步骤 1 找不到符合的相邻元素对，说明当前 [begin,end) 为一个降序顺序，则直接跳到步骤 4

该方法支持数据重复，且在 C++ STL 中被采用。

> 代码

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int len = nums.length;
        for (int i = len - 1; i > 0; i--) {
            //从后往前先找出第一个相邻的后一个大于前一个情况，此时的i-1位置就是需要交换的位置
            if (nums[i] > nums[i - 1]) {
                //对i自己和之后的元素排序，[i,len)从小到大，第一个大于i-1位置的进行交换，那么就是下一个排列
                Arrays.sort(nums, i, len);
                for (int j = i; j <len; j++) {
                    if (nums[j] > nums[i - 1]) {
                        int temp = nums[j];
                        nums[j] = nums[i - 1];
                        nums[i - 1] = temp;
                        return;//我：这个return保证只会交换一次！
                    }
                }
            }
        }
    	Arrays.sort(nums);//最后3,2,1情况的下一个就是1,2,3要重新排列成最小的，这种情况上面的交换执行不了
		return;  
    }
 }

```

- 网友：啊呜，好棒的想法，与其再从后往前找k，不如直接j-end排序后，直接升序里找一个k
- 网友问：我想问一下 倒数第二个语句Arrays.sort(nums);的作用是什么呢？不用这个语句执行也是成功的
  - 网友答： 因为题目要求，如果不存在下一个更大的排列，则将数字升序排列
- 我：Arrays.sort排序基本类型和对象[参考](http://t.csdn.cn/B3FIQ)。
- 我：`Arrays.sort(T[] a,int formIndex, int toIndex) `的区间是针对index的左闭右开。

#### 即时再战成功

```java
class Solution {
    public void nextPermutation(int[] nums) {
        /**
        从右往左看，尽量找到第一个比nums大的排列
         */
        
        //因为要尽可能接近nums原排列，所以从右往左遍历
        for(int i=nums.length-1;i>0;i--){//!!!注意：i必须大于0，这样i-1才不会超过数组边界
            //找到第一个递增的位置，这样才有逆转递增从而让数更大的机会
            if(nums[i]>nums[i-1]){
                //让[i,end)有序
                Arrays.sort(nums,i,nums.length);
                //交换i-1和[i,end)中第一个比i-1位置更大的数
                for(int j=i;j<nums.length;j++){
                    if(nums[j]>nums[i-1]){
                        //交换j和i-1位置上的元素
                        int temp=nums[j];
                        nums[j]=nums[i-1];
                        nums[i-1]=temp;
                        return;//！！！注意：保证最小的交换完后就退出。这里是return而不要误写成break
                    }
                }


            }
        }

        //根据题意，如果序列是递减的，就返回递增
        Arrays.sort(nums);
        return;

    }
}
```

- 我：留意代码注释中的"!!!注意"，这是在本次写代码时疏忽的地方

## 分治算法(中等)

### [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

codetop==10

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
        递归创建当前节点的左右子树：
        1.根据先序遍历数组的特点可知，左子节点（即左子树的根节点）的pre_root_idx即为当前pre_root_idx+1，右子节点（即右子树的根节点）的pre_root_idx即为当前pre_root_idx+左子树节点个数+1(加1是为了越过左子树而到达右子树)
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

codetop==3

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
            //不断自乘即为不断平方
            x *= x;
            b >>= 1;
        }
        return res;
    }
}
```

- 网友问：为什么要把其中的n换成long类型？
  - 网友答：因为32位int是补码形式，正数是和原码相同，范围是0到2的31次方-1，但是对于负数，需要反码+1，范围是负2的31次方到0，负数要比正数多一个数字。如果传进来的int刚好是负2的32次方，取相反数之后就超过int32类型的取值范围了，所以需要用long来扩大取值范围。不好意思啊，评论区数学公式不太好打。
    - 我：题目要求里`-2^31 <= n <= 2^31-1`也可以看出n的取值时整个整数范围，换算成十进制约为`-2*10^9~2*10^9`。
  - 注意：有符号的话有个符号站位，就级数是2的31次方；如果没有正负号，就所有位都用来保存数字，级数就为2的32次方。
- 我：带符号右移`>>`的话，正数在高位补0，负数在补码的高位补1；不带符号右移`>>>`的话，正数在高位补0，负数也在补码的高位补0

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



### 1111111[剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

codetop==12

#### 首战半寄

```java
class Solution {
    int[] postorder;
    boolean falseFound;//默认是false
    public boolean verifyPostorder(int[] postorder) {
        /**
        二叉搜索树中左子树的所有值小于rootval，右子树的所有值大于rootval。
        后续遍历的话，数组末尾就是树的根节点。根节点的左边是【左子树】【右子树】黏连而成。分割左右子树的方法：
        遍历【左子树】【右子树】区域，第一次发现节点大于rootval的时候，说明前一步遍历完了左子树，现在在遍历右子树的第一个节点；因为不知道什么时候节点变大，所以用hash没用
         */
        this.postorder=postorder;//把传入的数组赋值给全局数组，使得在递归中也能访问到数组
        int root_index=postorder.length-1;
        int left_index=0,right_index=postorder.length-2;
        return recur(root_index,left_index,right_index);
    }
    public boolean recur(int root_index,int left_index,int right_index){
        //如果分治到最后lr交叉都没发现问题，说明至少本段分治符合二叉搜索树
        if(left_index>right_index){
            return true;
        }

        
        //遍历数组，找到第一个比root大的，即为右子树的第一个节点；如果没遍历到，说明右子树为空
        int firstRight=-1;
        for(int i=left_index;i<=right_index;i++){
            System.out.println("正在寻找右子树的首节点，当前节点值为："+postorder[i]);
            if(postorder[i]>postorder[root_index]){
                firstRight=i;
                System.out.println("找到了右子树的首节点，节点值为："+postorder[i]);
                //找到了右子树的首节点后要及时break，不然首节点会右移
                break;
            }
        }
        
        //右子树为空的话，右子树的首节点设置在数组范围外。这样下一轮递归的话，left是firstRight，right是right-1，就会形成交叉，直接返回true而不会进一步处理
        if(firstRight==-1){
            System.out.println("右子树为空");
            firstRight=right_index+1;
        }
        
        
        //由于是用节点是否比rootval小找到右子树第一个节点，所以左子树的合法性不需要验证，是已成立的。
        //检查右子树合法性，右子树中出现比root大的数的话，直接返回false
        for(int i=firstRight;i<=right_index;i++){
            System.out.println("正在校验右子树的合法性，当前节点值为："+postorder[i]+" root的值为:"+postorder[root_index]);
            if(postorder[i]<postorder[root_index]){
                System.out.println("右子树不合法，当前节点值为："+postorder[i]+" root的值为:"+postorder[root_index]);
                falseFound=true;
                return false;
            }
        }

        //剪枝
        if(falseFound)return false;

        System.out.println("进入更深层次递归");
        //如果没看出什么问题，递归查看当前root的左右子树的情况；左边函数是左子树，右边函数是右子树。两个都为true才返回true。
        return recur(firstRight-1,left_index,firstRight-2)&&recur(right_index,firstRight,right_index-1);
    }
}
```

- 思路完整，框架完整，但是有一些小细节是debug才发现的。

- 注意把代码中的sout注释掉后，代码的执行速度更快。

- 解题debug反思：

  - 用` if(postorder[i]>postorder[root_index])`找到右子树首节点后，忘记退出for循环，导致循环可能继续右移。

  - firstRight初始值设置为0，然后用`if(firstRight==0)`判断得firstRight在找右子树首节点的过程中没被修改过，便手动给右子树首节点赋值；但是忽视了一种情况，就是左子树为空，那么firstRight本身就是==0的，反而被我手动赋值了。所以应该如提交代码中似的，把firstRight初始值设置为-1，并且针对firstRight在寻找完右子树后有没有变化来判断（没变化说明右子树为空）是否需要手动赋值。

#### 官方-递归分治

思想就是我写代码的思想

解题思路：

![image-20220412195804787](lcof.assets/image-20220412195804787.png)

递归分支思路：

- 根据二叉搜索树的定义，可以通过递归，判断所有子树的 **正确性** （即其后序遍历是否满足二叉搜索树的定义） ，若所有子树都正确，则此序列为二叉搜索树的后序遍历。

递归解析：

![image-20220412195901552](lcof.assets/image-20220412195901552.png)

- 注意：划分左右子树的时候，寻找的**第一个**大于根节点的节点；我写代码就漏了这点，虽然思考的时候肯定是知道要找第一个。
- 20230205我：这里的一个重点注意是，后序遍历的最后一位元素就是树的根节点！

复杂度分析：

![image-20220412200013412](lcof.assets/image-20220412200013412.png)

- 时间复杂度“每轮递归都需要遍历树所有节点”解释：从left到firstRight，从firstRight到right，正好遍历完当前root对应的子树，也就是on的遍历。递归节点又有on，所以综合来看时间复杂度是on2。

代码：

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return recur(postorder, 0, postorder.length - 1);
    }
    boolean recur(int[] postorder, int i, int j) {
        if(i >= j) return true;
        int p = i;
        while(postorder[p] < postorder[j]) p++;
        int m = p;
        while(postorder[p] > postorder[j]) p++;
        return p == j && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
    }
}
```

## 分治算法(困难)

### [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

codetop==4

#### 首战告捷

```java
class Solution {
    public int[] printNumbers(int n) {
        //求出数字上界
        int limit=1;
        while(n>0){
            limit*=10;
            n--;
        }

        int[] result=new int[limit-1];
        for(int i=0;i<limit-1;i++){
            result[i]=i+1;
        }

        return result;

    }
}
```

- 直接傻瓜法，没有用分类推荐的分治法
- 我：我这其实在得到10的整数次幂的limit后，对limit--就省的后面都以limit-1为标准了。

#### 官方-分治(大数情况下)

![image-20220422194503309](lcof.assets/image-20220422194503309.png)

![Picture1.png](lcof.assets/83f4b5930ddc1d42b05c724ea2950ee7f00427b11150c86b45bd88405f8c7c87-Picture1.png)

根据以上方法，可初步编写全排列代码：

```java
class Solution {
    StringBuilder res;
    int count = 0, n;
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    public String printNumbers(int n) {
        this.n = n;
        res = new StringBuilder(); // 数字字符串集
        num = new char[n]; // 定义长度为 n 的字符列表
        dfs(0); // 开启全排列递归
        res.deleteCharAt(res.length() - 1); // 删除最后多余的逗号
        return res.toString(); // 转化为字符串并返回
    }
    void dfs(int x) {
        if(x == n) { // 终止条件：已固定完所有位
            res.append(String.valueOf(num) + ","); // 拼接 num 并添加至 res 尾部，使用逗号隔开
            return;
        }
        for(char i : loop) { // 遍历 ‘0‘ - ’9‘
            num[x] = i; // 固定第 x 位为 i
            dfs(x + 1); // 开启固定第 x + 1 位
        }
    }
}
```

在此方法下，各数字字符串被逗号隔开，共同组成长字符串。返回的数字集字符串如下所示：

```
输入：n = 1
输出："0,1,2,3,4,5,6,7,8,9"

输入：n = 2
输出："00,01,02,...,10,11,12,...,97,98,99"

输入：n = 3
输出："000,001,002,...,100,101,102,...,997,998,999"
```

![image-20220422194602103](lcof.assets/image-20220422194602103.png)

复杂度分析：

![image-20220422194627235](lcof.assets/image-20220422194627235.png)

代码：

为 **正确表示大数** ，以下代码的返回值为数字字符串集拼接而成的长字符串。

```java
class Solution {
    StringBuilder res;
    int nine = 0, count = 0, start, n;
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    public String printNumbers(int n) {
        this.n = n;
        res = new StringBuilder();
        num = new char[n];
        start = n - 1;
        dfs(0);
        res.deleteCharAt(res.length() - 1);
        return res.toString();
    }
    void dfs(int x) {
        if(x == n) {
            String s = String.valueOf(num).substring(start);
            if(!s.equals("0")) res.append(s + ",");
            if(n - start == nine) start--;
            return;
        }
        for(char i : loop) {
            if(i == '9') nine++;
            num[x] = i;
            dfs(x + 1);
        }
        nine--;
    }
}
```

本题要求输出 int 类型数组。为 **运行通过** ，可在添加数字字符串 s*s* 前，将其转化为 int 类型。代码如下所示：

```java
class Solution {
    int[] res;
    int nine = 0, count = 0, start, n;
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    public int[] printNumbers(int n) {
        this.n = n;
        res = new int[(int)Math.pow(10, n) - 1];
        num = new char[n];
        start = n - 1;
        dfs(0);
        return res;
    }
    void dfs(int x) {
        if(x == n) {
            String s = String.valueOf(num).substring(start);
            if(!s.equals("0")) res[count++] = Integer.parseInt(s);
            if(n - start == nine) start--;
            return;
        }
        for(char i : loop) {
            if(i == '9') nine++;
            num[x] = i;
            dfs(x + 1);
        }
        nine--;
    }
}
```

网友：基本上看懂了，很巧妙的办法，收藏了，顺便加点注释帮助大伙理解：

```java
class Solution {
    int[] res;
    //start表示该数字当前左边界，这个左边界意思是指当前数字最高位对应的char数组下标。如n=2时，1~9左边界为1，10~99左边界为0
    //nine表示当前数字中出现了多少个9，如果出现1个9，左边界就要向左移1位。例如第1次出现“9”是在9这个数字出现的时候，此时nine++变为1，
    //进入下次递归n为2，nine为1，start为1，此时start就要-1，以便统计二位数字
    int nine = 0, count = 0, start, n;   
    char[] num, loop = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
    public int[] printNumbers(int n) {
        this.n = n;
        //用来保存最终数字结果的
        res = new int[(int)Math.pow(10, n) - 1];   
        //num数组用来表示字符串，比如n等于2，则num数组为['0''0']、['0''1']、['0''2']...后边是将它转为字符串并按照左边界的位置进行截取的
        num = new char[n];  
        start = n - 1;  //最开始的左边界是从n-1，开始的，因为char数组的下标是从0开始，最末一位为n-1
        dfs(0);   //从char数组的第0位开始
        return res;
    }
    void dfs(int x) {
        //结束条件：当前x的下标越过char数组的最后一位下标n-1，此时记录结果
        if(x == n) {
            String s = String.valueOf(num).substring(start);   //从start开始截取字符串，如"01"截取后就是"1"
            if(!s.equals("0")) res[count++] = Integer.parseInt(s);   //防止将"0"、"00"、"000"加进来
            if(n - start == nine) start--;   //n减去start等于nine，表示要进位了，进位就是将左边界start左移一位，即-1
            return;
        }
        //给char数组第x位添加数字，添加完后进入下一位
        for(char i : loop) {
            if(i == '9') nine++;
            num[x] = i;
            dfs(x + 1);
        }
        nine--;   //回溯
    }
}
```

#### 即时再战

分治法比较难，先不重写分治法了。

### [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

codetop=30

#### 首战寄

没有思路

#### 官方-归并排序

解题思路：

![image-20220423201841437](lcof.assets/image-20220423201841437.png)

![Picture1.png](lcof.assets/1614274007-nBQbZZ-Picture1.png)

合并阶段 本质上是 合并两个排序数组 的过程，而每当遇到 左子数组当前元素 > 右子数组当前元素 时，意味着 「左子数组当前元素 至 末尾元素」 与 「右子数组当前元素」 构成了若干 「逆序对」 。因此，考虑在归并排序的合并阶段统计「逆序对」数量，完成归并排序时，也随之完成所有逆序对的统计。

算法流程：

![image-20220423201943425](lcof.assets/image-20220423201943425.png)

![Picture2.png](lcof.assets/1614274007-rtFHbG-Picture2.png)

复杂度分析：

![image-20220423202030314](lcof.assets/image-20220423202030314.png)

代码：

```java
class Solution {
    int count;
    public int reversePairs(int[] nums) {
        this.count = 0;
        merge(nums, 0, nums.length - 1);
        return count;
    }

    public void merge(int[] nums, int left, int right) {
        int mid = left + ((right - left) >> 1);
        //至少有两个节点，才有切割和合并的必要
        if (left < right) {
            //先递归切割，直到切为单个元素（单个元素就不能再切了）
            merge(nums, left, mid);
            merge(nums, mid + 1, right);
            //自底向上，合并
            mergeSort(nums, left, mid, right);
        }
    }

    public void mergeSort(int[] nums, int left, int mid, int right) {
        //保存[left,right]间的合并后的数组
        int[] temparr = new int[right - left + 1];
        int index = 0;
        //temp1是左数组的第一个index；temp2是右数组的第一个index
        int temp1 = left, temp2 = mid + 1;

        //左数组和右数组都是有序的，合并两个数组
        while (temp1 <= mid && temp2 <= right) {
            //总是把两个数组各自的当前指针中较小的那个元素放入temparr
            if (nums[temp1] <= nums[temp2]) {
                temparr[index++] = nums[temp1++];
            } else {
                //用来统计逆序对的个数
                count += (mid - temp1 + 1);
                temparr[index++] = nums[temp2++];
            }
        }
        //把左边剩余的数移入数组
        while (temp1 <= mid) {
            temparr[index++] = nums[temp1++];
        }
        //把右边剩余的数移入数组
        while (temp2 <= right) {
            temparr[index++] = nums[temp2++];
        }
        //把新数组中的数覆盖nums数组
        for (int k = 0; k < temparr.length; k++) {
            nums[k + left] = temparr[k];
        }
    }
}
```

#### 即时再战

本题hard，虽然频度高，但是先放放不再战。

20230205我：能看懂

## 位运算(简单)

- 我：`& | >>> >> << <<<`参考[优秀文章](https://www.cnblogs.com/LQBlog/p/15210471.html#_label5)

### [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

codetop==5

####  首战寄

有思路，但是代码运行结果不符合预期。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        /**
        使用位运算，总是&最小位，为1的话则计数；计数完毕后右移，直到没有位了
         */
        System.out.println("输入的n:"+n);
        int count=0;
        while(n>0){
            System.out.println("当前n:"+n);
            if((n&1)==1){//计算数的二进制表示的末尾是否为1，是的话计数
                System.out.println("当前n的末尾为1");
                count++;
            }
            //不管末尾是否为1，都右移，即二进制末尾截掉一位
            n>>=n;
            System.out.println("右移1位后n:"+n);
        }

        return count;
        
    }
}
```

![image-20220413155221429](lcof.assets/image-20220413155221429.png)

- 问答问：打印出来的是输入了11右移一位后不是5，而直接为0退出了，不懂了。就算是以十进制为单位右移（除法），那也还有1啊。
  - 自答：我这应该是`n>>>=1;`，原因有两个：
    1. 源代码`n>>=n;`表示一下右移11位，肯定一下就移完了
    2. java中无符号位运算用`>>>`。注意区别`>>`和`>>>`，`>>>`是对int32和int64才有用的无符号右移,[参考](https://www.cnblogs.com/SunArmy/p/9837348.html)
- 看完官方解析后的反思，错误：
  - `while(n>0)`导致输入n<0的数话，会不进入while。应该写成`while(n!=0)`
  - ` n>>=n;`会一下子有符号右移n位，直接为0；并且对负数有符号位移的话补1，会导致计算错误。应该写成`n>>>=1;`。

#### 网友热门

java居然有这个方法:

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        return Integer.bitCount(n);
    }
}
```

#### 官方1-位运算+逐位判断

思路：

![image-20220413161839865](lcof.assets/image-20220413161839865.png)

算法流程：

![image-20220413161858902](lcof.assets/image-20220413161858902.png)



复杂度分析：

![image-20220413161917124](lcof.assets/image-20220413161917124.png)

代码：

```java
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
}
```

- 我：思路就是我首战寄的思路

#### 官方2-位运算+巧用 n \& (n - 1)

思路：

![image-20220413162127582](lcof.assets/image-20220413162127582.png)

算法流程：

![image-20220413162143982](lcof.assets/image-20220413162143982.png)

复杂度分析：

![image-20220413162204749](lcof.assets/image-20220413162204749.png)

- 官方1中，哪怕当前很多位是0也要一个一个地位移，浪费时间复杂度。本方法2直接采取“消去1”的策略，每次消去末尾的1都可以给res加一，只需要做1的个数次操作即可，而不是像官方方法1中做多位次运算。
  - 我：每次` n &= n - 1;`都会消去一个`1`，并且附带避免处理这个1右边的所有0。


代码：

```java
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res++;
            n &= n - 1;
        }
        return res;
    }
}
```

### [剑指 Offer 65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

#### 首战寄

无思路。

建议熟悉java位运算：[参考](https://zhuanlan.zhihu.com/p/106893096)

#### 官方-位运算

解题思路：

![image-20220413165208729](lcof.assets/image-20220413165208729.png)

- 异或：不同为1，相同为0；与：都为1才为1，有一个是0都为0。

- 我：比如`1111+1111`=`0000+11110`=30，无进位跳出

- 20230205我：画出这个表，就能找“无进位和”与“进位”的规律

![Picture1.png](lcof.assets/56d56524d8d2b1318f78e209fffe0e266f97631178f6bfd627db85fcd2503205-Picture1.png)

- 我：这里最难得就是，要记住java的异或运算符号`^`；异或表示”按位相异才为1，按位相同则为0“。

> Q ： 若数字 aa 和 bb 中有负数，则变成了减法，如何处理？
> A ： 在计算机系统中，数值一律用 补码 来表示和存储。补码的优势： 加法、减法可以统一处理（CPU只有加法器）。因此，以上方法 同时适用于正数和负数的加法 。

复杂度分析：

![image-20220413165358375](lcof.assets/image-20220413165358375.png)

代码：

```java
class Solution {
    public int add(int a, int b) {
        while(b != 0) { // 当进位为 0 时跳出
            int c = (a & b) << 1;  // c = 进位
            a ^= b; // a = 非进位和
            b = c; // b = 进位
        }
        return a;
    }
}
```

#### 即时再战成功

```java
class Solution {
    public int add(int a, int b) {
        /**
        加法=异或+与<<
         */
        
        //求进位；求进位要放在求非进位之前，因为“求非进位”时会修改a的值。注意<<1的移动位数1别忘记！！
        int c=(a&b)<<1;
        //求非进位
        a^=b;
        //要记住把c赋值给b，这样才能让进位和非进位继续相加
        b=c;

        //进位为0的时候，求和结果即为a。
        while(b!=0){
            //求进位
            c=(a&b)<<1;
            //求非进位
            a^=b;
            //要记住把c赋值给b，这样才能让进位和非进位继续相加
            b=c;
        }

        return a;

    }
}
```

- 注意：<<1的移动位数1别忘记！！

## 位运算(中等)

### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

codetop==2

#### 首战寄 

没思路

#### 官方-位运算异或

解题思路：

![image-20220414142917210](lcof.assets/image-20220414142917210.png)

```java
public int[] singleNumber(int[] nums) {
    int x = 0;
    for(int num : nums)  // 1. 遍历 nums 执行异或运算
        x ^= num;
    return x;            // 2. 返回出现一次的数字 x
}
```

异或符合交换律解析如下：

![image-20220414142641610](lcof.assets/image-20220414142641610.png)

难点：

![image-20220414145633625](lcof.assets/image-20220414145633625.png)

- 异或两个数，那么原数不同的地方会在异或后表示为1，我们侦测异或后的1的位置，就可以把两个不同的数区分开来；两个数可能有多个位不一样，任意抓住一位都能区分开原来的两个数，这里去找不同的最低位。
- 用mask不仅可以区分出两个目标数，还能把剩余的所有数分两队；在“mask为1的位” 为1的数被分到一队，在“mask为1的那位” 为0的数被分到另一队，因为前者和mask异或得0，后者和mask异或不得0。

代码：

```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        //用于将所有的数异或起来。最后的k即为两个只出现一次的数的异或结果
        int k = 0;
        for(int num: nums) {
            k ^= num;
        }
        
        //获得k中最低位的1。这个1标记了“两个只出现一次的数”从低位往高位比，第一次出现不同的位置
        int mask = 1;
        //mask = k & (-k) 这种方法也可以得到mask，具体原因百度 哈哈哈哈哈
        while((k & mask) == 0) {//异或结果k和mask再进行“与操作”得0，表示在mask为1的位置 k在此位置不为1，即两个只出现一次的数在这位是相同的（这样异或后 此位才为0）
            mask <<= 1;
        }
        
        //对数组用mask做分组的同时，用a和b分别异或处理属于不同类的数据，得到的a b结果即为两个只出现一次的数
        int a = 0;
        int b = 0;
        for(int num: nums) {
            if((num & mask) == 0) {
                a ^= num;
            } else {
                b ^= num;
            }
        }
        
        return new int[]{a, b};
    }
}
```

#### 即时再战成功

```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        /**
        使用异或^从出现两次的数中找到只出现一次的数
        使用与&把两个只出现一次的数，利用他两的异或结果，把它们分成2组
         */
        
        //数组全体异或，假设只出现一次的数为a b，那么得到的结果是a^b
        int c=0;
        for(int i=0;i<nums.length;i++){
            c^=nums[i];
        }

        //用探针pin把两个只出现一次的数分开
        int pin=1;
        while((c&pin)==0){//a和b在pin中1的位置相同。注意按位与&的优先级低，每次使用必须用括号包裹！！！
            pin=pin<<1;
        }

        //根据pin把nums分两队；两队内自行异或，即得到a和b
        int a=0,b=0;
        for(int i=0;i<nums.length;i++){
            if((nums[i]&pin)==0){
                a^=nums[i];
            }else{
                b^=nums[i];
            }
        }

        return new int[]{a,b};
    }
}
```

- 注意：
  - 按位与&的优先级低，每次使用必须用括号包裹
  - pin用&操作把数组分两组，因为一组与pin为0，另一组不是；num & pin得0，是因为num在pin为1的那一位为0，0&1==0,1&1==1。

### [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

codetop==3

#### 首战寄

#### 官方-位运算+有限状态自动机

如下图所示，考虑数字的二进制形式，对于出现三次的数字，各 **二进制位** 出现的次数都是 3 的倍数。
因此，统计所有数字的各二进制位中 1 的出现次数，并对 3求余，结果则为只出现一次的数字。

![Picture1.png](lcof.assets/28f2379be5beccb877c8f1586d8673a256594e0fc45422b03773b8d4c8418825-Picture1.png)

略。。题目出现率不高

#### 大佬-hashMap法

```java
// 使用 HashMap 记录各个数字出现的次数
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap();

        for(int i = nums.length - 1; i >= 0; --i){
            int key = nums[i];
            if(!map.containsKey(key)){
                // 如果之前没有遇到这一数字，则放入 map 中
                map.put(key, 1);
            }else{
                // 如果之前遇到过这一数字，则出现次数加 1
                map.put(key, map.get(key) + 1);
            }
        }

        for(Map.Entry<Integer, Integer> entry: map.entrySet()){
            if(entry.getValue() == 1){
                return entry.getKey();
            }
        }

        return -1;
    }
```

- 网友评：可以再优化一下，已经记过一次的数再放进来直接置值为-1就好，少一次get操作。

  ```
          for (int i = 0; i < nums.length; i++) {
              Integer obj = map.get(nums[i]);
              if(obj == null){
                  map.put(nums[i],1);
              }else if(obj == 1){
                  map.put(nums[i],-1);
              }
          }
  ```

#### 即时再战

用hashmap的话，就不需要再战了。

## 数学(简单)

### [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

codetop==16

#### 首战寄

有正负电荷抵消的思想，但是想不出来怎么实施。能实施的只有hashmap计数算法，性能不算好。

#### 官方-摩尔投票法

我个人更愿意称之为正负电荷抵消法,关注抵消的时刻。

解题思路：

![image-20220414164157239](lcof.assets/image-20220414164157239.png)

摩尔投票法：

![image-20220414164224802](lcof.assets/image-20220414164224802.png)

![Picture1.png](lcof.assets/1603612327-bOQxzq-Picture1.png)

![image-20220414164246112](lcof.assets/image-20220414164246112.png)

算法流程：

![image-20220414164259764](lcof.assets/image-20220414164259764.png)

复杂度分析：

![image-20220414164332148](lcof.assets/image-20220414164332148.png)

代码：

```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        return x;
    }
}
```

![image-20220414164717570](lcof.assets/image-20220414164717570.png)

```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0, count = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        // 验证 x 是否为众数
        for(int num : nums)
            if(num == x) count++;
        return count > nums.length / 2 ? x : 0; // 当无众数时返回 0
    }
}
```

- 本题众数指数目超过一般的数，但是一般指[出现次数最多的数](https://baike.baidu.com/item/%E4%BC%97%E6%95%B0/44796)

#### 即时再战成功

```java
class Solution {
    public int majorityElement(int[] nums) {
        /**
        正负电荷抵消
         */

        int major=0;
        int count=0;
        for(int i=0;i<nums.length;i++){
            //在计数为0时，把当前节点设置为众数,重设置计数为1
            if(count==0){
                major=nums[i];
                count=1;
            }else{//如果计数不为0，比较当前数和众数是否相同，相同则给计数+1，不同则给计数-1
                if(nums[i]==major){
                    count++;
                }else{
                    count--;
                }
            }
        }

        //退出for循环后，留下的众数必定是目标众数
        return major;
    }
}
```

### [剑指 Offer 66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

codetop==3

解题思路：

![image-20220414172126126](lcof.assets/image-20220414172126126.png)

![Picture1.png](lcof.assets/1624619180-vpyyqh-Picture1.png)

算法流程：

![image-20220414172154862](lcof.assets/image-20220414172154862.png)

复杂度分析：

![image-20220414172212852](lcof.assets/image-20220414172212852.png)

代码：

```java
class Solution {
    public int[] constructArr(int[] a) {
        int len = a.length;
        if(len == 0) return new int[0];
        int[] b = new int[len];
        b[0] = 1;
        int tmp = 1;
        for(int i = 1; i < len; i++) {
            b[i] = b[i - 1] * a[i - 1];
        }
        for(int i = len - 2; i >= 0; i--) {
            tmp *= a[i + 1];
            b[i] *= tmp;
        }
        return b;
    }
}
```

#### 即时再战成功

```java
class Solution {
    public int[] constructArr(int[] a) {
        /**
        用两个dp乘法
         */
        //处理特殊情况。我认为是不用处理的，但是系统还是会输入空数组,空数组如下设置
        if(a.length==0||a.length==1)return new int[0];

        //把对角线下半的乘积先放入结果数组。新建数组要new int[length],别忘了new
        int[] res=new int[a.length];
        int temp=1;
        res[0]=1;
        for(int i=1;i<a.length;i++){
            temp*=a[i-1];
            res[i]=temp;
        }

        //把对角线上半的成绩放入结果数组
        temp=1;
        for(int i=a.length-2;i>=0;i--){
            temp*=a[i+1];
            res[i]*=temp;
        }

        return res;

    }
}
```

- 注意：
  - 新建空数组的方式:`new int[0];`
  - 新建数组不要忘记new关键字

## 数学(中等)

### [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

codetop==5

#### 没思路

#### 官方1-贪心

我：贪心的数学要求高，普适性不大

![image-20220415170356783](lcof.assets/image-20220415170356783.png)

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        int a = n / 3, b = n % 3;
        if(b == 0) return (int)Math.pow(3, a);
        if(b == 1) return (int)Math.pow(3, a - 1) * 4;
        return (int)Math.pow(3, a) * 2;
    }
}
```

#### 官方2-动态规划

思路：

![image-20220423205556605](lcof.assets/image-20220423205556605.png)

代码：

```java
class Solution {
    public int cuttingRope(int n) {
        int[] dp = new int[n + 1];
        dp[2] = 1;
        for(int i = 3; i < n + 1; i++){
            for(int j = 2; j < i; j++){
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
}
```

- 网友问：dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j])); 动态规划dp[i] 为什么依赖于自己呢
  - 网友答：想了一会才反应过来，对于同一个i，内层循环对不同的j都还会拿到一个max，所以每次内层循环都要更新max
  - 网友答：不是依赖自己，是不同的j都会有一个dp[i]，这是在取不同的j情况下dp[i]的最大值。dp[i]就是更新储存最大值的，不是依赖自己，还是那两种情况, 感觉还是写成两段更容易理解一点` int tmp=max(j*(i-j),j*dp[i-j]); dp[i]=max(dp[i],tmp);`

复杂度分析：

![image-20220423210349090](lcof.assets/image-20220423210349090.png)

#### 即时重做成功

```java
class Solution {
    public int cuttingRope(int n) {
        /**
        绳子可以剪成两端，左边这段固定，右边这段剪不剪使用动态规划判断
         */
        //dpi表示绳子长度为i时的最大乘积;因为要记录dpn，所以dp数组的长度设置为n+1
        int []dp=new int[n+1];

        //初始化。长度为1的绳子没法剪，从长度为2的绳子开始考虑
        dp[2]=1;

        //从短绳子到长绳子，根据短绳子的最大乘积得到长绳子的最大乘积
        for(int i=3;i<=n;i++){
            //针对每个长度的绳子，取不同长度的左边段，求他的最大乘积。i表示绳子全长，j表示绳子的左边的长度
            for(int j=1;j<i;j++){//!!!我注意：官方做法是j从2开始，但是我这从1开始不会影响结果，因为j==1的都不是最佳写法，不会被记录进dpi；但是这样会计算无用功，推荐使用官方的写法。
                //右半部继续剪短，和右半部不继续剪短，两种情况求得一个较大值；使用到dp(i-j)就体现了动态规划由小得到大的思想
                int temp=Math.max(j*(i-j),j*dp[i-j]);//!!!我注意：刚开始担心i-j==1时，dp[1]没有定义而使用默认的0会导致错误，想着要不就给dp[1]赋值为1表示不切的话最大积为1；但是可以发现dp[1]的值不管是1还是0，在i-j==1时,temp都等于j，所以没必要可以设置dp[1]了（而且长度为1的绳子本来就不可剪，不符合题目限制）。
                //该较大值和其他j的情况下记载的较大值做比较，保留下大的那个
                dp[i]=Math.max(dp[i],temp);
            }
        }

        //处理完数组后，返回dpn即为剪短长度为n的数组可能得到的最大乘积
        return dp[n];


    }
}
```

- 20230205我：代码中加了“注意”的是我读完后的批注。

### [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

codetop==9

#### 首战半寄

能通过23/32个测试案例，但是在大数上超时了；至少有思路，不过不够优化。

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        /**
        等差数列求和公式+while双指针
         */
        List<List<Integer>> resultList =new ArrayList<>();
        List<Integer> oneTempInstance=null;
        //以每个整数为left开始做双指针
        for(int i=1;i<target;i++){
            long left=i,right=target;
            //做双指针
            while(left<right){
                long temp1=(left+right)*(right-left+1),temp2=target*2;//避免反复计算.除法默认向下取整，尽量避免。用long防止超过整数边界
                if(temp1==temp2){//找到目标时。不会再在本基础上缩小范围了，用break跳出while避免无效计算
                    oneTempInstance=new ArrayList<>();
                    for(int j=(int)left;j<=right;j++){
                        oneTempInstance.add(j);
                    }
                    resultList.add(new ArrayList<>(oneTempInstance));//为了避免oneInstance的操作影响resultList中已存入的结果，根据oneTempInstance的内容新分配一块内存作为resultList的元素
                    break;
                }else if(temp1<temp2){
                    left++;
                }else{
                    right--;
                }
            }
        }

        //把列表转化为2维数组返回。转换成数组是真傻比，直接返回链表不就行了！
        int[][] result=new int[resultList.size()][];//第一个方括号表示行，应该在这个括号限定行数
        for(int i=0;i<resultList.size();i++){
            //复用oneTempInstance
            oneTempInstance=resultList.get(i);
            int tempSize=oneTempInstance.size();
            //对二维数组不等长的情况分配内存：https://blog.csdn.net/weixin_30709179/article/details/115082821
            result[i]=new int[tempSize];
            for(int j=0;j<tempSize;j++){
                result[i][j]=oneTempInstance.get(j);
            }
        }
        return result;
    }
}
```

- 注意：
  - 大数乘法的时候可能会超过整数边界，要用long
  - long基本类型可以使用`(int) longValue`来强转成int。
- 我：已经想到左右指针了，相当于是官方解法滑动窗口的两边，但是用for循环暴力导致O(nlogn)的时间复杂度

#### 官方-滑动窗口

什么是滑动窗口：

![image-20220415204353222](lcof.assets/image-20220415204353222.png)

- 注意：官方这里说 滑动窗口一般左闭右开，但是实战中也可以自己实现两个都闭的方法，如本题。
  - 我：左闭右开，让我想到了二分查找的经典写法。


如何用滑动窗口解这道题：

![image-20220415204435385](lcof.assets/image-20220415204435385.png)

![proof](lcof.assets/728c705889a672d5a85709cb3fd157216bb1a41dc377dcc125818d9e18b8dd55.jpg)

![image-20220415204506800](lcof.assets/image-20220415204506800.png)

时间复杂度：

![image-20220415204754472](lcof.assets/image-20220415204754472.png)

代码：

```java
public int[][] findContinuousSequence(int target) {
    int i = 1; // 滑动窗口的左边界
    int j = 1; // 滑动窗口的右边界
    int sum = 0; // 滑动窗口中数字的和
    List<int[]> res = new ArrayList<>();

    while (i <= target / 2) {
        if (sum < target) {
            // 右边界向右移动
            sum += j;
            j++;
        } else if (sum > target) {
            // 左边界向右移动
            sum -= i;
            i++;
        } else {
            // 记录结果
            int[] arr = new int[j-i];
            for (int k = i; k < j; k++) {
                arr[k-i] = k;
            }
            res.add(arr);
            // 左边界向右移动
            sum -= i;
            i++;
        }
    }

    return res.toArray(new int[res.size()][]);
}
```

- 网友：还是有点不太懂为什么i <= target/2？求大佬
  - 网友答：5最大只可能是2+3，不可能等于3+4
- 注意：
  - `list.toArray(new 数组)`，要传入“分配了内存的数组“
  - 可以先使用`List<int[]>`对结果列表进行初始化，最后把只有一层的list轻松转为数组，即得二维数组。
  - `int[] arr = new int[j-i];//记录结果`体现了左闭右开，因为i位置的数不会被记录。

网友补充，有左闭右闭的，都闭更利于理解：

```java
class Solution {
public int[][] findContinuousSequence(int target) {
            List<int[]> res = new ArrayList<>();
            // 滑动窗口,左右指针指向的数
            // 初始窗口的宽度为1，所有左右都指向1
            int left = 1;
            int right = 1;
            // 窗口内的数字和,左闭右闭
            int sum = 1;
            // 只需要遍历到目标值的一般即可，后面加起来肯定大于target
            while (left <= target / 2){
                // 如果小了，右指针增大
                if (sum < target) {
                    right++;
                    sum += right;
                }else if (sum > target){
                    // 如果大了，左指针增大
                    sum -= left;
                    left++;
                }else {
                    // 说明匹配上了需要把窗口中的元素放进结果集
                    int[] level = new int[right - left + 1];
                    for (int i = left; i <= right; i++) {
                        level[i - left] = i;
                    }
                    res.add(level);
                    // 加入结果集之后窗口要移动
                    sum -= left;
                    left++;
                }
            }
            return res.toArray(new int[res.size()][]);
        }
}
```

#### 即时再战成功

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        /**
        滑动窗口左闭右闭，on时间复杂度
        滑动窗口左右都只能右移
         */
        List<int[]>resultList=new ArrayList<>();
        int l=1,r=1,sum=1;
        while(l<=target/2){//之所以要<=而不是<,是因为target==3的情况
            if(sum>target){
                sum-=l;
                l++;
            }else if(sum<target){
                r++;//注意sum 窗沿的处理顺序，在两个if情况是不一样的
                sum+=r;
            }else{
                //找到目标窗口，把数据填入
                int[] temp=new int[r-l+1];
                for(int i=l;i<=r;i++){
                    temp[i-l]=i;
                }
                resultList.add(temp);//因为temp只在else的花括号中存活，且在temp传入resultList和temp死亡前的这段时间里没有针对temp元素的修改；所以不需要额外给内存，即不用防止修改temp导致resultList被修改，
                //左窗沿右移
                sum-=l;
                l++;
            }
        }

        return resultList.toArray(new int[0][]);
    }
}
```

- 注意：
  - 把List<int[]>转化为int\[][]要用：`resultList.toArray(new int[resultList.size()][]);`。第二个中括号的内存是已知的，因为resultList中已经存了每个元素；需要让toArray知道的是resultList有多少个元素，所以要在第一个中括号中设置大小，即二维数组的行数。
  - `return resultList.toArray(new int[resultList.size()][]);`写成`return resultList.toArray(new int[0][]);`就行，更节省空间，不需要在toArray的输入中指定一共需要多少个，只需要指定一个即可；但是完全不指定大小如`return resultList.toArray(new int[][]);`也是不行的，会报错`error: array dimension missing`。

### [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

codetop 27

#### 首战寄

没思路。

[参考约瑟夫环](https://blog.csdn.net/u011500062/article/details/72855826)

- 网友：每杀死一个人，整体往前移动m个数是为了假设下一次开始数m个数是从0开始。

  也就是说f(11,3)=6
  假设最后存活下来的人在第一轮的坐标为6,杀掉第3个人(i=2)的人，杀掉后下一次从i=3的位置开始计数下一轮，这时候还剩10个人，这时候如果想从0计算最后存活下来的人的坐标，就得从把下一轮开始计数位置(也就是i=3)那个位置的坐标向左移动3，这时候整体向左移3，最后存活下来的坐标也就变成了3，也会是10个人的时候坐标变成了3

#### 官方1-模拟链表

O(n^2)的时间复杂度

![image-20220415235803966](lcof.assets/image-20220415235803966.png)

- 我：时间复杂度为Omn是因为，一共有n个节点，每删掉一个节点都要走m步，所以综合删到最后一个节点的时间复杂度是Omn

![image.png](lcof.assets/0a7538f213070b64cac8427d453de6e305fb63c99e9585948f52a116e0558ddc-image.png)

![image-20220415235828058](lcof.assets/image-20220415235828058.png)

```java
class Solution {
    public int lastRemaining(int n, int m) {
        ArrayList<Integer> list = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            list.add(i);
        }
        int idx = 0;
        while (n > 1) {
            idx = (idx + m - 1) % n;
            list.remove(idx);
            n--;
        }
        return list.get(0);
    }
}
```



#### 官方2-约瑟夫环

> 这个问题是以弗拉维奥·约瑟夫命名的，他是1世纪的一名犹太历史学家。他在自己的日记中写道，他和他的40个战友被罗马军队包围在洞中。他们讨论是自杀还是被俘，最终决定自杀，并以抽签的方式决定谁杀掉谁。约瑟夫斯和另外一个人是最后两个留下的人。约瑟夫斯说服了那个人，他们将向罗马军队投降，不再自杀。约瑟夫斯把他的存活归因于运气或天意，他不知道是哪一个。 —— 【约瑟夫问题】维基百科

数学解法，O(n):

这么著名的约瑟夫环问题，是有数学解法的！
因为数据是放在数组里，所以我在数组后面加上了数组的复制，以体现是环状的。我们先忽略图片里的箭头：
【第一轮后面的数字应该是[0, 1, 2 ,3 ,4]，手误打错了。。抱歉】


![image.png](lcof.assets/9dda886441be8d249abb76e35f53f29fd6e780718d4aca2ee3c78f947fb76e75-image.png)

![image-20220415235554790](lcof.assets/image-20220415235554790.png)

```java
class Solution {
    public int lastRemaining(int n, int m) {
        int ans = 0;//我：初始化为0是因为最后一轮推演完成后只剩下第0个位置上有数
        // 最后一轮剩下2个人，所以从2开始反推
        for (int i = 2; i <= n; i++) {
            ans = (ans + m) % i;
        }
        return ans;
    }
}
```



## 数学(困难)

### [剑指 Offer 14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

codetop=1

#### 首战寄

尝试直接在剪绳子I的基础上修改代码，失败：

```java
class Solution {
    public int cuttingRope(int n) {
        double[] dp = new double[n + 1];
        dp[2] = 1;
        for(int i = 3; i < n + 1; i++){
            for(int j = 2; j < i; j++){
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return (int)(dp[n]%1000000007);//n==120时，输出953271157，期待953271190//我：这里针对求余后的double强转为int，损失较小。但是还是有损失，应该是中间某个dp超过int边界导致的。
        //return (int)dp[n]%1000000007;//n==120时，输出147483633，期待953271190 //我：这个只转化了dp，对double强转为int导致损失较大
    }
}
```

1，我：本题和剪绳子I不同的点就是：

1. n的范围扩大了！现在n可以是1000，那么比如1000且成10个100，那么就是100^10>整数范围约2*10^9。那么就会造成整数的溢出，会导致数字变成在最小整数基础上增大。[参考](http://t.csdn.cn/SVbdE)。所以在比较前就得取余才能避免整数溢出。

2. 但是在比较之前取余的话，可能导致更小的数在取余后反而更大。比如`(1e9+8)%1e9+7==1`，小于`(1e9+6)%1e9+7==1e9+6`。
3. math.max只接受double long int float类型的数据，[参考](http://t.csdn.cn/Ggwc4)

- 综合以上3点，既不能只在结果处取余，也不能在过程中取余；所以得使用大数运算BigInteger来解决问题

#### 大佬-贪心

此题与 面试题14- I. 剪绳子 主体等价，唯一不同在于本题目涉及 “大数越界情况下的求余问题” 。
建议先做上一道题，在此基础上再研究此题目的大数求余方法。

代码：

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        long res=1L;
        int p=(int)1e9+7;
        //贪心算法，优先切三，其次切二
        while(n>4){
            res=res*3%p;
            n-=3;
        }
        //出来循环只有三种情况，分别是n=2、3、4
        return (int)(res*n%p);
    }
}
```

#### 大佬-dp

1，大佬1：这一题已经不能用动态规划了，取余之后max函数就不能用来比大小了。

- 我和网友问：为什么不可以比较？return dpn的时候取余不就可以了？

- 我：dp中不能存mod后的结果，否则无法比较大小；比如`(1e9+8)%1e9+7==1`，小于`(1e9+6)%1e9+7==1e9+6`。

2，大佬2：可以使用动态规划+大数运算。剪绳子II，代码逻辑和I一样，只是把求最大值和求模写成大数的运算：

```java
//我：注意要引入这个包
import java.math.BigInteger;
class Solution {
    public int cuttingRope(int n) {
        BigInteger dp[] = new BigInteger[n + 1];
        Arrays.fill(dp, BigInteger.valueOf(1));
        for (int i = 3; i <= n; i++) {
            for (int j = 1; j < i; j++) {
                dp[i] = dp[i].max(BigInteger.valueOf(j * (i - j))).max(dp[i - j].multiply(BigInteger.valueOf(j)));
            }
        }
        return  dp[n].mod(BigInteger.valueOf(1000000007)).intValue();
    }
}
```

- 我：注意biginteger里，max multiply valueof mod等的用法；所有的数都要转化成biginteger才能处理。
- 我：java基础知识里，BigDecimal用于精准处理大数（比如和钱相关的），提到过"推荐使用它的`BigDecimal(String val)`构造方法或者 `BigDecimal.valueOf(double val)` 静态方法来创建对象",就不会丢失进度。这里区分BigDecimal和biginteger；decimal指的是十进制，是允许小数的；而Biginteger只处理整数；[参考](http://t.csdn.cn/BRCAL)

#### 即时再战

本题频率低，且普适性一般，暂放

```java
class Solution {
    public int cuttingRope(int n) {
        long[] dp = new long[n + 1];
        dp[2] = 1;
        for(int i = 3; i < n + 1; i++){
            for(int j = 2; j < i; j++){
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return (dp[n])%(1e9+7);
    }
}
```



### [剑指 Offer 43. 1～n 整数中 1 出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

codetop==7

#### 首战寄

没有思路

#### 大佬-找规律做情况分类

思路来源：https://zhuanlan.zhihu.com/p/76031311

思路：

设N = abcde,其中abcde分别为十进制中各位上的数字。
如果要计算百位上1出现的次数，它要受到3方面的影响：百位上的数字，百位以下（低位）的数字，百位以上（高位）的数字。
**①如果百位上数字为0，百位上可能出现1的次数由更高位决定**。比如：12013，则可以知道百位出现1的情况可能是：100 ~ 199，1100 ~ 1199,2100 ~ 2199，，…，11100 ~ 11199，一共1200个。可以看出是由更高位数字（12）决定，并且等于更高位数字（12）乘以 当前位数（100）。
**② 如果百位上数字为1，百位上可能出现1的次数不仅受更高位影响还受低位影响**。比如：12113，则可以知道百位受高位影响出现的情况是：100 ~ 199，1100 ~ 1199,2100 ~ 2199，，….，11100 ~ 11199，一共1200个。和上面情况一样，并且等于更高位数字（12）乘以当前位数（100）。但同时它还受低位影响，百位出现1的情况是：12100~12113,一共114个，等于低位数字（113）+1。
**③ 如果百位上数字大于1（2 ~ 9），则百位上出现1的情况仅由更高位决定**，比如12213，则百位出现1的情况是：100 ~ 199,1100 ~ 1199，2100 ~ 2199，…，11100 ~ 11199,12100 ~ 12199,一共有1300个，并且等于更高位数字+1（12+1）乘以当前位数（100）。

- 思路[参考](https://www.nowcoder.com/profile/3371548/codeBookDetail?submissionId=16319486)

代码：

```java
大佬的代码没通过所有测试案例。。不过也不细究了，hard暂缓
class Solution {
    public int countDigitOne(int n) {
        
    //1的个数
    int count = 0;

    //当前位
    int i = 1;

    int current, after, before;

    while((n/i)!= 0){

        //当前位数字
        current = (n/i)%10;
        //高位数字
        before = n/(i*10);
        //低位数字
        after = n-(n/i)*i;

        //如果为0,出现1的次数由高位决定,数量等于高位数字 * 当前位数
        if (current == 0) {
            count += before * i;
        } else if(current == 1) {
            //如果为1,出现1的次数由高位和低位决定,高位*当前位+低位+1
            count += before * i + after + 1;
        } else{
            //如果大于1,出现1的次数由高位决定,（高位数字+1）* 当前位数
            count += (before + 1) * i;
        }
        //前移一位
        i = i*10;
    }
    return count;

    }
}
```

### [剑指 Offer 44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

codetop==2

#### 首战寄

数学真顶，，

#### 官方

暂略



## 模拟(中等)

### [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

codetop 54

#### 首战成功

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        /**
        每次触及边界，就让该方向能触及的边界内缩一位。top，bottom，left，right；
        用四个小while控制顺时针打印
        用一个大while包裹四个小while，每次小while结束都会侦测是否要退出大while（up和buttom交叉，left和right交叉）
         */
        //处理特殊情况
        if(matrix.length==0)return new int[0];

        //限制顺时针访问能到达的边界
        int top=0,bottom=matrix.length-1,left=0,right=matrix[0].length-1;
        //保存结果的数组
        int[] result=new int[matrix.length*matrix[0].length];
        //ij是二维数组中要打印的当前元素；count是结果数组的待点入坐标，并且起到了计数功能
        int i=0,j=0,count=0;
        //大while保持顺时针
        while(top<=bottom&&left<=right){
            //四个小while控制顺时针的方向
            while(j<=right){//先右移动
                result[count]=matrix[i][j];
                j++;
                count++;
            }
            //可能走到中途就要break
            if(count==result.length)break;
            //退出的时候多做了一次j加法使j超过了right，要减少回来,使j停留在边界right，准备向下移动;count不需要减少，因为它得停留在下一次要存储的位置
            j--;
            //向右处理完了，top要++，下次不能再触及这一行了
            top++;
            //下一波往下走
            i++;
            
            while(i<=bottom){//再向下移动
                result[count]=matrix[i][j];
                i++;
                count++;
            }
            //可能走到中途就要break
            if(count==result.length)break;
            //退出的时候多做了一次i加法使i超过了bottom，要减少回来,使i停留在边界bottom，准备向左移动;count不需要减少，因为它得停留在下一次要存储的位置
            i--;
            //向右处理完了，right要--，下次不能再触及这一列了
            right--;
            //下一波往左走
            j--;
            
            
            while(j>=left){//再向左移动
                result[count]=matrix[i][j];
                j--;
                count++;
            }
            //可能走到中途就要break
            if(count==result.length)break;
            //退出的时候多做了一次j减法使j超过了left，要增加回来,使j停留在边界left，准备向上移动;count不需要减少，因为它得停留在下一次要存储的位置
            j++;
            //向左处理完了，bottom要--，下次不能再触及这一行了
            bottom--;
            //下一波往上走
            i--;
            
            while(i>=top){//再向上移动
                result[count]=matrix[i][j];
                i--;
                count++;
            }
            //可能走到中途就要break
            if(count==result.length)break;
            //退出的时候多做了一次i减法使i超过了top，要减少回来，使i停留在边界top，准备向右移动;count不需要减少，因为它得停留在下一次要存储的位置
            i++;
            //向上处理完了，left要++，下次不能再触及这一列了
            left++;
            //下一波往右走
            j++;
            
        }

        return result;
    }
}
```

- 注意：i代表行，j代表列；想要向右移动的话，i不变，j增加。

#### 官方-模拟+设定边界

解题思路：

![image-20220416141340955](lcof.assets/image-20220416141340955.png)

算法流程：

![image-20220416141406403](lcof.assets/image-20220416141406403.png)

复杂度分析：

![image-20220416141422173](lcof.assets/image-20220416141422173.png)

代码：

> Java 代码利用了 ++ 操作的便利性，详情可见 ++i 和 i++ 的区别 ；
>
> res[x++] 等价于先给 res[x] 赋值，再给 x 自增 11 ；
> ++t > b 等价于先给 t 自增 11 ，再判断 t > b 逻辑表达式。

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0) return new int[0];
        int l = 0, r = matrix[0].length - 1, t = 0, b = matrix.length - 1, x = 0;
        int[] res = new int[(r + 1) * (b + 1)];
        while(true) {
            for(int i = l; i <= r; i++) res[x++] = matrix[t][i]; // left to right.
            if(++t > b) break;
            for(int i = t; i <= b; i++) res[x++] = matrix[i][r]; // top to bottom.
            if(l > --r) break;
            for(int i = r; i >= l; i--) res[x++] = matrix[b][i]; // right to left.
            if(t > --b) break;
            for(int i = b; i >= t; i--) res[x++] = matrix[i][l]; // bottom to top.
            if(++l > r) break;
        }
        return res;
    }
}
```

- 网友问：有一个疑惑，什么叫模拟？
  - 网友答：就是代码的思路是跟着实际要求的顺时针的思路来走的

- 我：自己写的版本和官方思路一致。

### [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

codetop==11

#### 首战寄 

没什么思路

#### 官方-模拟

解题思路：

如下图所示，给定一个压入序列 pushed和弹出序列 popped，则压入 / 弹出操作的顺序（即排列）是 **唯一确定** 的。

![Picture1.png](lcof.assets/c880f045c03a8e03b7908b2d49b658a9a32ba8f5d40cb19da62db32c7eb58830-Picture1.png)

如下图所示，栈的数据操作具有 **先入后出** 的特性，因此某些弹出序列是无法实现的。

![Picture2.png](lcof.assets/4397f5b44038603d70568147824877cacdaa76cef22371c2c80ff55f915092fd-Picture2.png)

考虑借用一个辅助栈 stackstack ，模拟 压入 / 弹出操作的排列。根据是否模拟成功，即可得到结果。

- 入栈操作： 按照压栈序列的顺序执行。
- 出栈操作： 每次入栈后，循环判断 “栈顶元素 == 弹出序列的当前元素” 是否成立，将符合弹出序列顺序的栈顶元素全部弹出。

> 由于题目规定 "栈的所有数字均不相等" ，因此在循环入栈中，每个元素出栈的位置的可能性是唯一的（若有重复数字，则具有多个可出栈的位置）。因而，在遇到 “栈顶元素 == 弹出序列的当前元素” 就应立即执行出栈。

算法流程：

![image-20220416152859724](lcof.assets/image-20220416152859724.png)

- 不停按照入栈数组入栈，等到入栈的节点和出栈数组的顶部相同时，才有出栈的资格，这是很好理解的。使用模拟可以侦测到不合理的出入栈顺序。

复杂度分析：

![image-20220416152918099](lcof.assets/image-20220416152918099.png)

代码：

> 题目指出 pushed 是 popped 的排列 。因此，无需考虑 pushedpushed 和 poppedpopped 长度不同 或 包含元素不同 的情况。
>

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for(int num : pushed) {
            stack.push(num); // num 入栈
            while(!stack.isEmpty() && stack.peek() == popped[i]) { // 循环判断与出栈
                stack.pop();
                i++;
            }
        }
        return stack.isEmpty();
    }
}
```

- 网友：不过Stack不建议使用，可以使用Deque，Deque也包含了栈的push和pop方法

  - 网友问：为什么建议用 Deque 代替 Stack呢
    - 网友答：这个是Java历史遗留问题，Stack继承了Vector且是Vector的唯一子类，Vector逐渐被Java抛弃了，且Stack是实体类，Deque是一个通用接口，使用起来更灵活，你查Java api中java.util.Stack中官方文档也给出了使用Deque来代替Stack的建议。Deque一般用LinkedList实现

  - 网友追问： stack的方法，deque都可以用吗？
    - 网友答：可以的，你就把它当成个stack就行，方法通用

#### 即时再战成功

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        /**
        1. 一个节点只有先入栈才能出栈，所以根据入栈数组不停入栈。
        2. 入栈后，当入栈的元素是出栈数组的顶部时，不停出栈
        3. 出栈完毕后，继续入栈，看看能否继续有与出栈数组顶部相同的元素入栈

        使用deque实现栈，而不是过时的stack
         */
        Deque<Integer> stack=new LinkedList<>();
        int pophead=0;
        for(int i=0;i<pushed.length;i++){
            //不停入栈
            stack.push(pushed[i]);
            //每次push后检查是否可以开始pop。注意要先保证stack不为空，再做peek等操作，否则可能出现空指针异常。integer在较小时是在常量池里，不过本题不碍事。
            while(!stack.isEmpty()&&stack.peek()==popped[pophead]){//退栈至不可再退为止，即栈顶元素和poped数组的当前元素不一致
                stack.pop();
                pophead++;
            }
        }
        
        //如果执行完入栈出栈后，stack不为空，说明poped[]是不合法的
        return stack.isEmpty();
    }
}
```



