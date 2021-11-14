# Redis

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
   - 严格的一致性（CAP中的C）
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

新开一个窗口，在redis软件所在的目录（bin）执行下列命令

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

明白Redis是很快的，官方表示，Redis是基于内存操作，CPU不是Redis性能瓶颈，Redis的瓶颈是根据机器的内存和网络带宽，既然可以使用单线程来实现，就使用单线程了!

Redis是C语言写的，官方提供的数据为100000+的QPS，完全不比同样是使用

key-vale的Memecache差!

Redis为什么单线程还这么快?

- 误区1∶高性能的服务器一定是多线程的?
- 误区2∶多线程（CPU上下文会切换! )一定比单线程效率高!先去CPU>内存>硬盘的速度要有所了解!

**核心**: redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程(CPU上下文会切换︰耗时的操作!! ! )反而更耗时，对于内存系统来说，如果没有上下文切换效率就是最高的!多次读写都是在一个CPU上的，在内存情况下，这个就是最佳的方案!



## 五大数据类型

官网文档

![image-20211113145944923](redis.assets/image-20211113145944923.png)

全段翻译︰
Redis是一个开源(BSD许可)的，内存中的数据结构存储系统，它可以用作**数据库**、**缓存**和**消息中间件MQ**。它支持多种类型的数据结构，如字符串( strings )，散列( hashes )，列表(lists )，集合( sets )，有序集合( sorted sets）与范围查询，bitmaps,hyperloglogs和地理空间( geospatial ）索引半径查询。Redis 内置了复制( replication )，LUA脚本( Lua scripting )，LRU驱动事件(LRU eviction )，事务( transactions）和不同级别的磁盘持久化( persistence )，并通过Redis哨兵( Sentinel )和自动分区( Cluster )提供高可用性( high availability )。

### Redis-Key相关简单操作

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
set user:1 {name:zhangyun,age:3}
```

使用mset的方式新建对象，或修改对象的值

- 这里的key是一个巧妙的设计：user:{id}:{field}，这种设计在redis中是完全ok的

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

所有list大部分命令都是用l开头的，lpush和rpush组合是一个例外。

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

set的基础上为每一个元素增加一个序号



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

**ZRANGEBYSCORE**为有序集合按照编号升序排序：

- 降序要使用ZREVRANGEBYSCORE 命令
- ZRANGEBYSCORE还能添加一些其他参数，比如 `withscore`，表示排序同时把编号也打印出来。

```
# low为-inf表示编号从负无穷开始计入排序，up为+inf表示编号截止到正无穷计入排序
ZRANGEBYSCORE zsetname low up
```

![image-20211114211041978](redis.assets/image-20211114211041978.png)

https://www.bilibili.com/video/BV1S54y1R7SB?p=17&spm_id_from=pageDriver

9.54

## 三种特殊的数据类型

