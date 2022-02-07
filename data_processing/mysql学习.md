# 安装MYSQL

1. [首先参考bilibili](https://www.bilibili.com/video/BV1EJ411p7Ty?from=search&seid=4187360526160968756)
   
   - 下载时会先出现的界面是zip archive。本来msi是自动配置，而zip要自己配置，但是跟着教程走吧。
   - 中间输入mysqld --initialize-insecure --user=mysql时报错，找到2.的帖子
   
2. [转而参考csdn](https://blog.csdn.net/weixin_37008013/article/details/104552145)

   - 该帖子在设置path时多加了 %SystemRoot%\system32，但是我看我的path里本来就有就没加
   - 按照安装“微软常用运行库合集后”，mysqld --initialize-insecure --user=mysql不报错了。

   - 但是该帖子文字部分不准确，要按照截图中的代码输命令

   - 最后修改密码时，输入指令为 

     ```
     alter user user() identified by '123456';
     ```

     原贴by和引号（英文的单引号和双引号都可以）贴在一起，而且不加分号，导致输入命令后sql没反应。

     [附带参考网址](https://www.hxstrive.com/article/728.htm)，解释一些有关用户密码的操作。

3. 回到bilibili视频得对应位置
   - 注意
     - 在命令行输入mysql命令时，比如```show databases;```__要加上英文引号__
     - 每一行在等待命令的状态时，都应以 mysql> 开头
     
   - 使用**完**数据库后的步骤，**一定要做**：
   
     ```
     quit
     
     net stop mysql
     ```
   
     才能终止mysql服务
   
   - 与终止对应的，**开启**mysql的方式。本地mysql connection id是8
   
     ``` 
         桌面 cmd（一定要管理员身份） 
     	net start mysql
     	mysql -u root -p
     	输入密码:123456
     ```
   
     其中root代表本地数据库的默认名字   

---

# 安装navicate

1. 先找到[b站资源](https://www.bilibili.com/video/BV17E411C7au?p=4)

2. 根据评论栏中一前辈的评论开始破解：

   *1.解压CnOaMvEiTcFaEtI12.zip，密码为CometFei
   并安装解压文件.exe
   2.解压破Navicat.zip，然后管理员运行
   点击右上角Patch, 找到1.中你Navicate安装目录(注意不是破解文件的目录)下.exe文件，界面出现cracked则成功完成第2步(注意不要关闭crack界面
   3.断网，然后在系统搜Navicat并运行，进入界面后找上方菜单栏帮助注册
   4.再打开crack界面，点击Serial Keygen所在行最右边的Generate按钮，生成注册码
   5.复制注册码到Navicate注册码界面，激活失败后选手动激活，复制请求码
   6.回到crack界面，在Request code一栏粘贴，再点击Activation Code一栏下的Generate按钮，生成激活码并复制到Navicate激活界面，最终显示出永久许可证*

   - 走到第六步时报错：“rsa public not find”，这是因为评论也说了：注册机应该**先**patch完，再__断网__打开navicate。而不是先打开navicate再patch。所以要卸载navicate重新装。
     - [卸载教程](https://www.php.cn/tool/navicat/428286.html)
     - 同时注意把安装了navicate的文件夹（D:\software\navicate\Navicat Premium 12）也删了

   - 卸载之后，严格按照上述评论执行，即破解成功

---

# navicate连接本地数据库

老版本破解20210920用的时候失败了：

1. 开启mysql
2. [开启mysql后在navicate的操作](https://blog.csdn.net/qq_40139254/article/details/94402800)
   - 注意，如果不先在cmd开启mysql就尝试用navicate连接mysql数据库，navicate中输入root 和密码后会提示“无法连接”
3. 用完之后记得先断Navicat，再关闭mysql
4. 备注
   - mysql数据库是基于“客户机-服务器”的数据库，本机安装的mysql相当于服务器，本机安装的navicate相当于客户机。只是本地开发时，两者在同一台电脑上，实际开发中，常常要连接远程的数据库。（知识在mysql必知p8）

新版本破解20210920用的时候成功了：

​	https://defcon.cn/214.html



注意：

本地控制台先net start mysql，navicat才能连接

---

# mysql知识

- 索引和键的区别：[链接](https://zhidao.baidu.com/question/1644206194209291020.html)

  我的理解：

  - 键是约束性的，保证一列中的值都是唯一的
  - 键通常是用作索引的好选择
  - 索引是一个类似hash键的东西，所以可以加快查询速度，只查一次就能定位到目标行。 没有索引而用主键查找的话，数据库会一行一行得比对主键，会查蛮久。 

- 修改视图

  虽然视图一般用于简化和复用select的代码，但是delete，insert等修改也是可以应用于视图的。但是因为视图本身只是sql，所以针对视图的更改实际上是作用于基本表（基表）上。

  同时，更改视图要满足一些条件，见链接。

  [参考链接](http://c.biancheng.net/view/2586.html)



## 推荐网课

面向面试的话，看MYSQL45讲