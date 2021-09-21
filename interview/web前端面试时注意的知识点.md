# 前端面试要求我们一定要熟练掌握js！！！

## 2020/4/10 中移物联网笔试

- html链接中，src和href的区别

src表示引用资源，表示替换当前元素，用在img，script，iframe上，src是页面内容不可缺少的一部分（必须显示他页面才完整）<br/>
href标识超文本引用，用在link和a等元素上，href是引用和页面关联，是在当前元素和引用资源之间建立联系<br/>
[原网站戳我](https://blog.csdn.net/binlety/article/details/81448195)

- http请求的一种不包含那个？：get post trace fetch

八种请求：get，post，put， options，head，delete，trace，connect。<br/>
但是常用的就是get post，因为其他的请求都可以通过get post间接实现<br/>
[原网址戳我](https://www.cnblogs.com/baiqiantao/p/7672471.html)<br/>

- 谷歌浏览器的内核是什么？

webkit

- 常见浏览器的内核有哪些？

(1)Trident内核（微软ie）<br/>
(2)Gecko内核（火狐）<br/>
(3)WebKit内核（谷歌）<br/>
(4)Blink内核（谷歌和opera合办）这一渲染引擎是开源引擎WebKit中WebCore组件的一个分支，并且在Chrome（28及往后版本）、Opera（15及往后版本）中使用。<br/>

- 行内元素是什么？

元素分为行内元素和块状元素<br/>
行内元素只能在显示页面中的一行使用，不能横跨多行。比如：em – 强调，br – 换行，i – 斜体，img – 图片，input – 输入框<br/>
块状元素可以横跨多行。比如：form – 交互表单，h1 – 大标题，dir – 目录列表，div – 常用块级容易，也是css layout的主要标签<br/>
[原网址戳我](https://blog.csdn.net/web_ys/article/details/54629298)
- 那些元素可以嵌套

块元素可以包含内联（行内）元素或`某些`块元素（不是所有块元素都可以包含在另一个块元素内，比如div不能包含在p内）。<br/>
但内联（行内）元素却不能包含块元素，只能包含其他的内联元素。<br/>
[原网址戳我](https://blog.csdn.net/weixin_45761317/article/details/103103062)<br/>

- js中属于引用数据类型的有哪些？

js的六大数据类型：Number, String, Boolean, Undefined , Null , Object<br/>
基本数据类型：Number,String,Boolean,Undefined, Null。储存的是值，赋值之后相互之间不影响<br/>
引用数据类型：Object , Array, Function。储存的是引用，相互之间有影响。<br/><br/>

首先要明确ECMAScript中所有的函数的参数都是按值来传递的。<br/>
变量存储的基本类型的值只是把值传递给参数之后参数和这个变量互不影响。
变量存储的引用数据类型值存储的是该引用值在堆内存中的内存地址，因此传递的值就是这个内存地址。<br/>
[原文戳我](https://blog.csdn.net/sunday97/article/details/84869727)

- 添加css样式的方法有哪些？比如内联，外联。。

JSP页面插入CSS样式有三种方法，且其优先级不同。优先级：内联样式 > 内部样式 >外部样式！<br/>
【1】外部样式<br/>
在使用外部样式表的情况下，你可以通过改变一个文件来改变整个`站点`的外观。每个页面使用<link>标签链接到样式表。<br/>
`<link rel="stylesheet" type="text/css" href="<%=uiPath%>hwtt_ui/skins/<%=skinName%>/css/login.css" />`<br/>
rel 属性规定当前文档与被链接文档之间的关系，在该例子中rel 属性指示被链接的文档是一个样式表<br/>
type是当前元素本身的功能<br/>
href指定的是当前元素关联的文件的链接<br/><br/>

【2】内部样式<br/>
当单个`文档`需要特殊的样式时，就应该使用内部样式表。可以使用<style>标签在文档头部定义内部样式表。<br/>
  ```
   <style type="text/css">
        .loginBtn{
            display:block;
            cursor: pointer;
            height: 32px;
            margin-bottom: 1px;
            width: 100px;
        }
    </style>
  ```

【3】内联样式（inline style）<br/>
由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势。请慎用这种方法，例如当样式仅需要在`一个元素上应用一次`时。要使用内联样式，你需要在相关的标签内使用样式（style）属性。Style属性可以包含任何CSS属性。<br/>
`<input type="text" name="authCode" style="vertical-align: middle" />`<br/>
type是当前元素本身的功能<br/>
name是为了之后设置css样式时方便引用<br/>
style直接设置了css样式<br/><br/>

[原链接戳我](https://blog.csdn.net/J080624/article/details/69557267?depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-5&utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-5)<br/><br/>

【4】如果插入的不是css而是js，则用<script> js们 <script/>在html中插入


## 和谁学，等一些公司的前端问题
- ul有序，ol无序吗？

列表(ul无序ol有序)。他们的全名分别是：unordered list，ordered list
```
<ul type="disc">  <!-- 实心圆 -->
        <li>早上</li>
        <li>中午</li>
        <li>晚上</li>
</ul>
 
 <ol type="A" start="5">  <!-- 显示英文字母 从E开始 -->
        <li>香蕉</li>
        <li>苹果</li>
        <li>橙子</li>
 </ol>
```

- http的特点，他和https的区别
  - 什么是HTTP：`超文本传输协议`，是一个基于请求与响应，无状态的，应用层的协议，常基于`TCP/IP协议传输数据`，互联网上应用最为广泛的一种网络协议,所有的WWW文件都必须遵守这个标准。设计HTTP的初衷是为了提供一种发布和接收`HTML页面`的方法。和https比，http是原始的，不安全的，因为他会收集密码和信用卡信息
  - 什么是HTTPS：《图解HTTP》这本书中曾提过`HTTPS是身披SSL外壳的HTTP`。HTTPS是一种通过计算机网络进行安全通信的传输协议，经由HTTP进行通信，利用SSL/TLS建立全信道，`加密数据包`。HTTPS使用的主要目的是提供对网站服务器的身份认证，同时保护交换数据的隐私与完整性。<br/>
PS:TLS是传输层加密协议，前身是SSL协议，由网景公司1995年发布，有时候两者不区分。



- 一个高为k的二叉树，他最多有2^k-1个节点吗？以及类似和二叉树深度，节点相关的公式类的题目

- 浏览器的两种储存方式
 
  我们从前端的角度了解一下浏览器存储，我们常见且常用的存储方式主要由两种：cookie、webStorage（localStorage和sessionStorage）。下面我们来一一认识它们
  - cookie
  
    Cookie基于HTTP规范，用来识别用户。<br/>
    Cookie是服务器发送到浏览器的一小段数据，会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。<br/>
    Cookie诞生之初的作用就是解决HTTP的无状态请求，用来`记录一些用户相关的一些状态`。
    - 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
    - 个性化设置（如用户自定义设置、主题等）
    - 浏览器行为跟踪（如跟踪分析用户行为等）<br/>
    
    因为一些前端交互的需要，后来cookie也被用于`存储一些客户端的数据`。<br/>
    Cookie的原生api不友好，需要自行封装一下。下面是封装后的方法。

  - webstorage
  
    HTML5 提供了两种在客户端存储数据的新方法：`localStorage`和`sessionStorage`，挂载在window对象下。
    `webStorage`是`本地`存储，数据不是由服务器请求传递的。从而它可以存储大量的数据，而不影响网站的性能。
    Web Storage的目的是为了克服由cookie带来的一些限制，当`数据`需要被严格控制在`客户端`上时，无须持续地将数据发回服务器。比如客户端需要保存的一些用户行为或数据，或从接口获取的一些短期内不会更新的数据，我们就可以利用Web Storage来存储。
    - localStorage的生命周期是永久性的。localStorage存储的数据，即使关闭浏览器，也不会让数据消失，除非主动的去删除数据。如果 想设置失效时间，需自行封装。
    - sessionStorage 的生命周期是在浏览器关闭前。<br/>
　　  特性：<br/>
      关闭浏览器sessionStorage 失效；<br/>
      页面刷新不会消除数据；<br/>
      只有在当前页面打开的链接，才可以访sessionStorage的数据，使用window.open打开页面和改变localtion.href方式都可以获取到


- git add是把文件保存到git的缓存区吗？

  工作目录-(git add)->暂存区-(git commit)->数据目录-(git push)->git hub仓库
  
#### 网易，树根
1. flex布局调整使列

2. 流布局怎样的，瀑布布局又怎样

3. 清除float怎么实现

4. 浏览器localstorage的api

- span是不是块状布局<br/>
不是

5. 同步和异步
