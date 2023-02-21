# Redis

”redis设计与实现讲的很细“

## 前言

学习方式

- 上手就用（码农）
- 基本的理论先学习，然后将知识融会贯通（软件工程师）



redis课程安排：

- nosql讲解
- 阿里巴巴架构演进
- nosql数据模型
- Nosql四大分类
- CAP
- BASE
- Redis入门
- Redis安装(Window & Linux服务器)
- 五大基本数据类型
  - string
  - List
  - set
  - Hash
  - Zset
- 三种特殊数据类型
  - geo
  - hyperloglog
  - bitmap
- Redis配置详解
- Redis持久化
  - RDB
  - AOF
- Redis事务操作
- Redis实现订阅发布（消息队列）
- Redis主从复制
- Redis哨兵模式（现在公司中所有的集群都用哨兵模式）
- 缓存穿透及解决方案
- 缓存击穿及解决方案
- 缓存雪崩及解决方案
- 基础API之Jedis详解
- SpringBoot集成Redis操作
- Redis的实践分析



## NoSQL概述

### 为什么要用NoSQL

我们现在处于2021年，大数据时代。大数据一般的数据库无法进行分析处理了；2006年hadoop出现。至于java框架，现在最低要求是springboot+springcloud。



#### 单机mysql的年代

![image-20211110110329777](redis.assets/image-20211110110329777.png)

90年代，一个基本的网站访问量不会太大，单个数据库完全足够。哪个时候，更多的使用静态网页html，服务器没有太大的压力。

思考一下，那种情况下，整个网站的瓶颈是什么？

- 数据量如果太大，一个机器放不下
- 数据的索引（B+Tree），一个机器内存也放不下
  - 数据量超过300万的时候必须建立索引，否则查询会很慢。
- 访问量（读写混合），一个服务器承受不了

只要出现以上的三个情况之一，那么就必须谨记。



#### Memcached（缓存）+mysql+垂直拆分（读写分离）

网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦! 所以说我们想减轻数据的压力，我们可以使用缓存来保证效率。

发展过程：优化数据结构和索引--》文件缓存（IO）--》Memcached（当时最热门的技术）

![image-20211110111502836](redis.assets/image-20211110111502836.png)



#### 分库分表+水平拆分+mysql集群

一点点的，技术规模变得越来越大。

- M代表负责“写”操作的数据库
- S代表负责“读”操作的数据库

**本质：数据库（读，写）**

早些年MyISAM：表锁，十分影响效率，高并发下就会出现比较严重的问题

早些年Innodb：行锁，效率比表锁高

慢慢的就开始使用分库分表来解决写的压力！Mysql在那个年代退出了表分区，但是并没有很多公司使用。同时Mysql推出了集群，很好的满足了那个年代的所有需求。

![image-20211110112136995](redis.assets/image-20211110112136995.png)



#### 最近的年代

技术爆炸，2010-2021，十年之间，世界发生了翻天覆地的变化。Mysql等关系型数据库就不够用了，数据量很多，变化快。MySQL有的使用它来村粗一些比较大的文件，博客，图片!数据库表很大，效率就低了!如果有一种数据库来专门处理这种数据,MySQL压力就变得十分小(研究如何处理这些问题!)大数据的IO压力下，表几乎没法更大!

目前一个基本的互联网项目：

![image-20211110120239459](redis.assets/image-20211110120239459.png)



#### 为什么要用NoSQL

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长，关系型数据库达到瓶颈了。这时候我们就需要使用NosQL数据库，NoSQL可以很好的处理以上的情况。



### 什么是NoSQL

#### NoSQL

NoSQL = Not Only SQL（不仅仅是SQL)

关系型数据库:表格，行，列

NoSQL泛指非关系型数据库的，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代。尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题。NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的，而且是我们当下必须要掌握的一个技术!

很多的数据类型用户的个人信息，社交网络，地理位置；这些数据类型的存储不需要一个固定的格式，不需要多月的操作就可以横向扩展的。Map<String,Object>使用键值对来控制，限定了key的类型，但object的类型随意，这就是典型的NoSQL思想。



#### NoSQL特点

1. 方便扩展（数据之间没有关系，很好扩展!)

2. 大数据量高性能(Redis一秒写8万次，读取11万，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高!)

3. 数据类型是多样型的!(不需要事先设计数据库!随取随用!如果是数据量十分大的表，很多人就无法设计了!)

4. 传统RDBMS和NoSQL
   传统的RDBMS

   - 结构化组织
   - sQL为查询语言
   - 固定的存储方式：数据和关系都存在单独的表中
   - 操作操作，数据定义语言
   - 严格的一致性（分布式计算系统的CAP中的C）
   - 基础的事务
   - 。。。

   Nosql

   - nosql意味不仅仅是数据
   - 没有固定的查询语言
   - 四种存储方式：键值对存储，列存储，文档存储，图形数据库（社交关系）
   - 最终一致性
   - CAP定理，BASE理论（异地多活）
   - 高性能，高可用，高可扩展



#### 3V+3高

大数据时代的3V:主要是描述问题的

1. 海量Volume
2. 多样Variety
3. 实时Velocity

大数据时代的3高:主要是对程序的要求

1. 高并发
2. 高可拓（随时水平拆分，机器不够了，可以扩展机器来）
3. 高性能



真正在公司中的实践:**NoSQL+ RDBMS一起使用**才是最强的，阿里巴巴的架构演进。技术没有高低之分，就看你如何去使用（提升内功，思维的提高)



### 阿里巴巴实践分析理解数据架构演进

思考问题:这么多东西难道都是在一个数据库中吗？网站中数据类型有视频，图片，评论；有些数据要经常改，有些不用经常改。

![image-20211110160914916](redis.assets/image-20211110160914916.png)

技术急不得，越是慢慢学，才能越扎实!

开源才是技术的王道!

**任何一家互联网的公司，都不可能只是简简单单让用户能用就好了!**大量公司做的都是相同的业务;(竞品协议)

随着这样的竞争，业务是越来越完善，然后对于开发者的要求也是越来越高!

从研一开始就要好好学习！



阿里巴巴数据架构：

![image-20211110163037292](redis.assets/image-20211110163037292.png)

如果未来做一个架构师，没有什么是加一层解决不了的。

一个简单的网页背后的技术很多：

1. 商品的基本信息

   - 名称、价格、商家信息;

   - 关系型数据库就可以解决了! MysqL / oracle（淘宝早年就去TOE了! -王坚:推荐文章:阿里云的这群疯子:40分钟重要!听说看完会爱上阿里巴巴)

   - 淘宝内部的MysQL不是大家用的MysQL，很多组件都被改了。

2. 商品的描述、评论（文字比较多)
   - 文档型数据库中，MongoDB

3. 图片

   - 分布式文件系统FastDFS

   - 淘宝自己的TFS
   - google的GFS
   - hadoop的HDFS
   - 阿里云的oss

4. 商品的关键字（搜索）

   - 搜素引擎 solr elasticsearch
   - ISearch：多隆（可以关注一下程序员中的明星）
     - 鸡汤：牛人有一段苦日子，坚持就终将牛

5. 商品热门的波段信息

   - 内存数据库
   - redis，tair，Memocache..

6. 商品交易，外部的支付接口
   - 三方应用



大型互联网应用问题：

- 数据类型多
- 数据源多，经常重构
- 数据要改造，大面积改造

解决问题：

![image-20211110164854772](redis.assets/image-20211110164854772.png)



缓存设计：

![image-20211110165139785](redis.assets/image-20211110165139785.png)



以上都是NoSQL入门概述，不仅能够提高大家的知识，还可以帮助大家了解大厂的工作内容!



### NoSQL的四大分类

#### KV键值对

- 新浪：redis
- 美团：Redis+tair
- 阿里，百度：redis+memocache

#### 文档型数据库

（bson格式，和json十分相似）

- MongoDB（一般必须要掌握）
  - 是一个基于分布式文件存储的数据库，c++编写，主要用来处理大量的文档
  - MongoDB是一个介于关系型数据库和非关系型数据库中间的产品，MongoDB是非关系型数据库中功能最丰富的，最像关系型数据库的。

- conthDB

#### 列存储数据库

- hbase
- 分布式文件系统

#### 图关系数据库

- 他不是存图形，**放的是关系**。比如朋友圈社交网络，广告推荐

​	![image-20211110170353256](redis.assets/image-20211110170353256.png)

- neo4j，infogrid



#### 四大分类对比

| 分类                | Examples举例                                       | 典型应用场景                                                 | 数据模型                                        | 优点                                                         | 缺点                                                         |
| ------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 键值对（key-value） | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。 | Key 指向 Value 的键值对，通常用hash table来实现 | 查找速度快                                                   | 数据无结构化，通常只被当作字符串或者二进制数据               |
| 列存储数据库        | Cassandra, HBase, Riak                             | 分布式的文件系统                                             | 以列簇式存储，将同一列数据存在一起              | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 |
| 文档型数据库        | CouchDB, MongoDb                                   | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| 图形(Graph)数据库   | Neo4J, InfoGrid, Infinite Graph                    | 社交网络，推荐系统等。专注于构建关系图谱                     | 图结构                                          | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |



鸡汤：敬畏之心可以使人进步，觉得自己垃圾了就能进步了。努力学习不要被社会抛弃。



## Redis入门

### 概述

#### Redis是什么？

Redis（**Re**mote **Di**ctionary **S**erver )，即远程字典服务。

是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供**多种语言**的API。

与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

免费和开源!是当下最热门的NoSQL技术之一!也被人们称之为结构化数据库!

#### Redis能干什么？

1. 内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
2. 高效率、用于高速缓冲
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计数器(eg：浏览量)
6. 。。。

#### 特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务
5. ...

#### 学习中用到的网站

- [redis官网](https://redis.io/)
- [redis中文网](http://redis.cn/),http连接不安全
- [下载地址](https://redis.io/download),通过官网下载即可
  - 但是windows系统在[github](https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100)上下载（停更很久了）
  - redis**推荐在linux**服务器上大家，接下来也基本都是基于linux学习



### Windows系统安装redis

1. 下载安装包

   访问[网址](https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100)

   ![image-20211111154005354](redis.assets/image-20211111154005354.png)

   

2. 解压，

   redis十分的小，只有5M																										

   ![image-20211111154145274](redis.assets/image-20211111154145274.png)![image-20211111154313531](redis.assets/image-20211111154313531.png)

3. 开启redis

   双击`redis-server.exe`,运行redisserver

   - 截图中看到redis默认端口号是6379

   ![image-20211111154650721](redis.assets/image-20211111154650721.png)

4. 使用redis客户端来连接redis

   保持redis-server.exe开着，双击`redis-cli.exe`

   ![image-20211111154915088](redis.assets/image-20211111154915088.png)

   使用键值对存储数据，并拿取数据

   ![image-20211111155514888](redis.assets/image-20211111155514888.png)

   记住一句话，Window下使用确实简单，但是Redis推荐我们使用Linux去开发使用!

### Linux安装redis（真企业级）

实战：

[来到官网](https://download.redis.io/releases/)，下载linux安装包，版本为课程中使用的redis版本

![image-20211111160822681](redis.assets/image-20211111160822681.png)

![image-20211111161101372](redis.assets/image-20211111161101372.png)

连接服务器

![image-20211111171722791](redis.assets/image-20211111171722791.png)

把redis包用XFTP放到home/zhangyun目录下，zhangyun目录使我自己建的，这个目录可以专门存一些上传上来的软件。

![image-20211111172704283](redis.assets/image-20211111172704283.png)

程序一般放到opt目录，移动过去

```
mv redis-5.0.8.tar.gz /opt
```

![image-20211111172939217](redis.assets/image-20211111172939217.png)

解压redis安装包

```
tar -zxvf redis-5.0.8.tar.gz
```

![image-20211111173111969](redis.assets/image-20211111173111969.png)

进入解压后的redis文件夹，可以看到redis的配置文件。

- 文件夹中没有redisserver和cli，因为文件夹解压后程序不在文件夹中；要自己安装一下

![image-20211111173320809](redis.assets/image-20211111173320809.png)

现在开始基本的环境安装

```
# 安装gcc环境，不过我之前安装docker已经装过，不过还是执行一下。
yum install gcc-c++
```

![image-20211111173709864](redis.assets/image-20211111173709864.png)

查看gcc的版本

```
gcc -v
```

![image-20211111173838019](redis.assets/image-20211111173838019.png)

执行make命令，把所有需要的文件配置上；即进行源代码编译，以及一些功能的提供

```
make
```

![image-20211111174140145](redis.assets/image-20211111174140145.png)

![image-20211111174152658](redis.assets/image-20211111174152658.png)

测试make语句是否执行成功

```
make test
```

![image-20211111174639787](redis.assets/image-20211111174639787.png)

执行make install，查看是不是所有东西安装好了

```
make install
```

![image-20211111175021082](redis.assets/image-20211111175021082.png)

redis的默认安装路径，`/usr/local/bin`下面

- 就想windows安装软件，默认路径在/usr/local/bin下面

```
cd /usr/local/bin
```

![image-20211111175825441](redis.assets/image-20211111175825441.png)

在bin目录下新建文件夹`zconfig`来存放配置文件

```
mkdir zconfig
```

![image-20211111180123467](redis.assets/image-20211111180123467.png)

把redis解压文件夹中的配置文件`redis.config`复制到`bin/zconfig`中

- 原生的配置文件就在解压目录下不会变，想恢复初始配置随时可以恢复；以后我们使用`zconfig`中的配置文件进行启动

```
cp /opt/redis-5.0.8/redis.conf zconfig
```

![image-20211111180433172](redis.assets/image-20211111180433172.png)

redis默认不是后台启动的，vim修改配置文件，使得redis后台启动

![image-20211111204018575](redis.assets/image-20211111204018575.png)

现在启动redis服务

```
# 来到bin目录
cd ../
```

![image-20211111205604725](redis.assets/image-20211111205604725.png)

```
# 用zconfig/redis.conf文件启动redis-server
redis-server zconfig/redis.conf
```

- 前面修改conf文件可能会造成文件损坏，当你启动不了redis删除配置文件重新操作一遍即可

![image-20211111205655153](redis.assets/image-20211111205655153.png)

现在使用redis客户端去连接redis服务端

```
# -p 6379表示指定客户端的端口号是6379
redis-cli -p 6379
```

现在就可以使用redis来存储数据啦

![image-20211111210442373](redis.assets/image-20211111210442373.png)

![image-20211111210518809](redis.assets/image-20211111210518809.png)

![image-20211111210612392](redis.assets/image-20211111210612392.png)

查看redis的进程是否开启

新开一个端口

![image-20211111211149816](redis.assets/image-20211111211149816.png)

```
# 查看系统中有关redis的进程
ps -ef|grep redis
```

看到redis-cli和redis-server的进程，说明redis服务和连接都开启了

![image-20211111211903459](redis.assets/image-20211111211903459.png)

现在关闭redis服务，回到redis功能页，依次执行下面命令

```
shutdown
```

```
exit
```

![image-20211111212320002](redis.assets/image-20211111212320002.png)

现在再来到新开的页面，重新查看有关redis的进程，已经没有redis的客户端和服务端的进程了。

![image-20211111212846090](redis.assets/image-20211111212846090.png)

后面我们会使用单机多redis启动集群测试



### 测试性能

redis-benchmark是一个压力测试工具

官方自带的性能测试工具



命令参数：

- 下图中-d 应该是3，图太老了。

![img](redis.assets/fea09ddf2bd6acecbdb7db2ddc420faa.png)



测试并发实战：

- 保证redis-server和redis-cli启动了

先用一个连接启动redis客户端和服务端

![image-20211112172125543](redis.assets/image-20211112172125543.png)

新开一个窗口，在redis软件所在的目录（bin）执行下列命令测试一下redis

```bash
# 来到目录
cd /usr/local/bin
# 使用端口号6379，100个并发链接，100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20211112172448803](redis.assets/image-20211112172448803.png)

现在来解读一下测试结果

![image-20211112173251978](redis.assets/image-20211112173251978.png)



### redis基本知识说明

redis默认有16个数据库

- zconfig/redis.conf文件中可以查看

![image-20211112173809918](redis.assets/image-20211112173809918.png)

默认使用的是第0个，可以使用select进行切换数据库!

```bash
# 切换数据库
select 数据库id

# 查看当前数据库大小
DBSIZE
```

![image-20211112174030455](redis.assets/image-20211112174030455.png)

一个数据库存值后，本数据库大小发生了变化；但是其他数据库的大小不变。

![image-20211112174305056](redis.assets/image-20211112174305056.png)

查看当前数据库中所有的key

```
keys *
```

![image-20211112174640999](redis.assets/image-20211112174640999.png)

清空redis数据库

```
# 清空当前数据库
flushdb
# 清空全部
FLUSHA LL
```



redis是单线程的：

明白Redis是很快的，官方表示，Redis是基于内存操作，**CPU不是Redis性能瓶颈，Redis的瓶颈是根据机器的内存和网络带宽**，既然可以使用单线程来实现，就使用单线程了!

Redis是C语言写的，官方提供的数据为100000+的QPS，完全不比同样是使用key-vale的Memecache差!

Redis为什么单线程还这么快?

- 误区1∶高性能的服务器一定是多线程的?
- 误区2∶多线程（CPU上下文会切换! )一定比单线程效率高!先去CPU>内存>硬盘的速度要有所了解!

**核心**: redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程(CPU上下文会切换︰耗时的操作!! ! )反而更耗时，对于**内存系统来说，如果没有上下文切换效率就是最高的**!多次读写都是在一个CPU上的，在内存情况下，这个就是最佳的方案!



## 五大数据类型

官网文档

![image-20211113145944923](redis.assets/image-20211113145944923.png)

全段翻译︰
Redis是一个开源(BSD许可)的，内存中的数据结构存储系统，它可以用作**数据库**、**缓存**和**消息中间件MQ**。它支持多种类型的数据结构，如字符串( strings )，散列( hashes )，列表(lists )，集合( sets )，有序集合( sorted sets）与范围查询，bitmaps,hyperloglogs和地理空间( geospatial ）索引半径查询。Redis 内置了复制( replication )，LUA脚本( Lua scripting )，LRU驱动事件(LRU eviction )，事务( transactions）和不同级别的磁盘持久化( persistence )，并通过Redis哨兵( Sentinel )和自动分区( Cluster )提供高可用性( high availability )。

### Redis-Key相关简单操作

- 20230218我：今天解开了一个误区。之前以为redis的String类型才是kv键值对，其他数据类型都不是kv键值对存储的。今天知道了，redis就是kv型的数据库，String类型是value为String；与之类似的，Hash类型就是value是一对对hash映射。
  - 参考文章[对比redis的String和hash类型](http://t.csdn.cn/1wdWx)有句话很到位：redis自身是一个Map，其中所有的数据都是采用**key:value**的形式存储，数据类型指的是存储的value的类型，key部分永远都是字符串
  - 坛友回复String和hash对比：hash本身就是多个kv形成的哈希表，假设需要存储对象且频繁修改，用hash比较合适，如果只是单纯的存一个json，直接将对象toJsonString用string存起来就可以

查看所有的key

```
keys *
```

![image-20211113152855396](redis.assets/image-20211113152855396.png)

设置键值对

```
set k v
```

![image-20211113153012484](redis.assets/image-20211113153012484.png)

获得指定k的v

```
get k
```

![image-20211113154322963](redis.assets/image-20211113154322963.png)

判断一个key是否有存在

```
# 返回1则存在，返回0则不存在
EXISTS k
```



![image-20211113153127900](redis.assets/image-20211113153127900.png)

把当前库的一个kv对移动到另一个库中

```
move k baseName
```

![image-20211113153712253](redis.assets/image-20211113153712253.png)

设置键值对的过期时间，单位是秒

```
EXPIRE k timeInSecond
```

- 网友：expire是对于已经存在的键值

![image-20211113154034044](redis.assets/image-20211113154034044.png)

查看键值对剩余生命，单位是秒

```
ttl k
```

查看指定k对应v的类型

- 一共有String,List,Set,Hash,Zset五种类型

```
type k
```

![image-20211113154647437](redis.assets/image-20211113154647437.png)



后面如果碰到不会的命令，可以在官网查看

![image-20211113154915498](redis.assets/image-20211113154915498.png)

### String

*很多java程序员用redis只会String类型*

下面演示redis中，string类型的v的一些基本操作

```bash
# 设置值
127.0.0.1:6379> set k1 v1
OK
# 获得值
127.0.0.1:6379> get k1
"v1"
# 为指定键的string类型v的结尾粘结字符串。如果键不存在相当于新建string型kv键值对。
127.0.0.1:6379> append k1 zhangyun
(integer) 10
127.0.0.1:6379> get k1
"v1zhangyun"
# 查看指定键对应的string类型v的长度
127.0.0.1:6379> strlen k1
(integer) 10
# 查看指定键对应的键值对是否存在
127.0.0.1:6379> exists k1
(integer) 1
127.0.0.1:6379> 
```

String中有关增减的一组命令：

```bash
127.0.0.1:6379> set num 1
OK
# 以步长为1增加指定的k对应的v
127.0.0.1:6379> incr num
(integer) 2
127.0.0.1:6379> get num
"2"
# 以步长为1减小指定的k对应的v
127.0.0.1:6379> decr num
(integer) 1
127.0.0.1:6379> get num
"1"
# 以指定步长，增加指定的k对应的v
127.0.0.1:6379> incrby num 10
(integer) 11
127.0.0.1:6379> get num
"11"
# 以指定步长，减小指定的k对应的v
127.0.0.1:6379> decrby num 10
(integer) 1
127.0.0.1:6379> get num
"1"
127.0.0.1:6379> 

```

**redis中tab键不仅可以补足文件和目录，还能补足redis命令**

字符串截取的一些操作

```bash
127.0.0.1:6379> get k1
"v1zhangyun"
# 取指定k对应v的，从0号位置到3号位置之间的内容
127.0.0.1:6379> GETRANGE k1 0 3
"v1zh"
# 取指定k对应v的全部内容，-1表示取全部
127.0.0.1:6379> GETRANGE k1 0 -1
"v1zhangyun"
127.0.0.1:6379> 
```

字符串内容替换的一些操作

```bash
127.0.0.1:6379> set k2 abcdefg
OK
127.0.0.1:6379> get k2
"abcdefg"
# 从v的1号位置开始，替换为zzz，未被zzz覆盖的字符串位置保持原状
127.0.0.1:6379> SETRANGE k2 1 zzz
(integer) 7
127.0.0.1:6379> get k2
"azzzefg"
127.0.0.1:6379> 
```

设置键值对的同时设置过期时间。

- 和expire区别：Setex是一个原子操作，设置值+设置过期时间两个动作，会在同一时间完成；在Redis缓存中，非常实用。expire是单纯的设置死亡时间。

```
# 设置键k3，值v3，生命20s的键值对。set with expire
setex k3 20 v3
```

![image-20211113191434683](redis.assets/image-20211113191434683.png)

不存在时的设置(set if not exist)，指定key对应的键值对不存在的时候才能设置成功。

- 在分布式锁中常常使用，因为保证值总是存在。

```
setnx k4 redis
```

![image-20211113192358003](redis.assets/image-20211113192358003.png)

批量设置键值对

```bash
mset k1 v1 k2 v2 k3 v3

# 批量设置键值对，当所有键值对不存在时才生效，原子性！
msetnx k1 v1 k4 v4
```

![image-20211113192817565](redis.assets/image-20211113192817565.png)

![image-20211113194057284](redis.assets/image-20211113194057284.png)

批量获取键对应的值

```bash
mget k1 k2 k3
```

![image-20211113193011565](redis.assets/image-20211113193011565.png)

使用json方式新建对象

```
set user:2 {name:zhangyun,age:3}
```

- 我：json方式新建对象时的value就是{name:zhangyun,age:3}，不能在redis层面通过get user:2:name获得value内部的值，只能先把整体value拿到，再到业务层去解析json成一个对象，然后读取对象中的字段的值。

使用mset的方式新建对象，或修改对象的值

- 这里的key是一个巧妙的设计：user:{id}:{field}，这种设计在redis中是完全ok的
  - 我：这是redis的一个约定，存入的key就是用冒号连接的，没有别的作用；我猜是增加key的可读性。


```
mset user:1:name zhangyun user:1:age 2
```

使用mget的方式获取对象的部分或全部信息

```
mget user:1:name user:1:age
```

![image-20211113200747145](redis.assets/image-20211113200747145.png)

先get再set

- 如果不存在值则返回null，并设置值；如果存在值则返回值，并设置新值
- getset可以用来做一些更新的操作
- 类似的操作还有CAS(compare and swap)，同时做两个操作。

```
getset db redis
```

![image-20211113201221941](redis.assets/image-20211113201221941.png)



String类似的使用场景:value除了是我们的字符串还可以是我们的数字!

- 计数器，incrby实现
- 统计多单位的数量书
- 粉丝数
- 对象缓存存储

### List

在redis里面，我们可以把list玩成，栈、队列、阻塞队列!

所有list大部分命令都是用l开头的，lpush rpush lpop rpop组合是一个例外。

redis命令不区分大小写。

redis中list不是列表，是双向链表。



LPUSH将一个值或者多个值插入列表的头部:

```
LPUSH listName v
```

LRANGE获取list中一定范围内的值：


```
LRANGE listName l r
```

![image-20211113203121627](redis.assets/image-20211113203121627.png)

RPUSH把一个值或多个值插入到列表的尾部：

```
RPUSH listName v
```

![image-20211113203744471](redis.assets/image-20211113203744471.png)

从列表头部（左边）移除一个值:

```
Lpop listname
```

从列表尾部（右边）移除一个值:

```
Rpop listname
```

![image-20211113205628884](redis.assets/image-20211113205628884.png)

获取列表的指定下标的值

```
Lindex listname index
```

![image-20211113210350181](redis.assets/image-20211113210350181.png)

查看列表的长度

![image-20211113210553333](redis.assets/image-20211113210553333.png)

移除指定的值（精确匹配）：

- 类似“取消关注”的功能，需要删除指定的值uid

```bash
# 从list列表，移除两个，值为“three”的元素
lrem list 2 three
```

![image-20211113211539127](redis.assets/image-20211113211539127.png)

截断列表保留下指定的值（们）

```
ltrim listname l r
```

![image-20211113212506958](redis.assets/image-20211113212506958.png)

rpoplpush,从原list右边(尾)pop一个元素，给新list左边（头）push进去

- **组合命令是原子操作**，在处理并发问题的时候，占有优势

```
rpoplpush src des
```

![image-20211114135459228](redis.assets/image-20211114135459228.png)

lset 将list（已存在）中指定下标（已存在）的值替换为另一个值

- 如果list或者下标不存在，lset会报错

```
lset listname index value
```

![image-20211114140649646](redis.assets/image-20211114140649646.png)

在list的指定位置插入元素：

```
linsert listname before/after pivotWord insertedWord
```

![image-20211114141754705](redis.assets/image-20211114141754705.png)



小结：

- list实际上是一个双向链表：before Node after ；left 和right都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增内容
- 如果移除了所有node，空链表，也代表不存在!
- 在两边插入或者改动值，效率最高!中间元素，相对来说效率会低一点~
- 实际上这些命令，用的时候看redis官方api即可。

队列（消息排队）：lpush+rpop；栈：lpush+lpop。



### Set

存值，取值，查值

```bash
# set集合中添加元素
sadd setname v
# 查看指定set集合的所有值
SMEMBERS setname
# 判断某个值是不是在set集合中
SISMEMBER setname v
```

![image-20211114155103001](redis.assets/image-20211114155103001.png)

获取set中元素的个数

```
scard setname 
```

![image-20211114160007163](redis.assets/image-20211114160007163.png)

srem从set中移除指定元素

```
srem setname v 
```

![image-20211114160222327](redis.assets/image-20211114160222327.png)

随机删除一些元素

```
spop setname
```

![image-20211114162336001](redis.assets/image-20211114162336001.png)

SRANDMEMBER从set中随机抽选一个元素

```
SRANDMEMBER setname num
```

![image-20211114160819128](redis.assets/image-20211114160819128.png)

smove把一个元素从一个set移动到另一个set

```
smove setnameSrc setnameDes v
```

![image-20211114162951891](redis.assets/image-20211114162951891.png)

sdiff求两个集合的差集

```
sdiff set1 set2
```

![image-20211114163637025](redis.assets/image-20211114163637025.png)

求两个集合的交集，比如微博中好友的共同关注

```
SINTER set1 set2
```

![image-20211114163802610](redis.assets/image-20211114163802610.png)

求两个集合的并集

```
sunion set1 set2
```

![image-20211114170134829](redis.assets/image-20211114170134829.png)

### Hash

redis中hashmap，本质和string没有什么太大区别，本质还是一个简单地k-v。



**hset**存hashmap，**hget和hmget和hgetall**取hashmap：

- 根据Redis 4.0.0，HMSET被视为已弃用。请在新代码中使用HSET。

```bash
# (批量)k-v存值
hset hashmapName k1 v1 k2 v2 k3 v3...
# 取值
hget hashmapName k
# 批量取v
hmget hashmapName k1 k2...
# 取hashmap所有的k-v
hgetall hashmapName
```

![image-20211114195850633](redis.assets/image-20211114195850633.png)

![image-20211114200058596](redis.assets/image-20211114200058596.png)

![image-20211114200702855](redis.assets/image-20211114200702855.png)

hdel删除hashmap中某一键值对

```
hdel hashmapName k
```

![image-20211114200926957](redis.assets/image-20211114200926957.png)

hlen查看hashmap中键值对的数目

```
hlen hashmapName
```

![image-20211114201753573](redis.assets/image-20211114201753573.png)

hexists判断某个键（值对）是否存在

```
HEXISTS hashmapName k
```

![image-20211114202256450](redis.assets/image-20211114202256450.png)

**hkeys**和**hvals**查看hashmap的所有键或值

```bash
# 查看hashmap所有的key
hkeys hashmapName
# 查看hashmap所有的value
hvals hashmapName
```

![image-20211114202618807](redis.assets/image-20211114202618807.png)

hincrby增加或减少hashmap中的v（应为数字类型）的值

```
HINCRBY hashmapName k step
```

![image-20211114203503206](redis.assets/image-20211114203503206.png)

hsetnx当键k不存在时添加键值对

```
hsetnx hashmapName k v
```

![image-20211114204221467](redis.assets/image-20211114204221467.png)



hashmap可以存储变更的数据，尤其是用户信息之类的，经常变动的信息。hash更适合对象的存储 ，string 更适合字符串的存储。

### Zset（有序集合）

set的基础上为每一个元素增加一个序号。

与set相比不同的是每个元素都会关联一个double类型的分数（score）。redis正是通过分数来为集合中的成员进行从小到大的排序。

- score相同：按字典顺序排序

- 有序集合的成员是唯一的,但分数(score)却可以重复。



#### zadd

往有序集合中添加元素

```
zadd zsetname index1 v1 index2 v2...
```

![image-20211114210449568](redis.assets/image-20211114210449568.png)

查看有序集合的一段元素

```
# 1为0，r为-1时，为查看所有元素
zrange zsetName l r
```



#### zrange+ZRANGEBYSCORE+ZREVRANGEBYSCORE

**ZRANGEBYSCORE**为有序集合按照编号升序排序：

- 降序要使用ZREVRANGEBYSCORE 命令
- ZRANGEBYSCORE还能添加一些其他参数，比如 `withscore`，表示排序同时把编号也打印出来。

```
# low为-inf表示编号从负无穷开始计入排序，up为+inf表示编号截止到正无穷计入排序
ZRANGEBYSCORE zsetname low up
```

![image-20211114211041978](redis.assets/image-20211114211041978.png)



#### zrem

移除zset(有序集合)中的指定元素value

```
zrem zsetname value
```

![image-20220102123532407](redis.assets/image-20220102123532407.png)



#### zcard

查看zset中元素的个数

```
zcard zsetname
```

![image-20220102123728704](redis.assets/image-20220102123728704.png)



#### zcount

获取zset指定区间的成员个数，因为有些序号可能没有对应的value。

```
zcount zsetname leftindex rightindex
```

![image-20220102124337426](redis.assets/image-20220102124337426.png)



#### zset应用案例

- set排序 存储班级成绩表 工资表排序！
- 普通消息为1。重要消息为2。带权重进行判断
- 排行榜应用实现，取Top N测试



### 其他

其余的一些API，如果工作中有需要，这个时候可以去查查看[官方文档命令](https://redis.io/commands)



后续可以使用java-api操控redis

```bash
# redis存string类型
jedis.set(key,value)
# redis存hash类型
jedis.set(key,Map)
```



## 三种特殊的数据类型

### Geospatial(地理位置)

#### 引入

使用场景

- 朋友的定位，附近的人，打车距离计算?



Redis的Geo在Redis3.2版本就推出了!这个功能可以推算地理位置的信息，两地之间的距离，方圆几里的人!



查询城市经纬度的[在线工具](http://www.jsons.cn/lngcode/)



相关命令

![image-20220102131134169](redis.assets/image-20220102131134169.png)





#### geoadd

[官网介绍](https://redis.io/commands/geoadd)。将一个或多个具体经纬度的坐标存入一个有序集合。



命令：

```
geoadd key longitud(经度) latitude(纬度) member(城市) [..]
```

- 地球两极：南极北极是无法直接添加的。

- 我们一般会下载城市数据，直接通过java程序一次性导入。

- 有效的经度从-180度到180度。有效的纬度从-85.05112878度到85.05112878度。当坐标位置超出上述指定范围时,该命令将会返回一个错误。

  ```
  #127.0.0.1:6379> geoadd china:city 39.90 116.40 beijing
  (error) ERR invalid longitude,latitude pair 39.900000,116.400000
  ```



实例：

```bash
# 清空redis
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> list
(error) ERR unknown command `list`, with args beginning with: 
127.0.0.1:6379> keys *
(empty list or set)
# 往redis中写入中国地理坐标
127.0.0.1:6379> geoadd china:city 116 39 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121 31 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106 29 chongqing
(integer) 1
127.0.0.1:6379> geoadd china:city 114 22 shenzhen 120.15 30.28 hangzhou
(integer) 2
127.0.0.1:6379> geoadd china:city 108.96 34.26 xian
(integer) 1

```



#### geopos

拿到某(几)个城市的坐标。



命令：

```
geopos key member
```



实战：

```bash
127.0.0.1:6379> geopos china:city beijing
1) 1) "116.00000113248825073"
   2) "38.99999918434559731"
# 获取上海和重庆的坐标
127.0.0.1:6379> geopos china:city shanghai chongqing
1) 1) "120.99999815225601196"
   2) "31.00000097648057817"
2) 1) "106.00000172853469849"
   2) "28.99999952347345555"
```



#### geodist

返回两个位置之间的距离，如果有位置不存在则返回空值。

单位:

- m表示单位为米。
- km表示单位为千米。
- mi表示单位为英里。
- ft表示单位为英尺。



命令：

```
geodist key member1 member2 [-para]
```



实战：

```bash
# 上海到北京的直线距离，单位米
127.0.0.1:6379> geodist china:city beijing shanghai
"999207.7044"
# 上海到北京的直线距离，单位千米
127.0.0.1:6379> geodist china:city beijing shanghai km
"999.2077"
```



#### georadius

以给定的经纬度为中心，找出某一半径内的元素。比如“附近的人”功能，通过半径来查询。



命令：

```
GEORADIUS key longtitude经度 latitude纬度 半径 半径单位 参数(们)
```



实战：

```bash
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
# 经纬度（110 30）的半径500km以内的城市
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km
1) "chongqing"
2) "xian"
# 查询城市的同时打印城市坐标
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withcoord
1) 1) "chongqing"
   2) 1) "106.00000172853469849"
      2) "28.99999952347345555"
2) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
# 查询城市的同时打印距离
127.0.0.1:6379> GEORADIUS china:city 110 30 500 km withdist
1) 1) "chongqing"
   2) "402.8557"
2) 1) "xian"
   2) "483.8340"
# 限制查询结果的个数，类似mysql的limit
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km count 3
1) "chongqing"
2) "xian"
3) "shenzhen"
127.0.0.1:6379> GEORADIUS china:city 110 30 1000 km count 2
1) "chongqing"
2) "xian"


```



#### georadiusbymember

类似georadius，不过不是给定坐标，而是给定城市名。换言之就是找出指定坐标元素周围的其他坐标元素。



命令：

```
GEORADIUSbymember key member 半径 半径单位 参数(们)
```



实战：

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 1000 km
1) "beijing"
2) "shanghai"
3) "xian"
127.0.0.1:6379> GEORADIUSBYMEMBER china:city hangzhou 400 km
1) "hangzhou"
2) "shanghai"
```



#### geohash

该命令将返回坐标对应的11个字符的Geohash字符串!将维的经纬度转换为一维的字符串。如果两个字符串越接近，说明相互距离越小。

一般用不上。



#### geoXXX底层实现原理

geoXXX底层的实现原理就是zset。我们可以使用zset命令来操作geo。

比如geoXXX中没有“移除坐标”的方法，可以使用zset的方法zrem来移除

```bash
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
# 移除杭州的坐标
127.0.0.1:6379> ZREM china:city hangzhou
(integer) 1
127.0.0.1:6379> ZRANGE china:city 0 -1
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "shanghai"
5) "beijing"
```



### HyperLogLog

#### 基数定义

A{1,3,5,7,8,7}，B{1,3,5,7,8}。A和B的基数（不重复的元素的个数）都为5，可以接受误差!

#### 简介

Redis 2.8.9版本就更新了Hyperloglog数据结构。

Redis Hyperloglog基数统计的算法：

- 布隆过滤器

优点∶

- 占用的内存是固定，2*64不同的元素的技术，只需要废12KB内存!如果要从内存角度来比较的话Hyperloglog首选!

适用场景: 

- 网站UV（unique visitor）量。传统用set存userid统计，但若存在大量用户id，则太消耗内容且麻烦，若只为计数且**允许有错误率**（0.81%），则可行，否则还是用set统计
- 一个人访问一个网站多次，还是算作一个人。



#### 实战

```bash
# Redis Pfadd 命令将所有元素参数添加到 HyperLogLog 数据结构中,key为ch1，a b c d e f g h i为元素参数
127.0.0.1:6379> PFADD ch1 a b c d e f g h i
(integer) 1
# 查询key为ch1的HyperLogLog数据结构中元素个数
127.0.0.1:6379> PFCOUNT ch1
(integer) 9
# 创建key为ch2的HyperLogLog数据结构
127.0.0.1:6379> PFADD ch2 g h i j k l m 
(integer) 1
# 查询key为ch2的HyperLogLog数据结构中元素个数
127.0.0.1:6379> PFCOUNT ch2
(integer) 7
# key为ch1的HyperLogLog数据结构 合并 key为ch2的HyperLogLog数据结构
127.0.0.1:6379> PFMERGE ch3 ch1 ch2
OK
# 查询合并后的HyperLogLog数据结构中元素个数
127.0.0.1:6379> PFCOUNT ch3
(integer) 13
```



### Bitmaps

#### 位存储

核心思想是“位存储”。

“位存储”思想类似“boolean”，比如统计登录or未登录，打卡or未打卡；又比如疫情期间中国14亿人，可以有14亿位数字，每个人用一位表示，如果感染了用1表示，没感染用0表示，如00010001000....。14亿位的数字其实很小，用一台计算机就能处理，这就是“位存储”的好处。

Bitmaps位图是一种数据结构，是操作二进制位来进行记录，就只有0和1两个状态。比如做365天的打卡与否的记录的话，只需要365 bit，约46个字节(1字节=8bit )左右!

只要是两个状态的，都可以使用Bitmaps。



#### 实战

使用bitmap来记录周一到周日的打卡。

- 周一：1（打卡了）；周二：0（没打卡），周三：0（没打卡），周四：1（打卡了），周五：1（打卡了）；周六：0（没打卡），周日：0（没打卡）

```bash
# 清空redis表，方便查看表
127.0.0.1:6379> keys *
1) "ch3"
2) "ch1"
3) "ch2"
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty list or set)
# 使用bitmap记录一周的打卡
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
# 查看周四打卡情况：得到结果为打卡了
127.0.0.1:6379> getbit sign 3
(integer) 1
# 查看周日打卡情况：得到结果为未打卡
127.0.0.1:6379> getbit sign 6
(integer) 0
# 统计操作：统计本周打卡天数，得到结果为3天
127.0.0.1:6379> bitcount sign
(integer) 3
```



有些教程都不喜欢讲这些，因为老师本身可能就是万年kv，没有尝试别的数据结构，这样代码的效率很低。在生活中或者开发中，不同工具都有十分多的应用场景，学习了，就是就是多一个思路。技多不压身!



## 事务

redis事务的本质：

- 一组命令的集合；一个事务中的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行。



redis事务的性质：

- redis单条命令具有原子性，但是**redis事务不保证原子性**。

  - 想实现redis多条语句的原子性，要使用lua脚本

- 一次性，顺序性，排他性

- **redis事务没有隔离级别的概念**。

  - 所有的命令在事务中并没有被直接执行，只有发起执行命令(Exec)的时候才会执行。
  - 不会有脏读，幻读之类的概念

- 事务步骤

  1. 开启事务（`multi`）。
  2. 命令入队（...）。这个没什么好讲的，就是一堆redis命令。
  3. 执行事务（`exec`）。

  

### 正常执行事务

实战：

```bash
# 清空redis数据库
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty list or set)
# 开启事务
127.0.0.1:6379> multi
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
# 执行事务，得到执行事务的结果
127.0.0.1:6379> exec
1) OK
2) OK
3) "v2"
4) OK

```



### 放弃事务

实战：

```bash
# 开启事务
127.0.0.1:6379> multi
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k4 v4
QUEUED
# 放弃事务，事务队列中的命令都不会被执行。
127.0.0.1:6379> DISCARD
OK
# 读不到k4的值，说明事务中对k4的设置没有执行，体现了放弃事务。
127.0.0.1:6379> get k4
(nil)
```



### 事务中的异常

#### 编译型异常

代码有错误即**命令有错误。事务中所有的命令都不会被执行**



实战：

```bash
# 清空redis数据库
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty list or set)
# 开启事务
127.0.0.1:6379>  multi
OK
# 命令入队
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
# 把会导致编译型异常的命令入队，直接就自动反馈了error。但是队列还在。
127.0.0.1:6379> getset k2
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set k3 v3
QUEUED
# 执行事务。因为命令中有编译型异常的命令，所有命令都执行不成功。
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
# 通过k3取值取不到也可以看出来事务中所有命令都没被执行。
127.0.0.1:6379> get k3
(nil)

```





#### 运行时异常

比如1除以0。如果事务队列中存在这种异常，那么执行命令的时候，**除开异常命令以外的其他命令都是可以执行的**，错误命令抛出异常。这也**体现了redis的事务不保证原子性**。



实战：

```bash
# 清空数据库
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty list or set)
# 开启事务
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 "v1"
QUEUED
# 把会导致运行时异常的命令入队，执行事务的时候本条命令会失败。
127.0.0.1:6379> incr k1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
# 执行事务。除了运行时异常的命令抛出异常，其他命令正确执行。
127.0.0.1:6379> exec
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
4) "v2"
```



### 监控

#### 悲观锁

这里不讲

#### 乐观锁

乐观锁定义：

- 很乐观，认为什么时候都不会出问题，所以不会上锁!更新数据的时候去判断一下，在此期间是否有人修改过这个数据。
- 获取version。**这里的version不是对值比较**，而是类似version戳的东西。
- 更新的时候比较version。



---



正常执行成功的实战：

```bash
# 清空redis数据库
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> keys *
(empty list or set)
# 设置值
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
# 设置乐观锁，监视money对象。
127.0.0.1:6379> watch money
OK
# 开启事务
127.0.0.1:6379> multi
OK
# 命令入队
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY out 20
QUEUED
# 执行事务。事务存在期间没有发生改动，这个时候事务就正常结束。
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
```



---



执行失败的实战：

先新开一个页面，连接redisserver，此页面便是第二个redisclient（客户端2）

![image-20220102205254782](redis.assets/image-20220102205254782.png)

在客户端1执行如下语句。注意事务没有执行！！！

```bash
# redis客户端1语句
# 监视money
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 10
QUEUED
127.0.0.1:6379> INCRBY money 10
QUEUED
```

客户端1的事务执行前，客户端2（另一条线程）插进来一脚，执行如下语句

```bash
# redis客户端2语句
127.0.0.1:6379> get money
"80"
# 一个线程修改了值
127.0.0.1:6379> set money 1000
OK
```

这是回到客户端1的页面，输入“执行事务”的命令

- 注意：**无论事务是否执行成功，Redis的exec和discard命令都会取消对所有键的Watch监控**

```bash
# redis客户端1语句
# 因为客户端1使用watch money监测money，并且客户端2在客户端1事务的生命中间修改了money，所以客户端1的事务执行失败。
127.0.0.1:6379> exec
(nil)
```

所以redis**多线程修改值**的时候，使用**watch**可以当做**redis的乐观锁**操作（**面试常问**）。

- 20230220我指出错误：redis是单线程操作数据的，同时处理多个客户端的请求是通过io多路复用，不会有线程问题。这里用watch实现乐观锁，避免事务执行期间有别的客户端连接修改关注的值，[参考](https://www.jianshu.com/p/935e3a06d0bb?ivk_sa=1024320u)

## 1111111Jedis

jedis是使用Java来操作Redis的中间件，Jedis是Redis官方推荐使用的Java连接redis的客户端。

后面springboot集成redis是顶层，这里讲解的jedis是底层，懂底层才能各种debug。



### jedis实战测试

#### 创建项目

创建空项目

![image-20220103095559078](redis.assets/image-20220103095559078.png)

![image-20220103095628506](redis.assets/image-20220103095628506.png)

![image-20220103095857114](redis.assets/image-20220103095857114.png)

创建空项目完毕后，idea自动跳出module创建要求，创建一下module

![image-20220103100102027](redis.assets/image-20220103100102027.png)

![image-20220103100206395](redis.assets/image-20220103100206395.png)

![image-20220103100301827](redis.assets/image-20220103100301827.png)

![image-20220103100354290](redis.assets/image-20220103100354290.png)

创建module完毕后，项目中可以看到module

![image-20220103100513396](redis.assets/image-20220103100513396.png)

空项目注意一个点：在project structure中把jdk配上

![image-20220103100643818](redis.assets/image-20220103100643818.png)

![image-20220103100758073](redis.assets/image-20220103100758073.png)

![image-20220103100835010](redis.assets/image-20220103100835010.png)

空项目还要在setting中把java版本改一下

![image-20220103101006993](redis.assets/image-20220103101006993.png)

![image-20220103101131499](redis.assets/image-20220103101131499.png)

#### 引入依赖

访问[mvnrepository](https://mvnrepository.com/)

![image-20220103102609505](redis.assets/image-20220103102609505.png)

使用3.2.0版本。碰到一些maven错误，参考[文章](https://blog.csdn.net/weixin_44005516/article/details/108293228)解决。

- 后面发现阿里云镜像就是用不了，直接删掉settings.xml中所有的镜像，用默认的maven中央仓库把jar包下载了。

![image-20220103102656085](redis.assets/image-20220103102656085.png)

```xml
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```

#### 编码测试

步骤概述：

- 连接数据库

- 操作命令

- 断开连接



具体步骤如下：

修改远程linux服务器上的redis配置文件（redis.conf），让redis监听所有ip的6379端口。这样才能远程连接。

![image-20220103152149818](redis.assets/image-20220103152149818.png)

腾讯云防火墙放开6379端口

![image-20220103151900947](redis.assets/image-20220103151900947.png)

远程云主机开启redis服务端

![image-20220103145746070](redis.assets/image-20220103145746070.png)

本地idea编写如下的java测试文件

```java
import redis.clients.jedis.Jedis;

public class TestPing {
    public static void main(String[] args) {
        //1. new Jedis对象
        Jedis jedis =new Jedis("120.53.244.17",6379);
        //2. jedis所有的命令就是我们之前学的所有redis指令,所以redis指令学习很重要
        System.out.println(jedis.ping());
        //3. 关闭连接。养成开启连接用完后随手关闭的习惯。
        jedis.close();
        
    }

}
```

运行测试文件，控制台成功输出“pong”，说明本地连接远程redis服务端成功！

![image-20220103151456568](redis.assets/image-20220103151456568.png)

**如果连接不上**，参考如下思路：远程链接不上，可能有几个原因：1.redis配置文件的ip限制注释掉，2.远程访问保护关掉，设置为no，3.如果是阿里ECS，需要开放6379端口，4。检查下是不是防火墙被拦了，5.远程redis启动了没有端口

### jedis常用API

string，List，set，Hash，zset，geospatial，hyperloglog，bitmaps。

所有的api命令，就是我们对应的上面学习的redis指令，一个都没有变化。



### jedis事务

实例+注释如下：

```java
package com.zhang;

import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TestTX {
    public static void main(String[] args) {
        //初始化
        Jedis jedis =new Jedis("120.53.244.17",6379);
        jedis.flushDB();

        //把得到json格式的字符串，作为kv的v
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello","world");
        jsonObject.put("name","zhangyun");
        String result = jsonObject.toJSONString();
        System.out.println("result -> " + result);

        //开启事务
        Transaction transaction = jedis.multi();

        //！！！可能执行失败并且要回滚的代码块，使用trycatch包裹起来！
        try {
            //命令入队
            transaction.set("user1",result);
            /*
            * 和redis环境不同，这里是javatrycatch环境，虽然1/0是运行时异常，但出现异常会走catch中的取消事务，exec没执行就抛错了，所以try中的set都不会执行
            *
            * exec都没执行，跟redis的事务执行异常根本没关系
            * */
            int i=1/0;//
            transaction.set("user2",result);

            //事务执行
            transaction.exec();
        } catch (Exception e) {
            //发生异常时放弃事务（事务回滚）
            transaction.discard();
            e.printStackTrace();
        } finally {
            //无论事务成功与否都会来到finally代码块。如果能看到user1和user2的值，则说明事务执行成功；如果值为空则事务失败。
            System.out.println("user1 -> " +jedis.get("user1"));
            System.out.println("user2 -> " +jedis.get("user2"));
            //关闭连接
            jedis.close();
        }
    }
}
```

运行代码的结果如下：

- 这个例子其实不好，因为异常都没被redis处理。本例子主要展示的其实是javatrycatch的功能：碰见报错的地方跳往catch。

![image-20220103155802073](redis.assets/image-20220103155802073.png)



## SpringBoot整合

### 概念说明

SpringBoot操作数据使用:spring-data，jpa，jdbc ，mongodb，redis !

SpringData也是和SpringBoot齐名的项目!

说明∶在SpringBoot2.x之后，原来`<artifactId>spring-boot-starter-data-redis</artifactId>`底层使用的jedis被替换为了lettuce?

- jedis :采用的直连，多个线程操作的话，是不安全的，如果想要避免不安全的，使用jedis pool连接池。更像BIO模式。
- lettuce:采用letty，实例可以再多个线程中进行共享，不存在线程不安全的情况!可以减少线程数量，更像NIO模式。



### 项目实战

#### 新建子module

新建springboot子module

![image-20220103171007969](redis.assets/image-20220103171007969.png)

![image-20220103171136057](redis.assets/image-20220103171136057.png)

![image-20220103171426588](redis.assets/image-20220103171426588.png)

springboot子module构建完成后可以在项目中看到

![image-20220103171521577](redis.assets/image-20220103171521577.png)

删掉没用的文件（夹），省的看的不舒服

![image-20220103171720153](redis.assets/image-20220103171720153.png)

![image-20220103171742801](redis.assets/image-20220103171742801.png)

#### 导入依赖

在创建项目时，勾选的模块已经自动体现在pom文件中的依赖里。

![image-20220103175739851](redis.assets/image-20220103175739851.png)

有一些构建的依赖和配置（如爆红的）要删掉，否则无法运行。删掉无关内容后，完整的pom内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.zhangyun</groupId>
    <artifactId>redis-02-springboot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>redis-02-springboot</name>
    <description>redis-02-springboot</description>
    <properties>
        <java.version>8</java.version>
        <repackage.classifier/>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```



#### 配置连接

在resource目录下的application.properties文件中进行springboot的配置。其中就可以配置redis的连接。

文件内容如下：

```properties
#SpringBoot所有的配置类，都有一个自动配置类:RedisAutoConfiguration
# 自动配置类都会绑定一个properties:RedisProperties

# 配置redis
spring.redis.host=120.53.244.17
spring.redis.port=6379
# 如果要使用连接池的话，要使用lettuce的连接池。因为jedis的连接池很多类都不存在也不生效。
```

#### 测试

打开springboot的测试类，编写内容+注释如下：

```java
package com.zhangyun.redis02springboot;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class Redis02SpringbootApplicationTests {

    //注入redisTemplate
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {

        /*redisTemplate操作不同的数据类型，api和redis指令是一样的：
        * opsForValue 操作字符串 类似String
        * opsForList 操作list 类似list
        * opsForSet
        * opsForHash
        * opsForZSet
        * opsForGeo
        * opsForHyperLogLog
        * */

        /*除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务，和基本的CRUD
        * */

        //获取redis的连接对象
        //RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
        //connection.flushDb();

        /*redis数据库中键值对有乱码，是因为java自带序列化的原因，参考文章：https://blog.csdn.net/qq_16159433/article/details/121491555*/
        redisTemplate.opsForValue().set("key1","zhangyun1");
        System.out.println(redisTemplate.opsForValue().get("key1"));
    }

}
```

运行测试方法后得到结果：

![image-20220103210556221](redis.assets/image-20220103210556221.png)

使用xshell查看远程redis库的内容，可以看到存的key1，但是因为序列化的问题，键和值的前面都有一大串不可读的字符：

![image-20220103210719758](redis.assets/image-20220103210719758.png)

为了解决这个问题，参见本章的“序列化相关”。



### 源码分析

#### RedisAutoConfiguration

分析RedisAutoConfiguration.java中的源码：

```java
@Bean
//我们可以自己定义一个redisTemplate来替换默认的
@ConditionalOnMissingBean(
    name = {"redisTemplate"}
)
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    //默认的RedisTemplate没有过多的设置，redis对象都是需要序列化的。
    //两个泛型都是Object, Object类型，我们经常使用的是String类型，所以后续需要强制转换为<String, Object>。
    RedisTemplate<Object, Object> template = new RedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}

@Bean
@ConditionalOnMissingBean
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
//由于String是redis中最常使用的类型，所以说单独提出来一个bean。
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
    return new StringRedisTemplate(redisConnectionFactory);
}
```



#### RedisTemplate

探讨序列化问题，看RedisTemplate.java

![image-20220103210216748](redis.assets/image-20220103210216748.png)

![image-20220103210952183](redis.assets/image-20220103210952183.png)



### 序列化相关

#### 传递不实现序列化的对象

编写一个不实现序列化的pojo类。

类如下：

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.stereotype.Component;

//把User类变成一个组件
@Component
//使用lombok，简化pojo类的编写（其实不推荐使用lombok，不过这用一下）
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User {
    private String name;
    private int age;

}
```

![image-20220104130233351](redis.assets/image-20220104130233351.png)

---

首先考虑把对象转化为json格式字符串后，存入redis。

测试函数如下：

```java
    @Test
    public void testJsonStringSerilization() throws JsonProcessingException {
        //真实的开发一般都使用json来传递对象,所以要把User对象序列化成（json格式的）String
        User user = new User("张云", 3);
        String userAsJsontypeString = new ObjectMapper().writeValueAsString(user);
        redisTemplate.opsForValue().set("user1",userAsJsontypeString);//传送序列化成jsonstring后的对象
        System.out.println("user1=>"+redisTemplate.opsForValue().get("user1"));
    }
```

控制台输出结果正确，说明成功存入远程redis：

![image-20220104125148969](redis.assets/image-20220104125148969.png)

远程redis查看数据。由于序列化，key的前面跟了很多不可读的字符：

![image-20220104125248286](redis.assets/image-20220104125248286.png)

---

现在考虑直接传对象本体。

测试函数如下：

```java
    @Test
    public void testObjectSerilization() throws JsonProcessingException {
        User user = new User("张云", 3);
        redisTemplate.opsForValue().set("user2",user);//直接传送对象
        System.out.println("user2=>"+redisTemplate.opsForValue().get("user2"));
    }
```

程序执行报错,并且远程redis看不到“user2"。这是因为pojo类User**没有实现序列化，不能直接传输**。

```
2022-01-04 12:54:48.145  INFO 22460 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 5 ms. Found 0 Redis repository interfaces.
2022-01-04 12:54:49.318  INFO 22460 --- [           main] c.z.r.Redis02SpringbootApplicationTests  : Started Redis02SpringbootApplicationTests in 1.814 seconds (JVM running for 2.615)

org.springframework.data.redis.serializer.SerializationException: Cannot serialize; nested exception is org.springframework.core.serializer.support.SerializationFailedException: Failed to serialize object using DefaultSerializer; nested exception is java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.zhangyun.redis02springboot.pojo.User]

	at org.springframework.data.redis.serializer.JdkSerializationRedisSerializer.serialize(JdkSerializationRedisSerializer.java:96)
	at org.springframework.data.redis.core.AbstractOperations.rawValue(AbstractOperations.java:128)
	at org.springframework.data.redis.core.DefaultValueOperations.set(DefaultValueOperations.java:304)
	at com.zhangyun.redis02springboot.Redis02SpringbootApplicationTests.testObjectSerilization(Redis02SpringbootApplicationTests.java:55)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.platform.commons.util.ReflectionUtils.invokeMethod(ReflectionUtils.java:725)
	at org.junit.jupiter.engine.execution.MethodInvocation.proceed(MethodInvocation.java:60)
	at org.junit.jupiter.engine.execution.InvocationInterceptorChain$ValidatingInvocation.proceed(InvocationInterceptorChain.java:131)
	at org.junit.jupiter.engine.extension.TimeoutExtension.intercept(TimeoutExtension.java:149)
	at org.junit.jupiter.engine.extension.TimeoutExtension.interceptTestableMethod(TimeoutExtension.java:140)
	at org.junit.jupiter.engine.extension.TimeoutExtension.interceptTestMethod(TimeoutExtension.java:84)
	at org.junit.jupiter.engine.execution.ExecutableInvoker$ReflectiveInterceptorCall.lambda$ofVoidMethod$0(ExecutableInvoker.java:115)
	at org.junit.jupiter.engine.execution.ExecutableInvoker.lambda$invoke$0(ExecutableInvoker.java:105)
	at org.junit.jupiter.engine.execution.InvocationInterceptorChain$InterceptedInvocation.proceed(InvocationInterceptorChain.java:106)
	at org.junit.jupiter.engine.execution.InvocationInterceptorChain.proceed(InvocationInterceptorChain.java:64)
	at org.junit.jupiter.engine.execution.InvocationInterceptorChain.chainAndInvoke(InvocationInterceptorChain.java:45)
	at org.junit.jupiter.engine.execution.InvocationInterceptorChain.invoke(InvocationInterceptorChain.java:37)
	at org.junit.jupiter.engine.execution.ExecutableInvoker.invoke(ExecutableInvoker.java:104)
	at org.junit.jupiter.engine.execution.ExecutableInvoker.invoke(ExecutableInvoker.java:98)
	at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.lambda$invokeTestMethod$7(TestMethodTestDescriptor.java:214)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.invokeTestMethod(TestMethodTestDescriptor.java:210)
	at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.execute(TestMethodTestDescriptor.java:135)
	at org.junit.jupiter.engine.descriptor.TestMethodTestDescriptor.execute(TestMethodTestDescriptor.java:66)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:151)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at java.util.ArrayList.forEach(ArrayList.java:1259)
	at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at java.util.ArrayList.forEach(ArrayList.java:1259)
	at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.submit(SameThreadHierarchicalTestExecutorService.java:35)
	at org.junit.platform.engine.support.hierarchical.HierarchicalTestExecutor.execute(HierarchicalTestExecutor.java:57)
	at org.junit.platform.engine.support.hierarchical.HierarchicalTestEngine.execute(HierarchicalTestEngine.java:54)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:107)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:88)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.lambda$execute$0(EngineExecutionOrchestrator.java:54)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.withInterceptedStreams(EngineExecutionOrchestrator.java:67)
	at org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:52)
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:114)
	at org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:86)
	at org.junit.platform.launcher.core.DefaultLauncherSession$DelegatingLauncher.execute(DefaultLauncherSession.java:86)
	at org.junit.platform.launcher.core.SessionPerRequestLauncher.execute(SessionPerRequestLauncher.java:53)
	at com.intellij.junit5.JUnit5IdeaTestRunner.startRunnerWithArgs(JUnit5IdeaTestRunner.java:71)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:235)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:54)
Caused by: org.springframework.core.serializer.support.SerializationFailedException: Failed to serialize object using DefaultSerializer; nested exception is java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.zhangyun.redis02springboot.pojo.User]
	at org.springframework.core.serializer.support.SerializingConverter.convert(SerializingConverter.java:64)
	at org.springframework.core.serializer.support.SerializingConverter.convert(SerializingConverter.java:33)
	at org.springframework.data.redis.serializer.JdkSerializationRedisSerializer.serialize(JdkSerializationRedisSerializer.java:94)
	... 70 more
Caused by: java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.zhangyun.redis02springboot.pojo.User]
	at org.springframework.core.serializer.DefaultSerializer.serialize(DefaultSerializer.java:43)
	at org.springframework.core.serializer.Serializer.serializeToByteArray(Serializer.java:56)
	at org.springframework.core.serializer.support.SerializingConverter.convert(SerializingConverter.java:60)
	... 72 more


Process finished with exit code -1

```

![image-20220104125911985](redis.assets/image-20220104125911985.png)

![image-20220104125627899](redis.assets/image-20220104125627899.png)

#### 传递实现序列化的对象

修改pojo类User如下：

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.stereotype.Component;

import java.io.Serializable;

//把User类变成一个组件
@Component
//使用lombok，简化pojo类的编写（其实不推荐使用lombok，不过这用一下）
@AllArgsConstructor
@NoArgsConstructor
@Data
//!!!implements Serializable使User类能够被序列化
public class User implements Serializable {
    private String name;
    private int age;
}
```

再次运行testObjectSerilization方法

![image-20220104130800230](redis.assets/image-20220104130800230.png)

控制台成功展示内容，远程redis成功展示名为“user2”的key。这是因为**User类implements Serializable了，可以被默认的序列化器“JdkSerializationRedisSerializer”所序列化**，所以传输成功。

![image-20220104130916401](redis.assets/image-20220104130916401.png)

![image-20220104130927736](redis.assets/image-20220104130927736.png)



#### 自定义redisTemplate

如果不想要默认的序列化器JdkSerializationRedisSerializer，我们自己实现一个redisTemplate来自定义序列化器，*这样就不会有长串不可读的字符出现*。为了实现这个目的，我们在springboot启动类的同级建立config文件夹并存放RedisConfig.java,以后redis的相关配置在这个类中做：

![image-20220103211406913](redis.assets/image-20220103211406913.png)

包含自定义redisTemplate的完整RedisConfig类如下（含注释），**开发时可以直接使用**！

```java
package com.zhangyun.redis02springboot.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;

//表示被注解的类是一个配置类
@Configuration
public class RedisConfig {
    /*RedisAutoConfiguration.java（如果只有RedisAutoConfiguration.class的话点击“download source”可得到java文件）
    中的redisTemplate方法可以直接借鉴过来。并把所有<Object, Object>改为<String, Object>，因为我们主要面向String*/
    //自己定义了一个RedisTemplate，这是一个固定模板，拿去可以直接使用
    @Bean
    @ConditionalOnMissingBean(name = "redisTemplate")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        //为了我们自己开发方便，一般直接使用<String,Object>
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);//set连接工厂，这个是默认的。

        //序列化器设置
        //json序列化配置
        Jackson2JsonRedisSerializer<Object> objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<Object>(Object.class);//json解析任意的对象，变成一个json序列化。
        ObjectMapper om = new ObjectMapper();//objectmappeer对json序列化进行转义
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectJackson2JsonRedisSerializer.setObjectMapper(om);//转义完毕后就可以使用了
        //String的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        //key采用String序列化方式（序列化器）
        template.setKeySerializer(stringRedisSerializer);
        //hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        //value采用jackson序列化方式
        template.setValueSerializer(objectJackson2JsonRedisSerializer);
        //hash的value采用jackson序列化方式
        template.setHashValueSerializer(objectJackson2JsonRedisSerializer);

        template.afterPropertiesSet();//在template中设置所有的properties
        return template;
    }
}
```

自定义redisTemplate后，由于@ConditionalOnMissingBean注解，原本的redisTemplate bean会失效

![image-20220104155736148](redis.assets/image-20220104155736148.png)

清空redis数据库

![image-20220104155854330](redis.assets/image-20220104155854330.png)

再次执行测试函数testObjectSerilization()

![image-20220104155923698](redis.assets/image-20220104155923698.png)

控制台成功得到结果了。并且远程数据库读取的key没有一大串不可读的字符前缀了。

- 网友说：get value值的时候中文乱码是因为windows的编码是gbk 而数据库的编码格式是utf8  chcp 65001

![image-20220104160144180](redis.assets/image-20220104160144180.png)

![image-20220104160157594](redis.assets/image-20220104160157594.png)



#### redis工具类

redis工具类`RedisUtil.java`位置如下，和代码图示如下。代码太多了就不黏贴进来了：

![image-20220104163438341](redis.assets/image-20220104163438341.png)

新建测试方法，测试RedisUtils.java

```java
    @Autowired
    private RedisUtil redisUtil;
    @Test void testRedisUtil(){
        redisUtil.set("name","zhangyun");
        System.out.println(redisUtil.get("name"));
    }
```

控制台成功输出value，远程redis也正确展示了键值对:

![image-20220104163531348](redis.assets/image-20220104163531348.png)

![image-20220104163615412](redis.assets/image-20220104163615412.png)









## Redis.conf详解

启动redisserver的时候就通过Redis.conf来启动。



#### 单位

容量单位不区分大小写，G和GB有区别：

![img](redis.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70.png)

#### 包含

可以使用 include 组合多个配置文件。这里include redis配置文件就类似spring中配置文件可以用import组合多个配置文件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214902552.png)

#### 网络配置

bind如果只是127.0.0.1的话，就只能本机访问，如果想实现远程访问，这里要bind 0.0.0.0。

![在这里插入图片描述](redis.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70-16412861708662.png)

#### 通用配置

daemonize以守护进程的方式运行，默认是no，我们需要自己开启为yes !要开启，才能在后台一直运行，不然一退出这个进程就结束了：

![image-20220104165607879](redis.assets/image-20220104165607879.png)

配置文件的pid文件。如果以后台（即守护进程）方式运行，那么我们就需要指定pid文件：

![image-20220104165731686](redis.assets/image-20220104165731686.png)

日志输出级别：

![img](redis.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70-16412867791274.png)

日志输出文件位置名。如果为空的话就是一个标准的输出，就不管他 ：

![在这里插入图片描述](redis.assets/20200513214933713.png)

redis数据库的数量，默认是16个

![image-20220104170426612](redis.assets/image-20220104170426612.png)

配置是否展示redisserver启动时的logo：

![image-20220104170519038](redis.assets/image-20220104170519038.png)



#### 持久化

由于Redis是基于内存的数据库，需要将数据由内存持久化到文件中

持久化方式：

- RDB
- AOF



持久化频度：

![在这里插入图片描述](redis.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70-16412875054077.png)

rdb文件相关：

![在这里插入图片描述](redis.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg3MzIyNw==,size_16,color_FFFFFF,t_70-16412875851439.png)

![在这里插入图片描述](redis.assets/20200513215006207.png)

#### 主从复制

这里先不讲。后面讲“redis主从复制”的时候，再进行讲解。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215016371.png)



#### 安全（重要）

密码设置：

- 上线一定设密码 否则会被攻击导致数据全丢，或者被挖矿病毒入侵。
- 默认是没有密码的。可以在config文件这设置，也可以通过命令`config set requirepass 自己的密码`设置。

![在这里插入图片描述](redis.assets/20200513215026143.png)

![image-20220104172307938](redis.assets/image-20220104172307938.png)

#### 限制客户端

```
maxclients 10000   #最大客户端数量
maxmemory <bytes>  #最大内存限制
maxmemory-policy noeviction # 内存达到限制值的处理策略。redis 中的默认的过期策略是 volatile-lru。
```

maxmemory-policy六种过期策略：

- volatile-lru：只对设置了过期时间的key进行LRU（默认值）
- allkeys-lru ： 删除lru算法的key
- volatile-random：随机删除即将过期key
- allkeys-random：随机删除
- volatile-ttl ： 删除即将过期的
- noeviction ： 永不过期，返回错误
  

#### AOF相关

开启与默认文件名：

![在这里插入图片描述](redis.assets/20200513215037918.png)

- 默认不开启AOF，而是使用RDB进行持久化

- 在大部分所有的情况下，rdb完全够用



数据同步策略：

![在这里插入图片描述](redis.assets/20200513215047999.png)

- always：消耗性能
- everysec：可能丢失1s的数据
- no：相当于关闭，基本不使用这个。



## Redis持久化

面试和工作，持久化都是重点!
Redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化功能!

### RDB

(redis database)

#### 什么是RDB

![image-20220106101614861](redis.assets/image-20220106101614861.png)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

我们生产环境常把rdb做备份，因为rdb一旦丢失损失严重！！

Redis会单独创建 ( fork )一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那**RDB方式要比AOF方式更加的高效**。**RDB的缺点是最后一次持久化后的数据可能丢失**。我们默认的就是RDB，一般情况下不需要修改这个配置!

rdb保存的文件**默认名是dump.rdb**，这是在我们的配置文件的快照部分进行配置的!

![image-20220106102303617](redis.assets/image-20220106102303617.png)

![image-20220106102147225](redis.assets/image-20220106102147225.png)

#### 触发机制

触发机制：

1. save的规则（比如60秒修改5次key）满足的情况下，会自动触发rdb规则
2. 执行flushall命令，也会触发我们的rdb规则!
3. 退出（即shutdown）redis（即redisserver），也会产生rdb文件!

备份就自动生成一个dump.rdb

![image-20220106103126251](redis.assets/image-20220106103126251.png)

#### 恢复rdb文件

只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb恢复其中的数据!

查看redis启动目录的位置：

```bash
127.0.0.1:6379>config get dir1) "dir"
I
2) "/usr / loca1/bin" # 如果在这个目录下存在 dump.rdb文件，启动就会自动恢复其中的数据

```

几乎就redis默认的配置就够用了，但是我们还是需要去学习!

#### 优缺点

优点:

- 适合大规模的数据恢复。
- 对数据的完整性要不高。

缺点:

- 需要一定的时间间隔进行操作。如果redis意外宕机了，那么最后一次rdb操作后修改的数据就没有的了。
- 子进程会处理所有保存工作，父进程fork出子进程的时候，会占用一定的内存空间。

### AOF

（append only file）

#### 什么是AOF

将我们的所有命令都记录下来，恢复的时候就把这个文件的所有命令全部再执行一遍：

![image-20220106105634836](redis.assets/image-20220106105634836.png)

以日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

Aof保存的是**appendonly.aof** 文件：

![image-20220106105905590](redis.assets/image-20220106105905590.png)

- 默认是不开启的，我们需要手动进行配置。
- appendonly.aof用vim打开是能看懂的，dump.rdb文件打开是看不懂的

#### 文件重写说明

aof默认就是文件的无限追加，文件会越来越大。

![image-20220106112430199](redis.assets/image-20220106112430199.png)

如果aof文件大于64m，太大了! fork一个新的进程来将我们的文件进行重写。

- 重写例子： 比如你进行了两次自增 他合成一个命令变成直接加2。
- 这里aof的重写是指将旧文件里面的最后一次修改完的数据和后面的写入指令保存到新的文件里面

#### 修复

如果这个aof文件有错误，这时候redis是启动不起来的，我们需要修复这个aof文件。redis给我们提供了一个工具`redis-check-aof --fix`：

- 修复的时候可能会把问题数据直接丢弃，导致数据丢失。但是也比数据全丢弃好！
- 网友：和钱打交道的不会用redis，银行用oracle数据库正确率极高。不过据我所知银行在“去ioe”

#### 优点缺点

优点：

- 因为可以配置每秒/每次修改后sync，所以数据完整性比rdb强很多。

缺点：

- 相对于数据文件来说，aof远远大于rdb，修复的速度也比 rdb慢。
- Aof运行效率也要比rdb慢。

综合来看，redis默认的配置用rdb持久化是合理的。aof几乎不使用。



### 扩展

- RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储
- AOF持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。
- 只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化
- 同时开启两种持久化方式
  - 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
  - RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢?作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。
- 性能建议
  - 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 9001这条规则。
  - 如果Enable AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的lO，二是AOF rewrite 的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
  - 如果不Enable AOF，仅靠Master-Slave Repllcation实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave 中的RDB文件，载入较新的那个，微博就是这种架构。



## Redis发布订阅

https://www.bilibili.com/video/BV1S54y1R7SB?p=30&spm_id_from=pageDriver

https://blog.csdn.net/DDDDeng_/article/details/108118544

#### 基本概念

Redis 发布订阅(pub/sub)是一种**消息通信模式**∶发送者(pub)发送消息，订阅者(sub)接收消息。微信、微博等有类似“关注”的功能都是用“发布订阅”模式。

Redis客户端可以订阅任意数量的频道。



订阅/发布消息图︰

![image-20220106155419409](redis.assets/image-20220106155419409.png)



订阅发布图中有三个角色：

- 消息发送者
- 频道
- 消息订阅者



下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![在这里插入图片描述](redis.assets/20200513215523258.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![image-20220106155935611](redis.assets/image-20220106155935611.png)

#### 命令

| 序号 | 命令                                   | 描述                               |
| ---- | -------------------------------------- | ---------------------------------- |
| 1    | PSUBSCRIBE pattern [pattern..]         | 订阅一个或多个符合给定模式的频道。 |
| 2    | PUNSUBSCRIBE pattern [pattern..]       | 退订一个或多个符合给定模式的频道。 |
| 3    | PUBSUB subcommand [argument[argument]] | 查看订阅与发布系统状态。           |
| 4    | PUBLISH channel message                | **向指定频道发布消息**             |
| 5    | SUBSCRIBE channel [channel..]          | **订阅给定的一个或多个频道。**     |
| 6    | SUBSCRIBE channel [channel..]          | 退订一个或多个频道                 |

#### 测试

在页面1，远程linux使用redis客户端连接redis服务端，监听的频道名字为“zhangyunxue”。

```bash
127.0.0.1:6379> SUBSCRIBE zhangyunxue
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "zhangyunxue"
3) (integer) 1
# 等待读取推送的信息
```

新开一个页面2，同样使用redis客户端去连接redis服务端，往“zhangyunxue”频道传送数据。

```
127.0.0.1:6379> PUBLISH zhangyunxue xuejava!
(integer) 1
```

回到页面1，可以看到redis客户端从订阅的“zhangyunxue”频道收到了消息。

![image-20220106201437959](redis.assets/image-20220106201437959.png)



#### 原理

Redis是使用C实现的，通过分析Redis 源码里的pubsub.c文件，了解发布和订阅机制的底层实现，籍此加深对Redis的理解。Redis通过PUBLISH、SUBSCRIBE和PSUBSCRIBE等命令实现发布和订阅功能。

通过SUBSCRIBE 命令订阅某频道后，redis-server里维护了一个字典，字典的键就是一个个channel，而字典的值则是一个链表，链表中保存了所有订阅这个channel的客户端。SUBSCRIBE命令的关键，就是将客户端添加到给定channel的订阅链表中。

通过PUBLSH命令向订阅者发送消息，redis-server会使用给定的频道作为键，在它所维护的channel字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

Pub/Sub从字面上理解就是发布( Publish)与订阅( Subscribe )，在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。

#### 使用场景

简单场景∶

- 实时消息系统。
- 实时聊天（频道当做聊天室，将信息回显给所有人即可)
- 订阅，关注系统都是可以的。

稍微复杂的场景：

- 我们就会使用消息中间件MQ ( )



## Redis主从复制

### 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower);数据的复制是单向的，只能由主节点到从节点。**Master以写为主，Slave以读为主**，一般至少“**一主二从**”来保证redis集群使用。

默认情况下，每台Redis服务器都是主节点;且一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。主从复制的作用主要包括︰

- 数据冗余∶主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
- 故障恢复︰当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复﹔实际上是一种服务的冗余。
- 负载均衡︰在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载;尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
- 高可用基石︰除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。

一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的，原因如下∶

- 从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大;
- 从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，**单台Redis最大使用内存不应该超过20G**。
  电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是"**多读少写**"。

对于这种场景，我们可以使如下这种架构:

![image-20220106205104815](redis.assets/image-20220106205104815.png)



### 环境配置

只配置从库，不用配置主库。

查看当前库的信息可以用如下命令：

```bash
127.0.0.1:6379> info replication
# Replication
role:master# 角色 master
connected_slaves:0 # 没有从机
master_replid:a47643d622c8547791d6a7febc63ddb40032a5f0
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```



### 一主二从

#### 概念

默认情况下redis集群每个节点都是主节点。我们一般情况下，只配置从机就行。

#### 使用命令实现一主二从实战

关闭redis服务端，关闭redis客户端。

来到redis配置文件所在目录，复制三分配置文件，作为一主二从的redis服务端集群：

![image-20220106213632333](redis.assets/image-20220106213632333.png)

**以修改redis80.conf文件为例**，先修改端口号为6380：

![image-20220106213836612](redis.assets/image-20220106213836612.png)

pidfile要改名：

![image-20220106213904327](redis.assets/image-20220106213904327.png)

日志名称要改：

![image-20220106213923412](redis.assets/image-20220106213923412.png)

dump文件名改一下：

![image-20220106213957124](redis.assets/image-20220106213957124.png)

其他强烈**推荐配置**：

- **主机有密码的小伙伴**，在从机的配置文件conf里加上：masterpass 密码。
- 并且 master和slave都需要配置 requirepass 和masterauth，主要防止发生主从切换后，从节点连接不上主节点。

**redis79.conf和redis81.conf同理修改**。

新开三个页面，在每个页面分别启动redisserver79，80,81

![image-20220106214301100](redis.assets/image-20220106214301100.png)

全都开启完毕后，使用ps命令查看进程，能看到6379,6380,6381端口对应的三个redis服务端进程：

![image-20220106214427949](redis.assets/image-20220106214427949.png)

同时三个页面用redis客户端连接上，以连接6380端口redisserver为例如下所示：

![image-20220106222346142](redis.assets/image-20220106222346142.png)

- 我理解三个页面共六个进程，每个页面有两个进程。以6380端口redisserver所在页面为例：有redisserver进程在提供服务端功能，有rediscli进程连接本页面的redisserver。

通过rediscli查看每个redisserver对应的replication，可以看到redis集群中每个redisserver默认状态下都是master：

![image-20220106222739545](redis.assets/image-20220106222739545.png)

为了实现“一主二从”，我们可以让6379端口redisserver做redis集群的master，6380和6381端口redisserver做redis集群的slaver。仅配置slaver（从）即可。以redisserver6380为例,**使用命令**`slaveof`：

![image-20220106223224538](redis.assets/image-20220106223224538.png)

- 配置redisserver6380为redisserver6379的从机后，重新查看redisserver6380的replication可以看到role已经变成slave了，而不是集群默认状态下的master。

回到作为master的6379redisserver，重新查看replication，可以看到connected_slaves已经不是0了：

![image-20220106223018504](redis.assets/image-20220106223018504.png)

6381redisserver和6380redisserver**同理配置**即可。

三个redisserver一主二从配置完毕后，查看master角色的6379redisserver的repilication，可以按到两个从机的信息：

![image-20220108111125159](redis.assets/image-20220108111125159.png)



#### 通过配置文件实现一主二从实战

真正开发一般都是通过修改配置文件做到一主二从。

上一节“使用命令实现一主二从实战”输入命令行的部分，可以通过配置文件做到：

![image-20220108113649077](redis.assets/image-20220108113649077.png)



### 细节

#### 读写

主机可以写，从机不能写只能读。主机中的所有信息和数据，都会自动被从机保存。

主机写：

![image-20220108114727603](redis.assets/image-20220108114727603.png)

从机只能读不能写：

![image-20220108114909456](redis.assets/image-20220108114909456.png)



#### 无哨兵时主机掉线

主机断开连接，从机依旧连接到主机的，但是没有写操作。这个时候，主机如果回来了，从机依旧可以直接获取到主机写的信息!

#### 无哨兵时从机掉线

如果是用命令行配置的主从复制：

- 从机断开又重连后，从机将成为主机，此时从机再也拿不到在断开期间及复联之后，主机写进去的数据。
  - 但是如果通过命令把机器再次变成主机的从机的话，因为从机数据同步复制主机，从机又可以读到主机所有的数据了。

#### 复制原理

Slave启动成功连接到master后会发送一个sync同步命令，Master接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，**master将传送整个数据文件到slave，并完成一次完全同步**。

全量复制vs增量复制：

- **全量复制**︰而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

- **增量复制**:Master继续将新的所有收集到的修改命令依次传给slave，完成同步.

但是只要是重新连接master，一次完全同步（全量复制)将被自动执行。主机的所有数据一定可以在从机中看到。



### 宕机后手动配置主机

#### 蜈蚣模式

(工作中不会用)

上一个节点作为下一个节点的master。但是整个集群来说，只有最开始的节点是真正的role为master。

![image-20220108131644165](redis.assets/image-20220108131644165.png)

这个时候也可以完整主从复制.



#### master宕机后手动配置主机

**哨兵模式没出来之前**，如果主机断开了连接：

1. 我们可以使用`SLAVEOF no one`手动让一个从机变成主机。
2. 其他的节点就可以手动连接到最新的这个主节点(手动）。
3. 原主机恢复的话，也不会拥有slave了，slave都被新主机抢走了（谋朝篡位）

所以哨兵模式没出来之前，用“命令”配置集群反而方便一点，否则主机宕机后修改主机还有修改配置文件，会比较麻烦。



### 哨兵模式(重点)

自动选举老大的模式。

#### 概念

主从切换技术的方法是:当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了Sentinel (哨兵）架构来解决这个问题。

“谋朝篡位”的自动版，能够后台监控主机是否故障，如果故障了根据投票数**自动将从库转换为主库**。

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，**哨兵是一个独立的进程**，作为进程，它会独立运行。其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。

![image-20220108135313042](redis.assets/image-20220108135313042.png)

这里的哨兵有两个作用：

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用**多个哨兵**进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式：

![image-20220108145850453](redis.assets/image-20220108145850453.png)

- 所以实现一个基本的哨兵模式需要6个线程：三哨兵，三redisserver。

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且检测到“主观下线”的哨兵的数量达到一定值时，那么哨兵之间就会进行一次投票选举新的master，投票的结果由一个哨兵发起，进行**failover**[故障转移]操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机（master），这个过程称为**客观下线**。



#### 实战单个哨兵

这里因为资源有限，只启动一个哨兵（阉割版）。完整的哨兵模式最少要三个哨兵。

1，sentinel.conf

在redis.conf所在的目录中设置哨兵模式的配置文件，名字不能错：`sentinel.conf`，指定被监听的master：

```bash
#sentinel monitor [被监控的名称] [masterhost] [masterport] [哨兵集群中有多少个哨兵主观认为master宕机时, 才真正认为master挂了]
sentinel monitor myredis 127.0.0.1 6379 1
```

2，启动哨兵

在启动redis的目录层级执行下列命令，启动哨兵：

```
[root@zhangyun bin]# redis-sentinel zconfig/sentinel.conf
```

3，master宕机的话

此时如果作为master的redisserver崩了，哨兵会自动从两个slave中选一个出来做master：

- 新master选出来并通知集群完毕后，哪怕原master复活，也会被哨兵安排做新master的从机。



#### 优缺点

优点∶

- 哨兵集群，基于主从复制模式，所有的主从配置优点，它全有
- 主从可以切换，故障可以转移，系统的可用性就会更好
- 哨兵模式就是主从模式的升级，手动到自动，更加健壮

缺点:

- Redis不好啊在线扩容的，集群容量一旦到达上限，在线扩容就十分麻烦
- 实现哨兵模式的配置其实是很麻烦的，里面有很多选择!



#### 哨兵模式的全部配置

```bash
# Example sentinel.conf
 
# 哨兵sentinel实例运行的端口 默认26379
port 26379
 
# 哨兵sentinel的工作目录
dir /tmp
 
# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 当这些quorum个数sentinel哨兵认为master主节点失联 那么这时 客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 1
 
# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd
 
 
# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
 
# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
 
 
 
# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。  
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
 
# SCRIPTS EXECUTION
 
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
 
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
#这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，
#一个是事件的类型，
#一个是事件的描述。
#如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
#通知脚本
# sentinel notification-script <master-name> <script-path>
  sentinel notification-script mymaster /var/redis/notify.sh
 
# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh

```



## Redis缓存穿透和雪崩(重点)

在这里我们不会详细的去分析解决方案的底层

Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。

读请求的示意图：

![image-20220108164935659](redis.assets/image-20220108164935659.png)

### 缓存穿透

核心原因：查不到

#### 缓存穿透概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候（秒杀场景），缓存都没有命中，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

#### 缓存穿透解决方案

1，布隆过滤器

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力;

![image-20220108165822631](redis.assets/image-20220108165822631.png)

2，缓存空对象

当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源;

![image-20220108171158193](redis.assets/image-20220108171158193.png)

但是这种方法会存在两个问题:

- 如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键;
- 即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。



### 缓存击穿

核心原因：访问量过大，缓存过期。

#### 缓存击穿概念

这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。

#### 缓存击穿解决方案

1，设置热点数据永不过期：

从缓存层面来看，没有设置过期时间，所以不会出现热点 key过期后产生的问题。

2，加互斥锁：

分布式锁（**重点，最好细了解**）︰使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。详细内容可以参考”JUC“

![image-20220108172215131](redis.assets/image-20220108172215131.png)

### 缓存雪崩

#### 缓存雪崩概念

缓存雪崩，是指在某一个时间段，缓存集中过期失效。Redis宕机!

产生雪崩的原因之一，比如在写本文的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

击穿是访问一个key，雪崩是redis的数据大批量消失。

![image-20220108172518674](redis.assets/image-20220108172518674.png)

其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压跨。

#### 缓存雪崩解决方案

1，redis高可用：

这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。(异地多活!)

2，限流降级（在SpringCloud讲解过!)：

这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

3，数据预热：

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。



