# VUE

## 前言

### 学VUE之前要掌握的

#### 回顾ssm

见springmvc.md的结尾



#### 学vue之前要学js重点

讲vue之前要熟练掌握javascript，重点（只要）看：

- 操作bom和dom
- 函数，标准对象

<img src="Vue.assets/image-20211117181353090.png" alt="image-20211117181353090" style="zoom:60%;" />

#### 前端核心

逻辑

- 判断
- 循环

事件

- 浏览器事件:window，document

- Dom事件:增，删，遍历，修改节点元素内容

- jQuery

视图

- html

- CSS :难点BootStrap

通信

- ajax



想要成为真正的“互联网Java全栈工程师”还有很长的一段路要走，其中前端是绕不开的一门必修课。本阶段课程的主要目的就是带领Java后台程序员认识前端、了解前端、掌握前端，为实现成为“互联网Java全栈工程师”再向前迈进一步。



### 前端核心分析

#### vue概述

- vue遵守Soc原则：关注点分离原则

- Vue 的核心库只关注视图层，方便与第三方库或既有项目整合。
  - 视图 ：HTML + CSS + JS ，`给用户看，刷新后台给的数据`

- vue网络通信 ： axios

- vue页面跳转 ： vue-router

- vue状态管理：vuex

- Vue-UI : ICE , Element UI



#### 前端三要素

- HTML（结构）：超文本标记语言（Hyper Text Markup Language），决定网页的结构和内容
- CSS（表现）：层叠样式表（Cascading Style Sheets），设定网页的表现样式。
- JavaScript（行为）：是一种弱类型脚本语言，其源码不需经过编译，而是由浏览器解释运行，用于控制网页的行为



#### 结构层（HTML）

太简单，略



#### 表现层（CSS）

CSS层叠样式表是一门标记语言，并不是编程语言，因此不可以自定义变量，不可以引用等，换句话说就是不具备任何语法支持，它主要缺陷如下：

- 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器；
- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护；

这就导致了我们在工作中无端增加了许多工作量。为了解决这个问题，前端开发人员会使用一种称之为【CSS预处理器】的工具,提供CSS缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。大大的提高了前端在样式上的开发效率。



什么是CSS预处理器：

CSS预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只需要使用这种语言进行CSS的编码工作。转化成通俗易懂的话来说就是“**用一种专门的编程语言，进行Web页面样式设计，再通过编译器转化为正常的CSS文件，以供项目使用”**。

常用的CSS预处理器有哪些

- SASS：基于Ruby ，通过服务端处理，功能强大。解析效率高。需要学习Ruby语言，上手难度高于LESS。
- **LESS**：基于NodeJS，通过客户端处理，使用简单。功能比SASS简单，解析效率也低于SASS，但在实际开发中足够了，所以如果我们后台人员如果需要的话，建议使用LESS。
  - less的官网很简单。



#### 行为层（JavaScript）

JavaScript一门弱类型脚本语言，其源代码在发往客户端运行之前不需要经过编译，而是将文本格式的字符代码发送给浏览器，由浏览器解释运行。



Native 原生JS开发：

原生JS开发，也就是让我们按照【ECMAScript】标准的开发方式，简称ES，特点是所有浏览器都支持。截至到当前，ES标准以发布如下版本：

- ES3
- ES4（内部，未正式发布）
- ES5（全浏览器支持）
- ES6（常用，当前主流版本：webpack打包成为ES5支持）
- ES7
- ES8
- ES9（草案阶段）

区别就是逐步增加新特性。



TypeScript 微软的标准：

TypeScript是一种由微软开发的自由和开源的编程语言。它是JavaScript的一个超集， 而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯·海尔斯伯格(C#、Delphi、TypeScript之父； .NET创立者) 主导。该语言的特点就是除了具备ES的特性之外还纳入了许多不在标准范围内的新特性，所以会导致很多浏览器不能直接支持TypeScript语法， 需要编译后(编译成JS) 才能被浏览器正确执行。

 

JavaScript框架：

- JQuery：大家熟知的JavaScript库，优点就是简化了DOM操作，缺点就是DOM操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容IE6，7，8；
- Angular：Google收购的前端框架，由一群Java程序员开发，其特点是将后台的MVC模式搬到了前端并增加了**模块化开发**的理念，与微软合作，采用了TypeScript语法开发；对后台程序员友好，对前端程序员不太友好；最大的缺点是版本迭代不合理（如1代–>2 代，除了名字，基本就是两个东西）
- React：Facebook 出品，一款高性能的JS前端框架；特点是提出了新概念 【虚拟DOM】用于减少真实 DOM 操作，在内存中模拟 DOM操作，有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门【JSX】语言；
- Vue：一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。其特点是综合了 Angular（模块化）和React(虚拟 DOM) 的优点；
- Axios：前端通信框架；因为 Vue 的边界很明确（SOC原则），就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用jQuery 提供的AJAX 通信功能；、



### 前端发展史

#### UI框架

- Ant-Design：阿里巴巴出品，基于React的UI框架
- ElementUI、iview、ice：饿了么出品，基于Vue的UI框架
- BootStrap：Twitter推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子UI”，一款HTML5跨屏前端框架



#### JavaScript构建工具

- Babel：JS编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
- WebPack：模块打包器，主要作用就是打包、压缩、合并及按序加载

注：以上知识点已将WebApp开发所需技能全部梳理完毕



#### 三端同一

混合开发（Hybrid App）：

主要目的是实现一套代码三端统一（**PC**、**Android**：.apk、**iOS**：.ipa）并能够调用到设备底层硬件（如：传感器、GPS、摄像头等），打包方式主要有以下两种：

- 云打包：HBuild -> HBuildX，DCloud 出品；API Cloud
- 本地打包： Cordova（前身是 PhoneGap）



微信小程序：

详见微信官网，这里就是介绍一个方便微信小程序UI开发的框架：WeUI



#### 后端技术

前端人员为了方便开发也需要掌握一定的后端技术但我们Java后台人员知道后台知识体系极其庞大复杂，所以为了方便前端人员开发后台应用，就出现了Node JS这样的技术。Node JS的作者已经声称放弃Node JS(说是架构做的不好再加上笨重的node modules，可能让作者不爽了吧)开始开发全新架构的De no
既然是后台技术，那肯定也需要框架和项目管理工具， Node JS框架及项目管理工具如下：

- Express：Node JS框架
- Koa：Express简化版
- NPM：项目综合管理工具，类似于Maven
- YARN：NPM的替代方案，类似于Maven和Gradle的关系



#### 主流前端框架

Vue.js：

- iView

  iview是一个强大的基于Vue的UI库， 有很多实用的基础组件比element ui的组件更丰富， 主要服务于PC界面的中后台产品。使用单文件的Vue组件化开发模式基于npm+webpack+babel开发， 支持ES 2015高质量、功能丰富友好的API， 自由灵活地使用空间。

  - 官网地址

  - Github

  - iview-admin

  **备注：属于前端主流框架，选型时可考虑使用，主要特点是移动端支持较多**

- Element UI

  Element是饿了么前端开源维护的Vue UI组件库， 组件齐全， 基本涵盖后台所需的所有组件，文档讲解详细， 例子也很丰富。主要用于开发PC端的页面， 是一个质量比较高的Vue UI组件库。

  - 官网地址
  - Git hub
  - vue-element-admin

  **备注：属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多**

- ICE

  飞冰是阿里巴巴团队基于React/Angular/Vue的中后台应用解决方案， 在阿里巴巴内部， 已经有270多个来自几乎所有BU的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。

  - 官网地址。

  - Git hub

  **备注：主要组件还是以React为主， 截止2019年02月17日更新博客前对Vue的支持还不太完善，目前尚处于观望阶段**

- VantUI

  Vant UI是有赞前端团队基于有赞统一的规范实现的Vue组件库， 提供了-整套UI基础组件和业务组件。通过Vant， 可以快速搭建出风格统一的页面，提升开发效率。

  - 官网地址

  - Github

- AtUI

  at-ui是一款基于Vue 2.x的前端UI组件库， 主要用于快速开发PC网站产品。它提供了一套n pm+web pack+babel前端开发工作流程， CSS样式独立， 即使采用不同的框架实现都能保持统一的UI风格。

  - 官网地址
  - Git hub

- Cube Ul

  cube-ui是滴滴团队开发的基于Vue js实现的精致移动端组件库。支持按需引入和后编译， 轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发。

  - 官网地址

  - Github



混合开发：

- Flutter

  Flutter是谷歌的移动端UI框架， 可在极短的时间内构建Android和iOS上高质量的原生级应用。Flutter可与现有代码一起工作， 它被世界各地的开发者和组织使用， 并且Flutter是免费和开源的。

  - 官网地址

  - Github

  **备注：Google出品， 主要特点是快速构建原生APP应用程序， 如做混合应用该框架为必选框架**

- **lonic**

  lonic既是一个CSS框架也是一个Javascript UI库， lonic是目前最有潜力的一款HTML 5手机应用开发框架。通过SASS构建应用程序， 它提供了很多UI组件来帮助开发者开发强大的应用。它使用JavaScript MV VM框架和Angular JS/Vue来增强应用。提供数据的双向绑定， 使用它成为Web和移动开发者的共同选择。

  - 官网地址
  - 官网文档
  - Git hub



微信小程序：

- mpvue

  mpvue是美团开发的一个使用`Vue.js`开发小程序的前端框架， 目前支持微信小程序、百度智能小程序，头条小程序和支付宝小程序。框架基于`Vue.js`， 修改了的运行时框架`runtime`和代码编译器`compiler`实现， 使其可运行在小程序环境中， 从而为小程序开发引入了`Vue.js`开发体验。

  - 官网地址
  - Git hub

  **备注：完备的Vue开发体验， 井且支持多平台的小程序开发， 推荐使用**

- WeUI

  WeUI是一套同微信原生视觉体验一致的基础样式库， 由微信官方设计团队为微信内网页和微信小程序量身设计， 令用户的使用感知更加统一。包含button、cell、dialog、toast、article、icon等各式元素。

  - 官网地址

  - Github



### 了解前后分离的演变史

为什么需要前后分离

#### 后端为主的mvc时代

![image-20211119164625470](Vue.assets/image-20211119164625470.png)

为了降低开发的复杂度， 以后端为出发点， 比如：Struts、Spring MVC等框架的使用， 就是后端的MVC时代；
以SpringMVC流程为例：

- 发起请求到前端控制器(Dispatcher Servlet)
- 前端控制器请求HandlerMapping查找Handler，可以根据xml配置、注解进行查找
- 处理器映射器HandlerMapping向前端控制器返回Handler
- 前端控制器调用处理器适配器去执行Handler
- 处理器适配器去执行Handler
- Handler执行完成给适配器返回ModelAndView
- 处理器适配器向前端控制器返回ModelAndView，ModelAndView是SpringMvc框架的一个底层对象，包括Model和View
- 前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图(JSP)
- 视图解析器向前端控制器返回View
- 前端控制器进行视图渲染，视图渲染将模型数据(在ModelAndView对象中)填充到request域
- 前端控制器向用户响应结果

其中唯一要**自己写**的就是**handler**。



优点：

- MVC是一个非常好的协作模式， 能够有效降低代码的耦合度从架构上能够让开发者明白代码应该写在哪里。为了让View更纯粹， 还可以使用Thyme leaf、Frree marker等模板引擎， 使模板里无法写入Java代码， 让前后端分工更加清晰。

缺点

- 前端开发重度依赖开发环境，开发效率低，这种架构下，前后端协作有两种模式：
- 第一种是前端写DEMO， 写好后， 让后端去套模板。好处是DEMO可以本地开发， 很高效。不足是还需要后端套模板，有可能套错，套完后还需要前端确定，来回沟通调整的成本比较大；
- 另一种协作模式是前端负责浏览器端的所有开发和服务器端的View层模板开发。好处是UI相关的代码都是前端去写就好，后端不用太关注，不足就是前端开发重度绑定后端环境，环境成为影响前端开发效率的重要因素。
- 前后端职责纠缠不清：模板引擎功能强大，依旧可以通过拿到的上下文变量来实现各种业务逻辑。这样，只要前端弱势一点，往往就会被后端要求在模板层写出不少业务代码，还有一个很大的灰色地带是Controller， 页面路由等功能本应该是前端最关注的， 但却是由后端来实现。Controller本身与Model往往也会纠缠不清，看了让人咬牙的业务代码经常会出现在Controller层。这些问题不能全归结于程序员的素养， 否则JSP就够了。
- 对前端发挥的局限性：性能优化如果只在前端做空间非常有限，于是我们经常需要后端合作，但由于后端框架限制，我们很难使用[Comet】、【Big Pipe】等技术方案来优化性能。

注：在这期间(2005年以前) ， 包括早期的JSP、PHP可以称之为Web 1.0时代。在这里想说一句， 如果你是一名Java初学者， 请你不要再把一些陈旧的技术当回事了。比如**JSP早过时了**，甚至可以学习时一笔带过。因为时代在变、技术在变、什么都在变(引用扎克伯格的一句话：唯一不变的是变化本身)；当我们去给大学做实训时，有些同学会认为我们没有讲什么干货，其实不然，只能说是你认知里的干货对于市场来说早就过时了而已



#### 基于AJAX带来的SPA时代

时间回到2005年A OAX(Asynchronous JavaScript And XML， 异步JavaScript和XML，老技术新用法)被正式提出并开始使用CDN作为静态资源存储， 于是出现了JavaScript王者归来(在这之前JS都是用来在网页上贴狗皮膏药广告的) 的SPA(Single Page Application) 单页面应用时代。
![image-20211119165307823](Vue.assets/image-20211119165307823.png)

优点：

- 这种模式下， **前后端的分工非常清晰， 前后端的关键协作点是AJAX接口。**看起来是如此美妙， 但回过头来看看的话， 这与JSP时代区别不大。复杂度从服务端的JSP里移到了浏览器的JavaScript，浏览器端变得很复杂。类似Spring MVC， 这个时代开始出现浏览器端的分层架构：
  ![image-20211119165431906](Vue.assets/image-20211119165431906.png)

缺点：

- 前后端接口的约定：如果后端的接口一塌糊涂，如果后端的业务模型不够稳定，那么前端开发会很痛苦；不少团队也有类似尝试，通过接口规则、接口平台等方式来做。有了和后端一起沉淀的接口规则，还可以用来模拟数据，使得前后端可以在约定接口后实现高效并行开发。
- 前端开发的复杂度控制：SPA应用大多以功能交互型为主，JavaScript代码过十万行很正常。大量JS代码的组织，与View层的绑定等，都不是容易的事情。



#### 前端为主的MV*时代

此处的MV*模式如下：

- MVC(同步通信为主) ：Model、View、Controller

- MVP(异步通信为主) ：Model、View、Presenter

- MVVM(异步通信为主)：Model、View、View Model为了降低前端开发复杂度，涌现了大量的前端框架，比如：Angular JS、React、Vue.js、Ember JS等， 这些框架总的原则是先按类型分层， 比如Templates、Controllers、Models， 然后再在层内做切分，如下图：

  ![image-20211119170128281](Vue.assets/image-20211119170128281.png)

优点

- 前后端职责很清晰：前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难， 前端可以本地开发。后端则可以专注于业务逻辑的处理， 输出RESTful等接口。
- 前端开发的复杂度可控：前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本书的厚度去说明。
- 部署相对独立：可以快速改进产品体验

缺点

- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步， 对SEO不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA不能满足所有需求， 依旧存在大量多页面应用。URL Design需要后端配合， 前端无法完全掌控。



#### Node JS带来的全栈时代

前端为主的MV*模式解决了很多很多问题， 但如上所述， 依旧存在不少不足之处。随着Node JS的兴起， JavaScript开始有能力运行在服务端。这意味着可以有一种新的研发模式：

![在这里插入图片描述](Vue.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Bhbl9oMTk5NQ==,size_16,color_FFFFFF,t_70#pic_center.png)

在这种研发模式下，前后端的职责很清晰。对前端来说，两个UI层各司其职：

- Front-end Ul layer处理浏览器层的展现逻辑。通过CSS渲染样式， 通过JavaScript添加交互功能， HTML的生成也可以放在这层， 具体看应用场景。
- Back-end Ul layer处理路由、模板、数据获取、Cookie等。通过路由， 前端终于可以自主把控URL Design， 这样无论是单页面应用还是多页面应用， 前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过Node， WebServer层也是JavaScript代码， 这意味着部分代码可前后复用， 需要SEO的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。
与JSP模式相比， 全栈模式看起来是一种回归， 也的确是一种向原始开发模式的回归， 不过是一种螺旋上升式的回归。
基于Node JS的全栈模式， 依旧面临很多挑战：

- 需要前端对服务端编程有更进一步的认识。比如TCP/IP等网络知识的掌握。
- Node JS层与Java层的高效通信。Node JS模式下， 都在服务器端， RESTful HTTP通信未必高效， 通过SOAP等方式通信更高效。一切需要在验证中前行。
- 对部著、运维层面的熟练了解，需要更多知识点和实操经验。
- 大量历史遗留问题如何过渡。这可能是最大最大的阻力。
  

#### 总结

综上所述，模式也好，技术也罢，没有好坏优劣之分，只有适合不适合；前后分离的开发思想主要是基于`Soc`(关注度分离原则)，上面种种模式，都是让前后端的职责更清晰，分工更合理高效。



## VUE功能讲解

### 第一个vue程序

#### 什么是MVVM

MVVM（Model-View-ViewModel）是一种软件设计模式，由微软WPF（用于替代WinForm，以前就是用这个技术开发桌面应用程序的）和Silverlight（类似于Java Applet，简单点说就是在浏览器上运行WPF）的架构师Ken Cooper和Ted Peters开发，是一种简化用户界面的**事件驱动编程方式**。由John Gossman（同样也是WPF和Sliverlight的架构师）与2005年在他的博客上发表。

MVVM源自于经典的MVC（Model-View-Controller）模式。MVVM的核心是ViewModel层，负责转换Model中的数据对象来让数据变得更容易管理和使用。其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与Model层通过接口请求进行数据交互

![在这里插入图片描述](Vue.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Bhbl9oMTk5NQ==,size_16,color_FFFFFF,t_70.png)

MVVM已经相当成熟了，主要运用但不仅仅在网络应用程序开发中。当下流行的MVVM框架有`Vue.js`，`Anfular JS`

#### 为什么要使用MVVM

MVVM模式和MVC模式一样，主要目的是分离视图（View）和模型（Model），有几大好处:

- 低耦合：视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的View上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
- 可复用：你可以把一些视图逻辑放在一个ViewModel里面，让很多View重用这段视图逻辑。
- 独立开发：开发人员可以专注于业务逻辑和数据的开发（ViewMode），设计人员可以专注于页面设计。
- 可测试：界面素来是比较难以测试的，而现在测试可以针对ViewModel来写。

![在这里插入图片描述](Vue.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Bhbl9oMTk5NQ==,size_12,color_FFFFFF,t_70.png)



MVVM结构介绍：

- View

  View是视图层， 也就是用户界面。前端主要由HTH L和csS来构建， 为了更方便地展现vi eu to del或者Hodel层的数据， 已经产生了各种各样的前后端模板语言， 比如FreeMarker，Thyme leaf等等， 各大MV VM框架如Vue.js.Angular JS， EJS等也都有自己用来构建用户界面的内置模板语

- Model

  Model是指数据模型， 泛指后端进行的各种业务逻辑处理和数据操控， 主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的接口规则

- ViewModel

  ViewModel是由前端开发人员组织生成和维护的视图数据层。在这一层， 前端开发者对从后端获取的Model数据进行转换处理， 做二次封装， 以生成符合View层使用预期的视图数据模型。

  需要注意的是View Model所封装出来的数据模型包括视图的状态和行为两部分， 而Model层的数据模型是只包含状态的

  - 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态(展示)
  - 页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为(交互)

  视图状态和行为都封装在了View Model里。这样的封装使得View Model可以完整地去描述View层。**由于实现了双向绑定， View Model的内容会实时展现在View层**（前端页面不必刷新）， 这是激动人心的， 因为前端开发者再也不必低效又麻烦地通过操纵DOM去更新视图。

  MVVM框架已经把最脏最累的一块做好了， 我们开发者只需要处理和维护View Model， 更新数据视图就会自动得到相应更新，真正实现事件驱动编程。

  View层展现的不是Model层的数据， 而是ViewModel的数据， 由ViewModel负责与Model层交互， **这就完全解耦了View层和Model层， 这个解耦是至关重要的， 它是前后端分离方案实施的重要一环。**

#### Vue

Vue(读音/vju/， 类似于view) 是一套用于构建用户界面的渐进式框架， 发布于2014年2月。与其它大型框架不同的是， Vue被设计为可以自底向上逐层应用。Vue的核心库只关注视图层， 不仅易于上手， 还便于与第三方库(如：vue-router，vue-resource，vue x) 或既有项目整合。

1）MVVM模式的实现者：

- Model：模型层， 在这里表示JavaScript对象
- View：视图层， 在这里表示DOM(HTML操作的元素)
- ViewModel：连接视图和数据的中间件， Vue.js就是MVVM中的View Model层的实现者

在MVVM架构中， 是不允许数据和视图直接通信的， 只能通过ViewModel来通信， 而View Model就是定义了一个Observer观察者

- ViewModel能够观察到数据的变化， 并对视图对应的内容进行更新
- ViewModel能够监听到视图的变化， 并能够通知数据发生改变

至此， 我们就明白了， Vue.js就是一个MVVM的实现者， 他的核心就是实现了DOM监听与数据绑定
2）为什么要使用Vue.js

- 轻量级， 体积小是一个重要指标。Vue.js压缩后有只有20多kb(Angular压缩后56kb+，React压缩后44kb+)
- 移动优先。更适合移动端， 比如移动端的Touch事件
- 易上手，学习曲线平稳，文档齐全
- 吸取了Angular(模块化) 和React(虚拟DOＭ) 的长处， 并拥有自己独特的功能，如：计算属性
- 开源，社区活跃度高



#### 实战准备

【说明】IDEA可以安装Vue的插件!

- 注意：Vue不支持IE 8及以下版本， 因为Vue使用了IE 8无法模拟的ECMAScript 5特性。但它支持所有兼容ECMAScript 5的浏览器。
- 前端用vscode编写的比较多，但是idea已经足够万能了，这里我们用idea。
  - idea开发vue之前必须先安装vue.js插件



vue下载地址:

- 开发版本:

  - 包含完整的警告和调试模式：`https：//yuejs.org/js/vue.js`

  - 删除了警告， 30.96KB min+gzip：`https：//vuejs.org/js/vue.min.js`

- CDN:

  - 用cdn方式使用vue的话，不需要从官网下载vue.js更方便

  ```html
  <script src=“https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js”></script>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
  ```



#### 实战

新建vue文件夹

- 由于前端很简单简洁，用一个清爽的文件夹开始，项目打开更快。

![image-20211120111646100](Vue.assets/image-20211120111646100.png)

idea中打开文件夹

![image-20211120113333715](Vue.assets/image-20211120113333715.png)

![image-20211120113351091](Vue.assets/image-20211120113351091.png)

![image-20211120113419946](Vue.assets/image-20211120113419946.png)

idea下载vue插件

![image-20211120133634647](Vue.assets/image-20211120133634647.png)

![image-20211120133710185](Vue.assets/image-20211120133710185.png)

![image-20211120134254785](Vue.assets/image-20211120134254785.png)

自己手动创建一个XXX.vue文件，激活idea的新建vue-component的功能

![image-20211120134528777](Vue.assets/image-20211120134528777.png)

![image-20211120134547536](Vue.assets/image-20211120134547536.png)

先不管新建vue-component，我们创建一个最简单的html来实现vue（VM功能）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--编写前端即view层-->
<div id="app">
<!--3. 第三步前端从data中拿数据-->
{{message}}
</div>


<!--使用cdn的方式导入vue.js轻量版-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<!--创建VM对象，沟通Model和view-->
<script>
    //Vue()是构造函数，括号中放置类似json字符串的大括号及值,值之间以逗号分割。可以创建多个vm来绑定多个前端组件。
    var vm = new Vue({
        //1.第一步绑定前端组件：el表示element即元素，用于绑定前端组件的id
        el: "#app",
        //2. 第二步data放置数据：前后端分离，data要交给前端控制,此为model层
        data:{
            message: "hello, vue!"
        }
    });
</script>

</body>
</html>
```

使用google浏览器打开

![image-20211120141527882](Vue.assets/image-20211120141527882.png)

前端成功展示了vm的data中存的数据

![image-20211120141708575](Vue.assets/image-20211120141708575.png)

现在来检查vue的VM的双向绑定的功能

通过console修改vm.data改变前端的值

- 不使用vue的话，前端不可能变，因为前后端没绑定。html写死了不能变数据。
- 但是vue中，VM层绑定了前端和后端，前端是根据vm.data来的，所以修改后端前端也会实时变化
-  在这个操作中， 我并没有主动操作DOM， 就让页面的内容发生了变化， 这就是借助了Vue的数据绑定功能实现的； MV VM模式中要求View Model层就是使用观察者模式来实现数据的监听与绑定， 以做到数据与视图的快速响应。

![image-20211120142229892](Vue.assets/image-20211120142229892.png)

![image-20211120142254331](Vue.assets/image-20211120142254331.png)





### vue基本语法

#### v-bind

我们已经成功创建了第一个Vue应用!看起来这跟渲染一个字符串模板非常类似， 但是Vue在背后做了大量工作。现在数据和DOM已经被建立了关联， 所有东西都是响应式的。我们在控制台操作对象属性，界面可以实时更新!

我们还可以使用v-bind来绑定元素特性!

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--编写前端即view层-->
<div id="app">
    <!--3. 第三步前端从data中拿数据-->
    <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
    </span>
</div>


<!--使用cdn的方式导入vue.js轻量版-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<!--创建VM对象，沟通Model和view-->
<script>
    //Vue()是构造函数，括号中放置类似json字符串的大括号及值,值之间以逗号分割。可以创建多个vm来绑定多个前端组件。
    var vm = new Vue({
        //1.第一步绑定前端组件：el表示element即元素，用于绑定前端组件的id
        el: "#app",
        /*2. 第二步data放置数据：前后端分离，data要交给前端控制,此为model层。后端就算不给数据，前端也可以在这里放个模拟数据，也是实现
        前后端分离的一种形式*/
        data:{
            message: "这里动态绑定了hello, vue!"
        }
    });
</script>

</body>
</html>
```

查看页面效果

![image-20211120155926322](Vue.assets/image-20211120155926322.png)

你看到的v-bind等被称为指令。指令带有前缀v以表示它们是Vue提供的特殊特性。可能你已经猜到了， 它们会在渲染的DOM上应用特殊的响应式行为在这里，该指令的意思是：“将这个元素节点的title特性和Vue实例的message属性保持一致”。

如果你再次打开浏览器的JavaScript控制台， 输入app， message=‘新消息’，就会再一次看到这个绑定了title特性的HTML已经进行了更新。




#### v-if,v-else,v-else-if

什么是条件判断语句，就不需要我说明了吧，以下两个属性!

- `v-if`
- `v-else`

演示代码如下

```html
<!DOCTYPE html>
<!--下面这行的xmln在某些版本情况下可以省略-->
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--view层，模板-->
<div id="app">
  <h1 v-if="ok">Yes</h1>
  <h1 v-else>No</h1>

</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
  var vm = new Vue({
    el:"#app",
    /*Model：数据*/
    data:{
      ok: true
    }
  });
</script>
</body>
</html>
```

![image-20211120160944994](Vue.assets/image-20211120160944994.png)

现在人为修改VM中的data来改变前端的显示

![image-20211120161048548](Vue.assets/image-20211120161048548.png)

![image-20211120161139976](Vue.assets/image-20211120161139976.png)

现在尝试使用`v-else-if`标签

- js中，双等号 数据类型不一样 光他的值一样 也算true； 而三等号 要求数据类型也得一样
- 注意：js中字符要用单引号包裹，字符串要用双引号包裹

```html
<!DOCTYPE html>
<!--下面这行的xmln在某些版本情况下可以省略-->
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--view层，模板-->
<div id="app">
  <h1 v-if="ok">Yes</h1>
  <h1 v-else>No</h1>
</div>
<div id="app1">
  <h1 v-if="type===`A`">A</h1>
  <h1 v-else-if="type===`B`">B</h1>
  <h1 v-else-if="type===`C`">C</h1>
  <h1 v-else>D</h1>
</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
  var vm = new Vue({
    el:"#app",
    /*Model：数据*/
    data:{
      ok: true
    }
  });

  var vm1 = new Vue({
    el:"#app1",
    /*Model：数据*/
    data:{
      type: `B`
    }
  });
</script>
</body>
</html>
```

![image-20211120162604805](Vue.assets/image-20211120162604805.png)

现在尝试改变VM中的data来改变前端的显示

![image-20211120162818131](Vue.assets/image-20211120162818131.png)

![image-20211120162941270](Vue.assets/image-20211120162941270.png)

#### v-for

格式说明

```html
<div id="app">
    <li v-for="(item,index) in items">
        {{item.message}}---{{index}}
    </li>

</div>
123456
```

- 注：`items`是数组，`item`是数组元素迭代的别名。我们之后学习的Thymeleaf模板引擎的语法和这个十分的相似！

代码实战：

这里数组不太对吧 这更像是多个对象 应改为 items:['一','二','三'] 然后遍历那里改为{{item}}

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--view层，模板-->
<div id="app">
  <li v-for="(item,index) in items">
<!--    这个index表示item在items中是第几位，默认的-->
    {{item.message}}---{{index}}
  </li>

</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
  var vm = new Vue({
    el:"#app",
    /*Model：数据*/
    data:{
      items:[
        {message:'张云Java'},
        {message:'张云学前端'},
        {message:'张云学运维'}
      ]
    }
  });
</script>
</body>
</html>

```

![image-20211120180551309](Vue.assets/image-20211120180551309.png)

尝试使用VM修改data来修改前端展示（不需刷新页面）

![image-20211120180928147](Vue.assets/image-20211120180928147.png)

![image-20211120180952573](Vue.assets/image-20211120180952573.png)

![image-20211120181204963](Vue.assets/image-20211120181204963.png)



### vue绑定事件

#### v-on

emsp;事件有Vue的事件、和前端页面本身的一些事件!我们这里的`click`是vue的事件， 可以绑定到Vue中的`methods`中的方法事件!

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<div id="app">
  <button v-on:click="sayHi">点我</button>
</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
  // vue有七大对象：el，data，template，methods，render，computed，watch；除了七大属性还有别的属性只不过这七大更重要。
  var vm = new Vue({
    el:"#app",
    data:{
      message:'Hello World'
    },
    //不要把methods写成method
    methods:{//方法必须定义在Vue的method对象中
      sayHi:function(){
      //'this'在方法里面指向当前Vue实例。this.message即为'Hello World'
      alert(this.message);
      }
    }
  });
</script>
</body>
</html>

```

![image-20211120205546403](Vue.assets/image-20211120205546403.png)

Vue还有一些基本的使用方式， 大家有需要的可以再跟着官方文档看看， 因为这些基本的指令几乎我们都见过了，一通百通!掌握学习的方式!



#### 彩蛋：vue功能搬上服务器

登录xshell，执行下列语句，查看tomcat主要出现在哪些目录，从而找到webinf所在目录

```bash
[root@zhangyun ~]# sudo find / -name *tomcat*
```

![image-20211120212458728](Vue.assets/image-20211120212458728.png)

来到root目录，发现确实有标志性的“index.js"和”webinf文件夹“。所以把上节写的von.html放置在Root目录下

![image-20211120212632096](Vue.assets/image-20211120212632096.png)

![image-20211120212804481](Vue.assets/image-20211120212804481.png)

访问：http://120.53.244.17:8080/von.html。即在外网看到自己写的页面。

![image-20211120212916694](Vue.assets/image-20211120212916694.png)



### Vue双向绑定

https://www.bilibili.com/video/BV18E411a7mC?p=7&spm_id_from=pageDriver

https://blog.csdn.net/qq_46138160/article/details/111028492

