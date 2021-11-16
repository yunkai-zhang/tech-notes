### 安装基础+搭建伪分布式（视频+博客）

#### 准备工作
- Linux环境：Centos 7.6
- hadoop版本：hadoop3.1.2
- jdk版本：1.8

- 远程登陆：xShell 7
  1. 登录服务器

- vim命令（默认已经会了），只贴本文用到的
  - vi 文件名 打开文件
  - i 编辑
  - Esc 退出编辑
  - :wq 保存并退出
  - 键盘控制光标上下左右

#### 检查&安装JDK

1. 检查系统是否存在openjdk

   ```
   rpm -qa | grep java
   
   ```

   若有java相关的软件显示，则表示存在，进行卸载：

   ```
   rpm -e --nodeps jdk 软件名称
   ```

   若没有显示，则不存在。

2. 下载&解压jdk

   从官网下载gz版的jdk到本机，再用xftp传到linux

   - 这里下载jdk8，因为使用jdk9及以上版本，启动Hadoop时会出现难以解决的问题，我搜索尝试了很多办法，得出结论:目前没有办法通过配置文件解决。
   - linux系统安装java可以参考linux.md笔记

   解压jdk，解压到哪个目录可以自行选择，但是要知道路径，配置环境变量时需要

   ```
   tar -zxvf jdk压缩包名称 -C 解压到哪个目录
   ```

   例如：

   ```
   tar -zxvf jdk-8u181-linux-x64.tar.gz -C /usr/local/
   ```

3. 配置java环境变量

   - 呼应linux.md笔记中的记载：rpm安装java不需要配置安装环境变量；安装包解压安装java需要配置环境变量
   - 配置环境变量时，`PATH=$PATH:`,`CLASSPATH=.:`,`$PATH`和`.`表示承接之前的变量。不带上的话会覆盖之前的环境变量导致出错。
     - 覆盖后也不需要像某些网友说的，就重装系统，可以在环境变量位置前面加上`$PATH`和`.`，在用source使之生效即可。

   打开proflie文件

   ```
   vim /etc/profile
   ```

   将光标移动至文末，按i进入编辑模式，在文末添加以下内容

   ```
   export JAVA_HOME=jdk路径
   export PATH=$PATH:$JAVA_HOME/bin
   export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   ```

   例如：

   ```
   export JAVA_HOME=/usr/local/jdk1.8.0_181
   export PATH=$PATH:$JAVA_HOME/bin
   export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
   ```

   按Esc键退出编辑，输入 `:wq` 保存并退出文件，执行以下内容使环境变量生效

   ```
   source /etc/profile
   ```

4. 检查jdk是否安装配置成功

   ```
   java -version
   ```

   出现java版本信息，则配置成功，例如

   > java version “1.8.0_181”
   > Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
   > Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)

#### 配置Hadoop

1. 下载hadoop

   从官网下载hadoop-3.1.2到本机，并用xftp上传到linux服务器

2. 解压hadoop，解压到哪个目录可以自行选择，但是要知道路径，配置环境变量时需要

   ```
   tar -zxvf hadoop-3.1.2.tar.gz -C /usr/local/
   ```

3. 配置hadoop（本文涉及到的ct01，均需要改为自己的主机名或者ip）

   1）配置环境变量

   ```
   vi /etc/profile
   ```

   按`i`进入编辑模式，在文末添加

   ```
   export HADOOP_HOME=/hadoop路径
   export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
   ```

   例如

   ```
   export HADOOP_HOME=/usr/local/hadoop-3.1.2
   export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
   ```

   按Esc退出编辑，输入 :wq 保存并退出，执行以下内容使环境变量生效

   ```
   source /etc/profile
   ```

   2）配置hadoop-env.sh

   切到/usr/local/hadoop-3.1.2/etc/hadoop目录下，2）- 6）都是在这个目录下配置

   ```
   cd /usr/local/hadoop-3.1.2/etc/hadoop
   ```

   打开hadoop-env.sh

   ```
   vi hadoop-env.sh
   ```

   找到# export JAVA_HOME=,按i进入编辑模式，修改为以下内容后，按Esc，输入 :wq 保存并退出

   ```
   export JAVA_HOME=/usr/local/jdk1.8.0_181
   ```

   3）配置core-site.xml，修改为以下内容，其中第5行 ct01 改为自己的主机名（用`hostname查看`）

   ```xml
   <configuration>
       <!--设置namenode所在节点-->
       <property>
           <name>fs.defaultFS</name>
           <value>hdfs://ct01:9000</value>
       </property>
       <!--
           设置hadoop存放数据的目录，Hadoop默认把数据块的元数据和数据存放在操作系统的/tmp目录下，但操作系统的/tmp目录会不定时清空，所以要做修改
       -->
       <property>
           <name>hadoop.tmp.dir</name>
           <value>/usr/local/hadoop-3.1.2/data</value>
       </property>
   </configuration>
   ```

   4）配置yarn-site.xml，修改为以下内容，ct01 均改为自己的主机名

   ```xml
   <configuration>
       <!-- 指定ResourceManager的地址-->
       <property>
   		<name>yarn.resourcemanager.hostname</name>
   		<value>ct01</value>
       </property>
       <property>
   		<name>yarn.resourcemanager.address</name>
   		<value>ct01:8032</value>
       </property>
       <property>
   		<name>yarn.resourcemanager.scheduler.address</name>
   		<value>ct01:8030</value>
       </property>
       <property>
   		<name>yarn.resourcemanager.resource-tracker.address</name>
   		<value>ct01:8031</value>
       </property>
       <property>
   		<name>yarn.resourcemanager.admin.address</name>
   		<value>ct01:8033</value>
       </property>
       <property>
   		<name>yarn.resourcemanager.webapp.address</name>
   		<value>ct01:8088</value>
       </property>
       <!-- 指定reducer获取数据的方式-->
       <property>
           <name>yarn.nodemanager.aux-services</name>
           <value>mapreduce_shuffle</value>
       </property>
   </configuration>
   ```

   5）配置hdfs-site.xml，修改为以下内容

   ```xml
   <configuration>
       <!--设置block副本数，不能超过节点数-->
       <property>
           <name>dfs.replication</name>
           <value>1</value>
       </property>
   </configuration>
   ```

   6）配置mapred-site.xml，修改为以下内容

   ```xml
   <configuration>
       <property>
           <name>mapreduce.framework.name</name>
           <value>yarn</value>
       </property>
   </configuration>
   ```

   7）配置start-dfs.sh，stop-dfs.sh

   切到/usr/local/hadoop-3.1.2/sbin目录,7）- 8）在这个目录配置

   ```
   cd /usr/local/hadoop-3.1.2/sbin
   ```

   在start-dfs.sh，stop-dfs.sh的最开头添加以下内容，因为是root登录的，不配置会出现错误

   ```
   HDFS_DATANODE_USER=root  
   HDFS_DATANODE_SECURE_USER=hdfs  
   HDFS_NAMENODE_USER=root  
   HDFS_SECONDARYNAMENODE_USER=root
   ```

   8）配置start-yarn.sh，stop-yarn.sh，在文件最开头添加以下内容

   ```
   YARN_RESOURCEMANAGER_USER=root
   HADOOP_SECURE_DN_USER=yarn
   YARN_NODEMANAGER_USER=root
   ```

4. 实现SSH免密码远程登录，因为是伪分布式集群，其实就是自己登录自己（如果已经配置过可跳过）

   1）创建公钥/私钥，一路回车

   ```
   ssh-keygen -t rsa
   ```

   2）创建authorized_keys文件并修改权限为600

   ```
   cd ~/.ssh
   touch authorized_keys
   chmod 600 authorized_keys
   ```

   3）将公钥追加到authorized_keys文件中

   ```
   cat id_rsa.pub >> authorized_keys
   ```

   尝试能否免密登录，能登陆就成功了，尝试后，输入exit退出

   ```
   ssh 自己的主机名
   ```

5. 关闭防火墙（如果已经配置过可跳过）

   查看防火墙状态

   ```
   firewall-cmd --state
   ```

   1）关闭防火墙
   
   - 因为腾讯云本身就有port安全组和防火墙，比较安全，这里关掉即可
   
   ```
   systemctl stop firewalld.service
   ```
   
   2）禁止开机时防火墙自启
   
   ```
   systemctl disable firewalld.service
   ```

#### 格式化&启动&停止hadoop

1. 格式化hadoop，不要多次格式化。多次格式化会导致DataNode无法正常启动，解决办法在文末

   ```
   hadoop namenode -format
   ```

2. 启动&停止hadoop

   ```
   start-all.sh
   ```

   ```
   stop-all.sh
   ```

   启动以后可以输入jps，看有没有下面这些进程

   >16352 NameNode
   >21362 Jps
   >16644 SecondaryNameNode
   >17017 NodeManager
   >16475 DataNode
   >16891 ResourceManager

   输入netstat -tpnl | grep java，查看是否有9870和8088端口，刚启动需要等待一会儿才能查询到端口，等待时间视虚拟机情况而定

   ```
   netstat -tpnl | grep java
   ```

#### 遇到的一些坑

1. 多次格式化会导致DataNode无法正常启动（很多人遇到这个问题）

   1）打开/usr/local/hadoop-3.1.2/data/dfs

   ```
   cd /usr/local/hadoop-3.1.2/data/dfs
   ```

   2）有name和data两个目录，将data/current/VERSION中clusterID的值改为name/current/VERSION中clusterID的值

   找到并复制clusterID的值

   ```
   cat name/current/VERSION
   ```

   修改clusterID的值

   ```
   vi data/current/VERSION
   ```

   3）重新启动hadoop

2. ip:50070无法访问

   hadoop 3.X版本以上，50070端口已改为9870端口，访问ip：9870即可

3. 解决问题的最好办法

   查看logs！如果看不懂请百度。修改之后记得重启hadoop

   - 以本文为例，logs路径为：/usr/local/hadoop-3.1.2/logs



[参考文档](https://yangyefu.github.io/2019/05/07/Centos7%E9%85%8D%E7%BD%AEhadoop3.1.2%20%E4%BC%AA%E5%88%86%E5%B8%83%E5%BC%8F%E8%AF%A6%E7%BB%86%E8%BF%87%E7%A8%8B/)

