# Prometheus+grafana+睿像云 监控系统

完整文档[参考](https://blog.csdn.net/hancoder/article/details/121703904)，我自己写的本文档是看视频过程中的简记录。

## 普罗米修斯入门

### 课程介绍

1，普罗米修斯做监控（收集数据），grafana做展现，睿像云做报警。大数据要做“存储传输计算”，计算无论是离线还是实时都得放到服务器去做：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-10-21-12-image.png)

2，监控系统逻辑图示：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-10-28-53-image.png)

- 图片中是“离线/实时应用”

### 课程结构说明

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-10-41-58-image.png)

### 普罗米修斯特点

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-11-01-48-image.png)

### 普罗米修斯架构

![img](https://img-blog.csdnimg.cn/img_convert/d6476d493d8232e51931cdae138f05f4.png)

- 后续会让grafana连接上睿像云，让睿像云做报警。

## 普罗米修斯安装和配置

### 安装普罗米修斯server

1，解压普罗米修斯安装包后可以看到：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-13-26-37-image.png)

- 运行绿色的普罗米修斯

- yml需要修改一下

2，yml包含 全局配置 告警配置 普罗米修斯的配置

3，先说全局配置：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-13-29-46-image.png)

- scrapeinterval是拉取时间间隔，设置为15s

- evaluationiunterval是告警时间间隔默认1min，设置为15s

4，再说告警相关配置：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-13-31-03-image.png)

- 但是我们不用alertmanageer做告警，所以这块忽略掉

5，最后说重点，普罗米修斯配置：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-13-37-25-image.png)

- jobname：任务名

- target：从哪个主机：端口获取数据，普罗米修斯端口号默认是9090

- 数据采集有两种方式：pushgateway 或 exporter
  
  - 这里node export拿数据是从三个节点上拿数据，端口号都设置为9100。

### 安装pushgateway

略

### 安装 alertmanager（付费 可选）

略。装了也不用。

1，安装后的yml问价中可以看到一些报警规则：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-14-59-19-image.png)

### 安装node exporter（可选）

本课程使用pushgateway，所以这个可以不装。

- 网友评论：不如用docker简单
  
  - 网友反驳：node exporter 由于要获取主机信息，不建议放容器里

### 启动普罗米修斯server+pushgateway+altermanager

1，普罗米修斯页面展示的所有up都变绿了，说明服务启动完成了：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-15-24-46-image.png)

## PromQL介绍

### 基本用法

略。参考文章开头给的完整笔记的地址。

### 实操

1，可以在普罗米修斯的页面输入promql，做一些基本的数据查询：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-15-29-24-image.png)

2，点击地球选中要查询的指标，再点击execute就会得到查询的结果：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-15-31-27-image.png)

## 普罗米修斯集成flink

1，`nc -lk`的用处：[Linux 使用NC命令监听本地端口 - 走看看](http://t.zoukankan.com/lazy-sang-p-11857377.html)

2，flink把数据放到pushgateway上，那么prometheus就可以从pushgateway上拿到数据：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-16-33-24-image.png)

3，flink和prometheus集成成功的话，点击 地球 可以看到 flink 开头的指标：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-16-42-57-image.png)

- 因为flink-conf.yaml中的配置会把flink程序运行时采集到的数据，push到指定的pushgateway中；普罗米修斯会从pushgateway中拿数据，也便能在“metric explorer”中看到各种flink开头的参数。

## prometheus和grafana集成

### 安装，启动 grafana

略

### grafana添加数据源为prometheus

略

### 手动创建仪表盘dashboard

1，创建方式有两种：手动添加 or 从官网下载别人共享的

2，一个row可以包含多个panel

### 添加相关仪表盘模板

1，官网有很多dashboard的模板，参考两个指标：下载数 和 review数

2，使用dashboard模板，其实使用的就是一个json格式的模板定义。

### 创建相关组件的启停脚本

1，要启动如下这些组件，一个个做会比较麻烦；所以考虑写一个一键启动的脚本：

![](C:\Users\15831\AppData\Roaming\marktext\images\2022-06-07-17-30-18-image.png)

2，具体脚本见文章首部的完整版笔记。

### 配置案例

#### 任务失败的监控

1，比如30s上报一次数据，那么0,30,60,90,90,90,90，，，看到数据不变了，就知道90s的时候任务失败了。

2，当前数据上报时间和30s前的数据上报时间是一样的，说明这30s都没有上报，也就说明服务挂了。

#### 网络延时或任务重启监控

略

#### 重启次数监控

略

## 集成第三方告警平台睿像云

1，有免费试用，有专业版。
