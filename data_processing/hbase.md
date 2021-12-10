

## （hadoop中毒版）安装Hbase

**之前hadoop中毒时，根据csdn等博客安装的hbase。我得重新装一次。这个之前的安装笔记留存不删掉，当做经验存留。**

### 下载+上传+解压

先查看自己的hadoop版本

![image-20211205112238868](hbase.assets/image-20211205112238868.png)

然后到[官网](https://hbase.apache.org/book.html#hadoop)，查看和自己hadoop版本相匹配的hbase版本

![image-20211205112424664](hbase.assets/image-20211205112424664.png)

来到北理工的[镜像](http://mirror.bit.edu.cn/apache/hbase/)，下载对应版本的hbase包

![image-20211205112553344](hbase.assets/image-20211205112553344.png)

![image-20211205112631494](hbase.assets/image-20211205112631494.png)

通过xFTP把压缩包传送到linux服务器

![image-20211205112807926](hbase.assets/image-20211205112807926.png)

![image-20211205174210427](hbase.assets/image-20211205174210427.png)

使用如下命令解压压缩包

```
tar -zxvf hbase-2.3.7-bin.tar.gz -C /usr/local 
```

查看解压结果

![image-20211205174313916](hbase.assets/image-20211205174313916.png)



### 配置hbase

执行如下代码,配置HBASE_HOME 和 PATH

```
vim /etc/profile
```

```
JAVA_HOME=jdk地址（文件里已经有的话就不用加了）
HADOOP_HOME=hadoop解压（文件里已经有的话就不用加了）
export HBASE_HOME=/usr/local/hbase-2.3.7
export PATH=$PATH:$HBASE_HOME/bin
```

保存

```
ESC
```

```
:wq
```

执行如下代码来刷新配置

```
source /etc/profile
```

修改 $HBASE_HOME/conf/ 目录下的 **hbase-env.sh**，

- 添加 JAVA_HOME，文件中自带了，只需要取消注释即可。

- 放开HBASE_MANAGES_ZK，如果你是使用hbase自带的zk就是true，如果使用自己的zk就是false。这里使用hbase自带的。

```
cd /usr/local/hbase-2.3.7/conf
```

```
vim hbase-env.sh
```

```
export JAVA_HOME=/usr/java/jdk1.8.0_311-amd64
export HBASE_MANAGES_ZK=true
```

![image-20211205190925654](hbase.assets/image-20211205190925654.png)

![image-20211206011916208](hbase.assets/image-20211206011916208.png)

修改 $HBASE_HOME/conf/ 目录下的 **hbase-site.xml**为以下内容

```
<configuration>
  <property>
    <name>hbase.rootdir</name>
    <!-- value 的 URL 地址请与hadoop配置文件core-site.xml
                     中的 fs.default.name 保持一致，然后再加上 /hbase。
    此处建议用主机名（不建议直接填写 IP 地址，否则可能出错）-->
    <value>hdfs://zhangyun:9000/hbase</value>
  </property>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.tmp.dir</name>
    <value>./tmp</value>
  </property>
  <property>
    <name>hbase.unsafe.stream.capability.enforce</name>
    <value>false</value>
  </property>
</configuration>
```



### 启动hadoop

启动hbase之前需要先启动hadoop

```bash
cd /usr/local/hadoop-3.1.2/sbin
```

```bash
start-all.sh
```

输入`jps`查看线程，确认hadoop启动成功

![image-20211205173748049](hbase.assets/image-20211205173748049.png)

访问`服务器公网ip:9870/`,确认hadoop成功启动。

![image-20211205114229172](hbase.assets/image-20211205114229172.png)

### 启动hbase

进入habse解压的目录，执行启动命令。127.0.0.1（本机）第一次连接的话，确认下添加到known_hosts。

```
cd /usr/local/hbase-2.3.7
```

```
./bin/start-hbase.sh 
```

![image-20211205192851992](hbase.assets/image-20211205192851992.png)

使用jps查看hbase启动情况，会发现比只启动hadoop时多了3个进程 HMaster、HRegionServer 和 HQuorumPeer

```
jps
```

![image-20211205193122791](hbase.assets/image-20211205193122791.png)

确保腾讯云防火墙放开了`16010`端口

![image-20211205193528384](hbase.assets/image-20211205193528384.png)

然后访问 `服务器外网ip:16010` （HBase 1.0 之后，60010端口被修改为16010），能够成功访问的话，说明配置成功。

- 注意这里可能由于网络原因会出现：先访问成功，一段时间刷新后访问失败。碰到这个情况可以新开页面输入链接访问试试，或者换个浏览器输入链接访问试试。

![image-20211205193839573](hbase.assets/image-20211205193839573.png)



## 跟着官网安装hbase

环境：

- centos7.6（腾讯云轻量级应用服务器）
- jdk1.8 arm 64，**一定要官方下载，否则可能中毒欲哭无泪，球球**。java+hbase规范查看[网址](https://hbase.apache.org/book.html#java)
- hadoop2.10.1，**一定要官方下载，否则可能中毒欲哭无泪，球球**
- 强烈建议跟着[官方文档](https://hbase.apache.org/book.html#quickstart_pseudo)来装，当然同时参考多方信息源避免坑。
  - 先学基本原理，再来安装



### 下载解压并配置

1）Choose a download site from this list of [Apache Download Mirrors](https://www.apache.org/dyn/closer.lua/hbase/). Click on the suggested top link. This will take you to a mirror of *HBase Releases*. Click on the folder named *stable* and then download the binary file that ends in *.tar.gz* to your local filesystem. Do not download the file ending in *src.tar.gz* for now.

去[北理工镜像](http://mirror.bit.edu.cn/apache/hbase/)，安装2.3.X版本的hbase

![image-20211207154041957](hbase.assets/image-20211207154041957.png)

通过XFTP上传到linux服务器

![image-20211207154315559](hbase.assets/image-20211207154315559.png)

2）Extract the downloaded file, and change to the newly-created directory.

```
cd /root/packages/

tar -xzvf hbase-2.3.7-bin.tar.gz -C /usr/local/

cd /usr/local/hbase-2.3.7
```

3）You must set the `JAVA_HOME` environment variable before starting HBase. To make this easier, HBase lets you set it within the *conf/hbase-env.sh* file. You must locate where Java is installed on your machine, and one way to find this is by using the *whereis java* command. Once you have the location, edit the *conf/hbase-env.sh* file and uncomment the line starting with *#export JAVA_HOME=*, and then set it to your Java installation path.

Example extract from *hbase-env.sh* where *JAVA_HOME* is set

```
# Set environment variables here.
# The java implementation to use.
export JAVA_HOME=/usr/jdk64/jdk1.8.0_112
```

执行如下操作:

```
whereis java


vim conf/hbase-env.sh
```

![image-20211207155326542](hbase.assets/image-20211207155326542.png)

![image-20211207155232698](hbase.assets/image-20211207155232698.png)

为了伪分布式，配置使用habse自带的zk

![image-20211207214331245](hbase.assets/image-20211207214331245.png)

为了伪分布式，使用hdfs作为HBase的储存

![image-20211207215423907](hbase.assets/image-20211207215423907.png)

4）这里来到伪分布相关的配置了。Configure HBase.

Edit the *conf/hbase-site.xml* configuration. First, add the following property which directs HBase to run in distributed mode, with one JVM instance per daemon.

```
<property>
  <name>hbase.cluster.distributed</name>
  <value>true</value>
</property>
```

Next, add a configuration for `hbase.rootdir`, pointing to the address of your HDFS instance, using the `hdfs://` URI syntax. In this example, HDFS is running on the localhost at port 8020.

- 这里的端口号到底用什么，可以参考[网址](https://stackoverflow.com/questions/22855232/default-namenode-port-of-hdfs-is-50070-but-i-have-come-across-at-some-places-802)。因为hadoop那我配置的fs.defaultFS的端口是9000，所以我的hdfs的端口是9000。不同版本的hadoop有不同的默认端口

  ![image-20211207161043087](hbase.assets/image-20211207161043087.png)

```
<property>
  <name>hbase.rootdir</name>
  <value>hdfs://localhost:9000/hbase</value>
</property>
```

You do not need to create the directory in HDFS. HBase will do this for you. If you create the directory, HBase will attempt to do a migration, which is not what you want.

Finally, **remove** existing configuration for `hbase.tmp.dir` and `hbase.unsafe.stream.capability.enforce`,

![image-20211207161658199](hbase.assets/image-20211207161658199.png)



### 开启并使用hbase

1）Start HBase.

Use the *bin/start-hbase.sh* command to start HBase. If your system is configured correctly, the `jps` command should show the HMaster and HRegionServer processes running.

运行下面的代码，启动hbase。

- **启动前确保hadoop+yarn启动了**

```
cd /usr/local/hbase-2.3.7

bin/start-hbase.sh
```

![image-20211207162131244](hbase.assets/image-20211207162131244.png)

![image-20211207162355445](hbase.assets/image-20211207162355445.png)

2）Check the HBase directory in HDFS.

If everything worked correctly, HBase created its directory in HDFS. In the configuration above, it is stored in */hbase/* on HDFS. You can use the `hadoop fs` command in Hadoop’s *bin/* directory to list this directory.

在hadoop安装目录下执行下面语句，查看文件夹和文件是否被正确创建

```
./bin/hadoop fs -ls /hbase
```

![image-20211207163123621](hbase.assets/image-20211207163123621.png)

也可以通过hdfs的webui查看文件夹内容：

![image-20211207163216146](hbase.assets/image-20211207163216146.png)



## 使用Hbase

### createTable

#### 新建java项目

![image-20211205232826804](hbase.assets/image-20211205232826804.png)

![image-20211205232836535](hbase.assets/image-20211205232836535.png)

![image-20211205233416975](hbase.assets/image-20211205233416975.png)

![image-20211205233510596](hbase.assets/image-20211205233510596.png)

在File->Project Structure->Libraries中，添加 HBase安装目录中lib目录与lib/client-facing-thirdparty目录下的jar包。

![image-20211205233946436](hbase.assets/image-20211205233946436.png)

![image-20211205234418901](hbase.assets/image-20211205234418901.png)

![image-20211205234842884](hbase.assets/image-20211205234842884.png)

![image-20211205234851122](hbase.assets/image-20211205234851122.png)

![image-20211205234942580](hbase.assets/image-20211205234942580.png)

![image-20211205235018812](hbase.assets/image-20211205235018812.png)

![image-20211205235025883](hbase.assets/image-20211205235025883.png)

![image-20211205235049428](hbase.assets/image-20211205235049428.png)

![image-20211205235131171](hbase.assets/image-20211205235131171.png)

#### 编写createTable程序

注意包不要导错了

```java
package hbase;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.*;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class createTable {
    public static void main(String[] args) throws IOException {
        /*********得到配置文件类对象**********/
        //创建一个配置文件类对象
        Configuration conf = HBaseConfiguration.create();
        //设置配置文件类的相关参数，以下四个配置参数需与HBase/conf目录下hbase-site.xml文件内容一致。
        // 默认配置的配置参考（http://blog.sina.com.cn/s/blog_63a075af0101izmx.html）
        conf.set("hbase.rootdir","hdfs://zhangyun:9000/hbase");
        conf.set("hbase.zookeeper.property.dataDir", "/tmp");
        conf.set("hbase.zookeeper.quorum", "zhangyun");
        conf.set("hbase.cluster.distributed", "true");

        /*************得到与HBase建立连接的类对象**************/
        //使用连接工厂类ConnectionFactory的createConnection()方法构造一个连接类对象。该方法以一个配置类对象作为输入参数。
        Connection connection = ConnectionFactory.createConnection(conf);

        /**********************得到表格管理类对象************************/
        /*
        使用连接类对象的getAdmin()方法获得表格管理类HBaseAdmin对象。
        由于该方法返回的是抽象类Admin，需将其强制类型转化为HBaseAdmin
        */
        HBaseAdmin admin = (HBaseAdmin) connection.getAdmin();

        /***************创建一张名为“class”的表，该表包含两个列簇“information”与“results”。*****************/
        /*
        * 使用HBaseAdmin类创建一张HBase表的流程为：声明一个表格描述器构造器类TableDescriptorBuilder对象，并初始化表名为“class”
        * */
        TableDescriptorBuilder tdb = TableDescriptorBuilder.newBuilder(TableName.valueOf("class"));
        /*
        * 使用HBaseAdmin类创建一张HBase表的流程（续1）：使用列族描述器构造器类的build()方法创建列族描述器ColumnFamilyDescriptor类对象。
        * 如果有多个列族，则可以存放在Java中自带的数据结构List中。
        * */
        List<ColumnFamilyDescriptor> listColumns = new ArrayList<>();
        ColumnFamilyDescriptor cfd_info = ColumnFamilyDescriptorBuilder.newBuilder("information".getBytes()).build();
        ColumnFamilyDescriptor cfd_res = ColumnFamilyDescriptorBuilder.newBuilder("results".getBytes()).build();
        listColumns.add(cfd_info);
        listColumns.add(cfd_res);
        /*
        *  使用HBaseAdmin类创建一张HBase表的流程（续2）：使用TableDescriptorBuilder类的setColumnFamilies()方法，
        * 将存储多个ColumnFamilyDescriptor类的List对象加入到TableDescriptorBuilder类
        * */
        tdb.setColumnFamilies(listColumns);
        /*
        * 使用TableDescriptorBuilder类的build()方法创建表格描述器TableDescriptor类
        * */
        TableDescriptor td = tdb.build();
        //使用HBaseAdmin类的createTable()方法创建HBase表
        admin.createTable(td);

        //收尾工作
        admin.close();
        connection.close();
    }
}
```

#### 运行所编写的代码

点击“Main class”输入框旁边的按钮，在弹出窗口中 选择想要运行的主类“createTable”即完成配置。此时 IDEA右上角的“Add Configuration”按钮将显示所选择的 主类，点击旁边“运行”按钮即可运行。

![image-20211206001812455](hbase.assets/image-20211206001812455.png)

报错如下

```
2021-12-06 00:18:20,728 WARN  [main] util.Shell (Shell.java:<clinit>(679)) - Did not find winutils.exe: {}
java.io.FileNotFoundException: java.io.FileNotFoundException: HADOOP_HOME and hadoop.home.dir are unset. -see https://wiki.apache.org/hadoop/WindowsProblems
	at org.apache.hadoop.util.Shell.fileNotFoundException(Shell.java:534)
	at org.apache.hadoop.util.Shell.getHadoopHomeDir(Shell.java:555)
	at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
	at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
	at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
	at org.apache.hadoop.conf.Configuration.getBoolean(Configuration.java:1567)
	at org.apache.hadoop.hbase.HBaseConfiguration.checkDefaultsVersion(HBaseConfiguration.java:70)
	at org.apache.hadoop.hbase.HBaseConfiguration.addHbaseResources(HBaseConfiguration.java:84)
	at org.apache.hadoop.hbase.HBaseConfiguration.create(HBaseConfiguration.java:98)
	at hbase.createTable.main(createTable.java:16)
Caused by: java.io.FileNotFoundException: HADOOP_HOME and hadoop.home.dir are unset.
	at org.apache.hadoop.util.Shell.checkHadoopHomeInner(Shell.java:454)
	at org.apache.hadoop.util.Shell.checkHadoopHome(Shell.java:425)
	at org.apache.hadoop.util.Shell.<clinit>(Shell.java:502)
	... 6 more
2021-12-06 00:18:20,783 WARN  [main] util.NativeCodeLoader (NativeCodeLoader.java:<clinit>(62)) - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:zookeeper.version=3.5.7-f0fdd52973d373ffd9c86b81d99842dc2c7f660e, built on 02/10/2020 11:30 GMT
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:host.name=LAPTOP-5G63ITR0
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.version=1.8.0_301
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.vendor=Oracle Corporation
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.home=C:\Program Files\Java\jdk1.8.0_301\jre
2021-12-06 00:18:21,295 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.class.path=C:\Program Files\Java\jdk1.8.0_301\jre\lib\charsets.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\deploy.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\access-bridge-64.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\cldrdata.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\dnsns.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\jaccess.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\jfxrt.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\localedata.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\nashorn.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\sunec.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\sunjce_provider.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\sunmscapi.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\sunpkcs11.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\ext\zipfs.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\javaws.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\jce.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\jfr.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\jfxswt.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\jsse.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\management-agent.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\plugin.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\resources.jar;C:\Program Files\Java\jdk1.8.0_301\jre\lib\rt.jar;D:\CodeProjects\GitHub\BUPT_private\PGcoursework\bigdata-appli\out\production\bigdata-appli;D:\Resources\myTempFile\lib\asm-3.1.jar;D:\Resources\myTempFile\lib\fst-2.50.jar;D:\Resources\myTempFile\lib\guice-3.0.jar;D:\Resources\myTempFile\lib\avro-1.7.7.jar;D:\Resources\myTempFile\lib\gson-2.2.4.jar;D:\Resources\myTempFile\lib\okio-1.6.0.jar;D:\Resources\myTempFile\lib\joni-2.1.11.jar;D:\Resources\myTempFile\lib\jsch-0.1.54.jar;D:\Resources\myTempFile\lib\xmlenc-0.52.jar;D:\Resources\myTempFile\lib\guava-11.0.2.jar;D:\Resources\myTempFile\lib\jets3t-0.9.0.jar;D:\Resources\myTempFile\lib\jetty-6.1.26.jar;D:\Resources\myTempFile\lib\okhttp-2.7.5.jar;D:\Resources\myTempFile\lib\ehcache-3.3.1.jar;D:\Resources\myTempFile\lib\json-io-2.5.1.jar;D:\Resources\myTempFile\lib\paranamer-2.3.jar;D:\Resources\myTempFile\lib\caffeine-2.8.1.jar;D:\Resources\myTempFile\lib\hbase-it-2.3.7.jar;D:\Resources\myTempFile\lib\javax.inject-1.jar;D:\Resources\myTempFile\lib\jettison-1.3.8.jar;D:\Resources\myTempFile\lib\aopalliance-1.0.jar;D:\Resources\myTempFile\lib\commons-cli-1.2.jar;D:\Resources\myTempFile\lib\commons-csv-1.0.jar;D:\Resources\myTempFile\lib\commons-net-3.1.jar;D:\Resources\myTempFile\lib\disruptor-3.4.2.jar;D:\Resources\myTempFile\lib\httpcore-4.4.13.jar;D:\Resources\myTempFile\lib\java-util-1.9.0.jar;D:\Resources\myTempFile\lib\jaxb-api-2.2.12.jar;D:\Resources\myTempFile\lib\jcodings-1.0.18.jar;D:\Resources\myTempFile\lib\stax2-api-3.1.4.jar;D:\Resources\myTempFile\lib\zookeeper-3.5.7.jar;D:\Resources\myTempFile\lib\commons-lang-2.6.jar;D:\Resources\myTempFile\lib\hbase-http-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-rest-2.3.7.jar;D:\Resources\myTempFile\lib\httpclient-4.5.3.jar;D:\Resources\myTempFile\lib\jsr311-api-1.1.1.jar;D:\Resources\myTempFile\lib\libthrift-0.14.1.jar;D:\Resources\myTempFile\lib\commons-io-2.11.0.jar;D:\Resources\myTempFile\lib\commons-lang3-3.9.jar;D:\Resources\myTempFile\lib\guice-servlet-3.0.jar;D:\Resources\myTempFile\lib\hamcrest-core-1.3.jar;D:\Resources\myTempFile\lib\hbase-hbtop-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-shell-2.3.7.jar;D:\Resources\myTempFile\lib\hk2-api-2.5.0-b32.jar;D:\Resources\myTempFile\lib\jaxb-impl-2.2.3-1.jar;D:\Resources\myTempFile\lib\jetty-util-6.1.26.jar;D:\Resources\myTempFile\lib\snappy-java-1.0.5.jar;D:\Resources\myTempFile\lib\api-i18n-1.0.0-M20.jar;D:\Resources\myTempFile\lib\api-util-1.0.0-M20.jar;D:\Resources\myTempFile\lib\checker-qual-3.1.0.jar;D:\Resources\myTempFile\lib\commons-codec-1.13.jar;D:\Resources\myTempFile\lib\hadoop-auth-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-hdfs-2.10.0.jar;D:\Resources\myTempFile\lib\hbase-client-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-common-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-server-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-thrift-2.3.7.jar;D:\Resources\myTempFile\lib\javax.el-3.0.1-b08.jar;D:\Resources\myTempFile\lib\leveldbjni-all-1.8.jar;D:\Resources\myTempFile\lib\metrics-core-3.0.1.jar;D:\Resources\myTempFile\lib\metrics-core-3.2.6.jar;D:\Resources\myTempFile\lib\commons-math3-3.6.1.jar;D:\Resources\myTempFile\lib\hbase-asyncfs-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-logging-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-metrics-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-rsgroup-2.3.7.jar;D:\Resources\myTempFile\lib\hk2-utils-2.5.0-b32.jar;D:\Resources\myTempFile\lib\jackson-core-2.10.1.jar;D:\Resources\myTempFile\lib\jamon-runtime-2.4.1.jar;D:\Resources\myTempFile\lib\java-xmlbuilder-0.4.jar;D:\Resources\myTempFile\lib\javassist-3.20.0-GA.jar;D:\Resources\myTempFile\lib\jersey-guava-2.25.1.jar;D:\Resources\myTempFile\lib\jul-to-slf4j-1.7.30.jar;D:\Resources\myTempFile\lib\protobuf-java-2.5.0.jar;D:\Resources\myTempFile\lib\spymemcached-2.12.2.jar;D:\Resources\myTempFile\lib\woodstox-core-5.0.3.jar;D:\Resources\myTempFile\lib\commons-crypto-1.0.0.jar;D:\Resources\myTempFile\lib\commons-digester-1.8.jar;D:\Resources\myTempFile\lib\curator-client-4.2.0.jar;D:\Resources\myTempFile\lib\hadoop-client-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-common-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-distcp-2.10.0.jar;D:\Resources\myTempFile\lib\hbase-endpoint-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-examples-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-it-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-protocol-2.3.7.jar;D:\Resources\myTempFile\lib\jersey-client-2.25.1.jar;D:\Resources\myTempFile\lib\jersey-common-2.25.1.jar;D:\Resources\myTempFile\lib\jersey-server-2.25.1.jar;D:\Resources\myTempFile\lib\zookeeper-jute-3.5.7.jar;D:\Resources\myTempFile\lib\commons-compress-1.19.jar;D:\Resources\myTempFile\lib\commons-daemon-1.0.13.jar;D:\Resources\myTempFile\lib\curator-recipes-4.2.0.jar;D:\Resources\myTempFile\lib\hbase-mapreduce-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-procedure-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-zookeeper-2.3.7.jar;D:\Resources\myTempFile\lib\HikariCP-java7-2.4.12.jar;D:\Resources\myTempFile\lib\hk2-locator-2.5.0-b32.jar;D:\Resources\myTempFile\lib\javax.ws.rs-api-2.0.1.jar;D:\Resources\myTempFile\lib\jcl-over-slf4j-1.7.30.jar;D:\Resources\myTempFile\lib\mssql-jdbc-6.2.1.jre7.jar;D:\Resources\myTempFile\lib\api-asn1-api-1.0.0-M20.jar;D:\Resources\myTempFile\lib\hadoop-yarn-api-2.10.0.jar;D:\Resources\myTempFile\lib\javax.inject-2.5.0-b32.jar;D:\Resources\myTempFile\lib\jcip-annotations-1.0-1.jar;D:\Resources\myTempFile\lib\jetty-sslengine-6.1.26.jar;D:\Resources\myTempFile\lib\netty-all-4.0.23.Final.jar;D:\Resources\myTempFile\lib\nimbus-jose-jwt-4.41.1.jar;D:\Resources\myTempFile\lib\apacheds-i18n-2.0.0-M15.jar;D:\Resources\myTempFile\lib\curator-framework-4.2.0.jar;D:\Resources\myTempFile\lib\hbase-annotations-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-metrics-api-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-replication-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-shaded-gson-3.3.0.jar;D:\Resources\myTempFile\lib\jackson-core-asl-1.9.13.jar;D:\Resources\myTempFile\lib\jackson-databind-2.10.1.jar;D:\Resources\myTempFile\lib\javax.servlet-api-3.1.0.jar;D:\Resources\myTempFile\lib\javax.servlet.jsp-2.3.2.jar;D:\Resources\myTempFile\lib\hadoop-hdfs-2.10.0-tests.jar;D:\Resources\myTempFile\lib\hbase-common-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-server-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-shaded-netty-3.3.0.jar;D:\Resources\myTempFile\lib\hbase-testing-util-2.3.7.jar;D:\Resources\myTempFile\lib\javax.annotation-api-1.2.jar;D:\Resources\myTempFile\lib\jersey-media-jaxb-2.25.1.jar;D:\Resources\myTempFile\lib\netty-codec-4.1.45.Final.jar;D:\Resources\myTempFile\lib\commons-collections-3.2.2.jar;D:\Resources\myTempFile\lib\commons-configuration-1.6.jar;D:\Resources\myTempFile\lib\hadoop-annotations-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-hdfs-client-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-minicluster-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-client-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-common-2.10.0.jar;D:\Resources\myTempFile\lib\hbase-asyncfs-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-hadoop-compat-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-rsgroup-2.3.7-tests.jar;D:\Resources\myTempFile\lib\jackson-jaxrs-base-2.10.1.jar;D:\Resources\myTempFile\lib\jackson-mapper-asl-1.9.13.jar;D:\Resources\myTempFile\lib\jetty-io-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\netty-buffer-4.1.45.Final.jar;D:\Resources\myTempFile\lib\netty-common-4.1.45.Final.jar;D:\Resources\myTempFile\lib\hbase-hadoop2-compat-2.3.7.jar;D:\Resources\myTempFile\lib\jackson-annotations-2.10.1.jar;D:\Resources\myTempFile\lib\javax.activation-api-1.2.0.jar;D:\Resources\myTempFile\lib\jetty-jmx-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\jetty-xml-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\netty-handler-4.1.45.Final.jar;D:\Resources\myTempFile\lib\validation-api-1.1.0.Final.jar;D:\Resources\myTempFile\lib\hadoop-yarn-registry-2.10.0.jar;D:\Resources\myTempFile\lib\hbase-mapreduce-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-procedure-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-protocol-shaded-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-resource-bundle-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-shaded-protobuf-3.3.0.jar;D:\Resources\myTempFile\lib\hbase-zookeeper-2.3.7-tests.jar;D:\Resources\myTempFile\lib\javax.servlet.jsp-api-2.3.1.jar;D:\Resources\myTempFile\lib\jetty-http-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\jetty-util-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\netty-resolver-4.1.45.Final.jar;D:\Resources\myTempFile\lib\osgi-resource-locator-1.0.1.jar;D:\Resources\myTempFile\lib\netty-transport-4.1.45.Final.jar;D:\Resources\myTempFile\lib\error_prone_annotations-2.3.4.jar;D:\Resources\myTempFile\lib\hbase-annotations-2.3.7-tests.jar;D:\Resources\myTempFile\lib\jetty-server-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\jetty-webapp-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\jetty-servlet-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\hbase-external-blockcache-2.3.7.jar;D:\Resources\myTempFile\lib\hbase-hadoop-compat-2.3.7-tests.jar;D:\Resources\myTempFile\lib\jetty-security-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\aopalliance-repackaged-2.5.0-b32.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-common-2.10.0.jar;D:\Resources\myTempFile\lib\hbase-hadoop2-compat-2.3.7-tests.jar;D:\Resources\myTempFile\lib\hbase-shaded-miscellaneous-3.3.0.jar;D:\Resources\myTempFile\lib\jetty-util-ajax-9.3.28.v20191105.jar;D:\Resources\myTempFile\lib\apacheds-kerberos-codec-2.0.0-M15.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-hs-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-app-2.10.0.jar;D:\Resources\myTempFile\lib\jackson-jaxrs-json-provider-2.10.1.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-core-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-web-proxy-2.10.0.jar;D:\Resources\myTempFile\lib\geronimo-jcache_1.0_spec-1.0-alpha-1.jar;D:\Resources\myTempFile\lib\jersey-container-servlet-core-2.25.1.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-common-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-nodemanager-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-tests-2.10.0-tests.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-shuffle-2.10.0.jar;D:\Resources\myTempFile\lib\jackson-module-jaxb-annotations-2.10.1.jar;D:\Resources\myTempFile\lib\hadoop-mapreduce-client-jobclient-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-resourcemanager-2.10.0.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-timelineservice-2.10.0.jar;D:\Resources\myTempFile\lib\netty-transport-native-epoll-4.1.45.Final.jar;D:\Resources\myTempFile\lib\netty-transport-native-unix-common-4.1.45.Final.jar;D:\Resources\myTempFile\lib\hadoop-yarn-server-applicationhistoryservice-2.10.0.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\log4j-1.2.17.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\slf4j-api-1.7.30.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\commons-logging-1.2.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\slf4j-log4j12-1.7.30.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\audience-annotations-0.5.0.jar;D:\Resources\myTempFile\lib\client-facing-thirdparty\htrace-core4-4.2.0-incubating.jar;C:\Program Files\JetBrains\IntelliJ IDEA 2021.2\lib\idea_rt.jar
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.library.path=C:\Program Files\Java\jdk1.8.0_301\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;C:\Program Files (x86)\Common Files\Oracle\Java\javapath;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Windows\System32\OpenSSH\;C:\Tools\mysql-8.0.26-winx64\bin;C:\Program Files\Java\jdk1.8.0_301\bin;C:\Program Files\Java\jdk1.8.0_301\jre\bin;C:\Program Files\dotnet\;C:\Program Files\JetBrains\IntelliJ IDEA 2021.2\plugins\maven\lib\maven3\bin;C:\Program Files\PuTTY\;C:\Program Files (x86)\NetSarang\Xshell 7\;C:\Program Files (x86)\NetSarang\Xftp 7\;C:\Program Files\Git\cmd;C:\Program Files\nodejs\;C:\Users\15831\AppData\Local\Programs\Python\Python39;C:\Users\15831\AppData\Local\Microsoft\WindowsApps;C:\Users\15831\AppData\Local\GitHubDesktop\bin;C:\Users\15831\AppData\Local\Programs\Microsoft VS Code\bin;C:\Users\15831\AppData\Roaming\npm;.
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.io.tmpdir=C:\Users\15831\AppData\Local\Temp\
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:java.compiler=<NA>
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.name=Windows 10
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.arch=amd64
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.version=10.0
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:user.name=15831
2021-12-06 00:18:21,296 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:user.home=C:\Users\15831
2021-12-06 00:18:21,297 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:user.dir=D:\CodeProjects\GitHub\BUPT_private\PGcoursework\bigdata-appli
2021-12-06 00:18:21,297 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.memory.free=197MB
2021-12-06 00:18:21,297 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.memory.max=3156MB
2021-12-06 00:18:21,297 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (Environment.java:logEnv(109)) - Client environment:os.memory.total=213MB
2021-12-06 00:18:21,299 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ZooKeeper (ZooKeeper.java:<init>(868)) - Initiating client connection, connectString=zhangyun:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.ReadOnlyZKClient$$Lambda$8/711977689@1531689c
2021-12-06 00:18:21,303 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] common.X509Util (X509Util.java:<clinit>(79)) - Setting -D jdk.tls.rejectClientInitiatedRenegotiation=true to disable client-initiated TLS renegotiation
2021-12-06 00:18:21,490 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ClientCnxnSocket (ClientCnxnSocket.java:initProperties(237)) - jute.maxbuffer value is 4194304 Bytes
2021-12-06 00:18:21,496 INFO  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ClientCnxn (ClientCnxn.java:initRequestTimeout(1653)) - zookeeper.request.timeout value is 0. feature enabled=
2021-12-06 00:18:23,771 ERROR [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d-SendThread()] client.StaticHostProvider (StaticHostProvider.java:resolve(154)) - Unable to resolve address: zhangyun:2181
java.net.UnknownHostException: zhangyun
	at java.net.Inet6AddressImpl.lookupAllHostAddr(Native Method)
	at java.net.InetAddress$2.lookupAllHostAddr(InetAddress.java:929)
	at java.net.InetAddress.getAddressesFromNameService(InetAddress.java:1324)
	at java.net.InetAddress.getAllByName0(InetAddress.java:1277)
	at java.net.InetAddress.getAllByName(InetAddress.java:1193)
	at java.net.InetAddress.getAllByName(InetAddress.java:1127)
	at org.apache.zookeeper.client.StaticHostProvider$1.getAllByName(StaticHostProvider.java:92)
	at org.apache.zookeeper.client.StaticHostProvider.resolve(StaticHostProvider.java:147)
	at org.apache.zookeeper.client.StaticHostProvider.next(StaticHostProvider.java:375)
	at org.apache.zookeeper.ClientCnxn$SendThread.run(ClientCnxn.java:1137)
2021-12-06 00:18:23,775 WARN  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d-SendThread(zhangyun:2181)] zookeeper.ClientCnxn (ClientCnxn.java:run(1246)) - Session 0x0 for server zhangyun:2181, unexpected error, closing socket connection and attempting reconnect
java.lang.IllegalArgumentException: Unable to canonicalize address zhangyun:2181 because it's not resolvable
	at org.apache.zookeeper.SaslServerPrincipal.getServerPrincipal(SaslServerPrincipal.java:71)
	at org.apache.zookeeper.SaslServerPrincipal.getServerPrincipal(SaslServerPrincipal.java:39)
	at org.apache.zookeeper.ClientCnxn$SendThread.startConnect(ClientCnxn.java:1087)
	at org.apache.zookeeper.ClientCnxn$SendThread.run(ClientCnxn.java:1139)
2021-12-06 00:18:23,881 WARN  [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d] zookeeper.ReadOnlyZKClient (ReadOnlyZKClient.java:exec(192)) - 0x3c9d0b9d to zhangyun:2181 failed for get of /hbase/hbaseid, code = CONNECTIONLOSS, retries = 1
2021-12-06 00:18:24,893 ERROR [ReadOnlyZKClient-zhangyun:2181@0x3c9d0b9d-SendThread(zhangyun:2181)] client.StaticHostProvider (StaticHostProvider.java:resolve(154)) - Unable to resolve address: zhangyun:2181
java.net.UnknownHostException: zhangyun
```

#### debug

> 先处理问题：HADOOP_HOME and hadoop.home.dir are unset。

- 两个参考连接：[1](https://www.freesion.com/article/5997398294/),[2](https://blog.csdn.net/u013305864/article/details/97191344)

![image-20211206003026249](hbase.assets/image-20211206003026249.png)

![image-20211206003552282](hbase.assets/image-20211206003552282.png)

问题被解决！！！

> 再处理问题`java.lang.IllegalArgumentException: Unable to canonicalize address zhangyun:2181 because it's not resolvable`

1. 先根据[文章](https://blog.csdn.net/bbwangj/article/details/74502967)查看两种防火墙是否都关闭，确实都是关闭的，应该不是防火墙的锅

2. 放开腾讯云的2181（zk）端口，还是不行

3. 参考[文章](https://stackoverflow.com/questions/68237419/unable-to-canonicalize-address-localhost-unresolved2222-because-its-not-reso)，可能是我们的zk没启动，联想到之前没有设置HBASE_MANAGES_ZK，放开后，关闭hbase，关闭hadoop，开启hadoop，开启habse重试。还是不行。

4. [参考1](https://blog.csdn.net/JgyXyyxy/article/details/76618380),[参考2](https://blog.csdn.net/sunjiangangok/article/details/80501859)会不会是因为我hadoop配置一直用zhangyun（hostname），而不是公网ip，导致本机程序无法连接远程服务器？

   干脆看看网上教程，搜索“hbase伪分布式 javaapi建hbase表”
   
   - 比如访问（http://120.53.244.17:8088/cluster）可以看到节点。访问（http://120.53.244.17:9000/）会提示错误。

5. 怀疑是因为hbase中没有配置zk节点名，参考使用hbase自带zk的[文章](https://www.cxyzjd.com/article/weixin_42369418/99440946)配置一下

   ![image-20211207205812574](hbase.assets/image-20211207205812574.png)

   重启hbase

   ```
   cd /usr/local/hbase-2.3.7
   
   bin/start-hbase.sh
   
   bin/stop-hbase.sh
   ```

   ![image-20211207210032782](hbase.assets/image-20211207210032782.png)

   发现：如果使用hbase自带的zk的话，不用配置quorum，改回来：

   ```
   vim conf/hbase-site.xml
   ```

   ![image-20211207215802435](hbase.assets/image-20211207215802435.png)

   6. 课件提示datadir,quorum,要和hbase-site.xml一致，那么添加如下内容，和课件保持一致：

      - 注意hbase.zookeeper.quorum的值，因为使用的hbase自带的zk，必须是ssh能认识的节点名，比如`localhost`

      ```
      <property>
        <name>hbase.zookeeper.property.dataDir</name>
        <value>/root/zhangyunroot/hbase/zk</value>
      </property>
      <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zknode1</value>
      </property>
      
      ```

      ![image-20211207222124777](hbase.assets/image-20211207222124777.png)

      重新尝试还是不行

   7. 根据控制台报错，搜索`hbase Opening socket connection to server localhost/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)`找到一篇[文章](https://stackoverflow.com/questions/26522513/hbase-connection-about-zookeeper-error)，也是用hbase自带zk。

      Add follwing properties in hbase-site.xml file:

      ```
       <property>
       <name>hbase.zookeeper.property.clientPort</name>
       <value>2181</value>
       </property>
      ```

      ![image-20211207223247503](hbase.assets/image-20211207223247503.png)

      restart ./start-hbase.sh:

      还是不行,,,

      ```
      2021-12-07 22:36:50,202 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ZooKeeper (ZooKeeper.java:<init>(868)) - Initiating client connection, connectString=localhost:2181 sessionTimeout=90000 watcher=org.apache.hadoop.hbase.zookeeper.ReadOnlyZKClient$$Lambda$8/1681655132@3eb99c05
      2021-12-07 22:36:50,207 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] common.X509Util (X509Util.java:<clinit>(79)) - Setting -D jdk.tls.rejectClientInitiatedRenegotiation=true to disable client-initiated TLS renegotiation
      2021-12-07 22:36:50,400 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ClientCnxnSocket (ClientCnxnSocket.java:initProperties(237)) - jute.maxbuffer value is 4194304 Bytes
      2021-12-07 22:36:50,404 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ClientCnxn (ClientCnxn.java:initRequestTimeout(1653)) - zookeeper.request.timeout value is 0. feature enabled=
      2021-12-07 22:36:50,411 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1112)) - Opening socket connection to server localhost/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
      2021-12-07 22:36:52,439 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:run(1244)) - Socket error occurred: localhost/127.0.0.1:2181: Connection refused: no further information
      2021-12-07 22:36:52,551 WARN  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ReadOnlyZKClient (ReadOnlyZKClient.java:exec(192)) - 0x6ce139a4 to localhost:2181 failed for get of /hbase/hbaseid, code = CONNECTIONLOSS, retries = 1
      2021-12-07 22:36:53,562 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1112)) - Opening socket connection to server localhost/0:0:0:0:0:0:0:1:2181. Will not attempt to authenticate using SASL (unknown error)
      2021-12-07 22:36:55,577 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:run(1244)) - Socket error occurred: localhost/0:0:0:0:0:0:0:1:2181: Connection refused: no further information
      2021-12-07 22:36:55,686 WARN  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ReadOnlyZKClient (ReadOnlyZKClient.java:exec(192)) - 0x6ce139a4 to localhost:2181 failed for get of /hbase/hbaseid, code = CONNECTIONLOSS, retries = 2
      2021-12-07 22:36:56,692 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:logStartConnect(1112)) - Opening socket connection to server localhost/0:0:0:0:0:0:0:1:2181. Will not attempt to authenticate using SASL (unknown error)
      2021-12-07 22:36:58,724 INFO  [ReadOnlyZKClient-localhost:2181@0x6ce139a4-SendThread(localhost:2181)] zookeeper.ClientCnxn (ClientCnxn.java:run(1244)) - Socket error occurred: localhost/0:0:0:0:0:0:0:1:2181: Connection refused: no further information
      2021-12-07 22:36:58,835 WARN  [ReadOnlyZKClient-localhost:2181@0x6ce139a4] zookeeper.ReadOnlyZKClient (ReadOnlyZKClient.java:exec(192)) - 0x6ce139a4 to localhost:2181 failed for get of /hbase/hbaseid, code = CONNECTIONLOSS, retries = 3
      ```

   8. 建议搜索。。。hbase伪分布式 javaapi

   

