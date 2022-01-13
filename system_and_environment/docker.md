

# Docker



## Docker学习大纲

学前准备：

- linux：必须会简单的命令操作，比如cd，mkdir。
- springboot：会的话更好，不会也行。



学什么：

1. docker概述

2. docker安装

3. docker命令

   - 镜像命令

   - 容器命令
   - 操作命令
   - 。。。

4. docker镜像

5. 容器数据卷

6. dockerFile

7. Docker网络原理

8. IDEA整合Docker

9. Docker Compose（集群）

10. Docker Swam（集群管理）
    - 相当于简单版的k8s
11. CI\CD Jenkins

1-8算初级，先学着入门。



## Docker前述

### Docker为什么出现

一套产品有开发/上线，两套环境！应用环境，应用配置在两个环境中都要处理。



一个公司有开发和运维两个主要角色。两角色会碰到一些问题：

- 我在我的电脑上可以用，在你那不行。
- 版本更新，导致服务不可用。这对于运维来说考验很大。

解决方案：

- 开发也干运维的活。



环境配置是十分麻烦的，每个机器都要部署环境（集群redis，ES，Hadoop。。）。费时费力。

解决问题：

- 发布一个项目时，之前是发布一个jar包或者war包。现在发布jar包，它可能需要各种环境（redis，mysql，jdk，ES）；发布jar包是一分钟的事，但是部署环境可能一天都部署不好。
- 我们就想：发布一个项目时，项目能不能带上开发时的环境安装打包，即：（jar+（redis，mysql，jdk，ES））。docker就是来干这个的，开发人员开发完毕后，直接用docker部署上线。



拿安卓开发类比docker：

- java项目--制作apk--发布apk到应用商店--李四使用apk--下载安装即用。
- java项目--制作了jar包（和环境）--打包项目带上环境（即镜像）--docker仓库（类似应用商店）--下载我们发布的镜像，直接运行即可。



Docker思想：

- Docker的思想来源于集装箱。
- 没有docker时，多个应用可能会有端口冲突，应用都是交叉的。
- docker的核心思想是“隔离”。打包装箱，每个箱子是相互隔离的。
  - 比如之前船上装“生化武器”和“食品”可能导致食品变质。但是docker把“生化武器”和“食品”分别装箱后，两者互不影响，可以放到一艘轮船上。
- Docker通过隔离机制，可以将服务器的性能用到极致。不会浪费。



### Docker的历史

2010年，几个搞it的年轻人在美国成立了`dotcloud`公司，做paas云计算服务。有一些linux相关的容器技术。他们将自己的技术（容器化技术）称为docker。

docker刚开始的时候没有引起行业注意，dotcloud活不下去，年轻人又不想放弃这个项目，2013年就把项目做了开源。（有的公司一开始就开源，有的是活不下去了才开源）。越来越多的人发现了docker的优点。

2014年4月9日，docker1.0发布。并成为如今开发人员必须会的技术之一。



docker为何这么火？：

- 十分的轻巧
  - 在容器技术出来之前，我们用的都是虚拟机技术。虚拟机特别占内存，普通电脑开不了几个。
  - 虚拟机：在windows中装一个vmware虚拟机，通过这个软件我们可以虚拟出来一台或者多台电脑。虚拟出来的电脑和真实电脑很想，也是很笨重的。虚拟机是属于虚拟化技术， docker是容器化技术也属于一种虚拟化技术。
  - 例子
    - vm（虚拟机）：linux centos原生镜像（类似一电脑），有隔离性，占用几个G的硬盘，开启要几分钟。
    - docker：有隔离性，镜像（只包含最核心的环境如jdk，mysql），运行镜像就可以，占用几M的硬盘，秒级开启。



聊聊docker：

- docker是基于go语言开发的，开源项目。
- 官网地址：https://www.docker.com/
- 官网文档地址：https://docs.docker.com/
- docker仓库地址：https://hub.docker.com/
  - 操作命令类似github。



### Docker能干什么

#### 虚拟机技术

电脑/虚拟机技术的功能示意图：

![image-20211017155406756](docker.assets/image-20211017155406756.png)

虚拟机技术的缺点：

- 资源占用很多
- 冗余步骤很多
- 启动很慢



#### 容器化技术

**容器化技术不是模拟的一个完整地操作系统**



容器化技术示意图：

![image-20211017160252265](docker.assets/image-20211017160252265.png)



比较docker和虚拟机技术的不同：

- 传统虚拟机虚拟出一套硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件。
- 容器内的应用直接运行在“宿主机”的内容中，容器是没有自己的内核，也没有虚拟硬件，所以就轻便了。
- 每个容器内是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响。



#### DevOps

Dev（开发）Ops（运维）=升职加薪的秘诀



Docker在DevOps角度的优点：

- docker可以实现更快速的交付和部署

  - 传统：一堆帮助文档，安装程序

  - Docker：打包镜像，发布测试，一键运行。

- 更便捷的升级和扩容
  - 使用了docker后，我们部署应用就像搭积木一样。
  - 项目打包为一个影响，一键就能扩展。
  - 各种环境的版本管理升级更便捷。

- 更简单的系统运维
  - 在容器化后，我们的开发，测试环境都是高度一致的；不会出现在A电脑上能用，在B电脑上不能用的情况。

- 更高校的计算资源利用
  - 1核2g的服务器，用docker后，可以运行几十个redis；这是之前虚拟机做不到的。
  - docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例。服务器的性能可以被压榨到极致。



## Docker安装与卸载

### Docker的基本组成

docker架构图：

![See the source image](docker.assets/dock_arch.jpg)

- 三大块：
  - client=客户端
  - host=服务器
  - registry=仓库

- 三个关注点：
  - 镜像（image）
    - docker镜像好比是一个模板，可以通过这个模板来创建容器服务。镜像理解为java中的类，容器理解为java中的对象。
    - 比如有tomcat镜像，镜像是不能直接启动的，要把镜像“运行”起来才能“启动”。tomcat镜像-->run-->tomcat01容器(提供服务)。通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。
  - 容器（container）
    -  docker利用容器技术可以独立运行一个或者一组应用，应用是通过镜像来创建的。
    - 基本命令：启动，停止，删除。。
    - 目前可以把容器理解为一个简易的linux系统。
  - 仓库（repository）
    - 仓库就是存放镜像的地方。
    - 仓库分为“共有仓库”和“私有仓库”。
    - 像阿里云等公司都有容器服务器，一般要配置国内镜像加速，不然很慢（如maven镜像）。



### 安装Docker准备

#### 环境准备

1. 需要会一点点的linux基础
2. 一个远程服务器（推荐阿里云）
3. CentOS 7（远程服务器系统）
4. 我们用XShell连接远程服务器进行操作

#### 环境查看

shell连接远程服务器，输入一下命令进行查看

```bash
# 系统内核是3.10以上的
[root@zhangyun/]# uname -r
3.10.0-XXX
```

```bash
# 系统版本
[root@rootuser ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

### 使用仓库安装实战

#### 访问官网

访问[官网](https://docs.docker.com/get-docker/)

![image-20220109223415483](docker.assets/image-20220109223415483.png)

![image-20220109223528806](docker.assets/image-20220109223528806.png)

#### 卸载旧版本

在任意目录下运行下列语句：

```bash
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

![image-20220109223853201](docker.assets/image-20220109223853201.png)

#### 安装一些有用的包

安装一些有用的包

```bash
yum install -y yum-utils
```

![image-20220109224133256](docker.assets/image-20220109224133256.png)

#### 设置镜像的仓库

默认是国外的仓库，很慢，**不要用**：

```
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

应该使用阿里云docker仓库地址：

```
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

- 这镜像指的是linux应用镜像仓库

![image-20220109225054378](docker.assets/image-20220109225054378.png)



#### 更新yum索引

更新软件包的索引：

```bash
yum makecache fast
```

![image-20220109230344662](docker.assets/image-20220109230344662.png)

#### 安装docker引擎

```bash
# docker-ce社区版，ee是企业版，推荐ce
yum install docker-ce docker-ce-cli containerd.io
```

![image-20220109230517430](docker.assets/image-20220109230517430.png)



#### 启动docker

启动：

```bash
systemctl start docker
```

判断docker是否安装成功

```bash
docker version
```

![判断](docker.assets/image-20220109230641420.png)



#### 测试helloworld

运行helloworld镜像：

```bash
docker run hello-world
```

![image-20220109231112905](docker.assets/image-20220109231112905.png)



#### 查看下载的镜像

```bash
[root@rootuser ~]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
docker72590/alpine   latest    f5a69fceabd2   9 days ago     5.59MB
# 成功发现了刚刚下载的helloworld镜像
hello-world          latest    feb5d9fea6a5   3 months ago   13.3kB
```



### docker卸载

卸载依赖：

```bash
yum remove docker-ce docker-ce-cli containerd.io
```

删除docker的运行环境

```bash
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

- `/var/lib/docker`是docker的默认工作路径。



### 配置阿里云镜像加速

我用的腾讯云服务器，用的阿里云镜像。只要有阿里云账号，就可以用阿里云镜像加速，和服务器没关系。



登录阿里云，找到"容器镜像服务"：

![image-20220110105149681](docker.assets/image-20220110105149681.png)

找到镜像加速的[地址](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors):

![image-20220110105402483](docker.assets/image-20220110105402483.png)

依次执行命令：

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://eno5zxt9.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```

- 如果不是root用户，命令前需要加`sudo`

![image-20220110110757305](docker.assets/image-20220110110757305.png)

使用`docker info`来查看是否设置成功，确实看到了自己配置的镜像:

![image-20220110111015750](docker.assets/image-20220110111015750.png)



## Docker底层原理

### 回顾helloworld流程

run hello-world镜像，测试docker时命令行展示如下：

![image-20220109231112905](docker.assets/image-20220109231112905.png)

根据命令行截图，就可以理解docker run后的运行流程图：

![image-20220110111340270](docker.assets/image-20220110111340270.png)

### 原理

#### docker是怎么工作的

Docker是一个Client - Server结构的系统。Docker的”守护进程“运行在主机上，客户端通过Socket可以访问Docker的”守护进程“。DockerServer接收到Docker-Client的指令，就会执行这个命令。

docker的c-s架构工作思路： 

![image-20220110114037554](docker.assets/image-20220110114037554.png)



#### docker为什么比vm快

1，从逻辑方面考虑

对比图示：

![image-20220110114652844](docker.assets/image-20220110114652844.png)

原因：

- docker有比虚拟机更少的抽象层

- docker利用的是宿主机的内核，vm需要Guest OS

所以说，新建一个容器的时候，docker不需要像虚拟机一样重新加载一个操作系统内核，避免引导。虚拟机是加载Guest OS，分钟级别的；而docker是利用宿主机的操作系统，省略了“加载Guest OS”这个复杂的过程，秒级!



2，性能方向对比

![image-20220110115154697](docker.assets/image-20220110115154697.png)



## Docker命令

### 镜像基本命令

#### 帮助命令

```bash
# 查看docker版本
docker version
# 查看docker的详细系统信息，包括镜像+容器
docker info
# 查看不懂的命令的用法
docker 不懂的命令 --help
```

帮助文档的[网址](https://docs.docker.com/reference/):

![image-20220110120515064](docker.assets/image-20220110120515064.png)



### 镜像命令

#### 查看本地镜像

[docker images](https://docs.docker.com/engine/reference/commandline/images/)查看所有本地的主机上的镜像

实战：

```bash
[root@rootuser ~]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
docker72590/alpine   latest    f5a69fceabd2   9 days ago     5.59MB
hello-world          latest    feb5d9fea6a5   3 months ago   13.3kB
You have new mail in /var/spool/mail/root
```

栏讲解：

- REPOSITORY：镜像的仓库源。根据这一栏的信息从镜像仓库下载镜像。
- TAG：镜像的标签
- IMAGE ID：镜像的id
- created：镜像的创建时间
- size：镜像的大小

常用可选项讲解：

- -a：all，列出所有镜像
- -q：quiet，只显示镜像的id

- -aq：经常-a和-q联合使用，`docker -aq`，展示所有镜像的id。



#### 搜索线上dockerhub镜像

可以在[docker hub](https://hub.docker.com/)上搜索镜像：

![image-20220110144529025](docker.assets/image-20220110144529025.png)

但是官网在境外有的时候访问不稳定。

---

也可以在linux命令行中使用命令`docker search 搜索的东西`来搜索镜像：

```bash
[root@rootuser ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11940     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4562      [OK]       
```

可选项：

- `--filter=STARS=3000`可以搜索出stars大于3000的镜像。

使用filter后得到的部分结果：

```bash
[root@rootuser ~]# docker search mysql --filter=stars=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11940     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4562      [OK]     
```



#### 下载镜像

直接下载最新版,以下载mysql镜像为例：

```bash
[root@rootuser ~]# docker pull mysql
# 如果不写tag，默认下载latest版本
Using default tag: latest
latest: Pulling from library/mysql
# 分层下载，docker image的核心！联合文件系统。
72a69066d2fe: Pull complete 
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
# 防伪标志：签名
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Status: Downloaded newer image for mysql:latest
# 真实地址
docker.io/library/mysql:latest
```

说明：

- `docker pull mysql`和`docker pull docker.io/library/mysql:latest`等价

  ，因为docker.io/library/mysql:latest是真实地址。

---

指定镜像的版本下载

```bash
[root@rootuser ~]# docker pull mysql:5.7
# 下载指定的5.7版本
5.7: Pulling from library/mysql
# 已经下载的层不会下载，从already exit可以看出。使用了linux的”联合文件系统“功能
72a69066d2fe: Already exists 
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
# 没下载过的层才会下载
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
You have new mail in /var/spool/mail/root
```

- 指定的版本一定要在docker hub存在，可以通过docker hub网站确认。
- 下载镜像是分层的，有的层已经被下载过的话不会被重复下载。其实每个层也是小镜像。

下载完毕后查看本地的所有镜像：

![image-20220110151223009](docker.assets/image-20220110151223009.png)



#### 删除镜像

rm表示删除，i表示image即镜像。

根据镜像id删除镜像：

```bash
[root@rootuser ~]# docker rmi 3218b38490ce
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
# 只会删除独有的层，和其他镜像共有的层不会被删除。
Deleted: sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b
Deleted: sha256:aa81ca46575069829fe1b3c654d9e8feb43b4373932159fe2cad1ac13524a2f5
Deleted: sha256:0558823b9fbe967ea6d7174999be3cc9250b3423036370dc1a6888168cbd224d
Deleted: sha256:a46013db1d31231a0e1bac7eeda5ad4786dea0b1773927b45f92ea352a6d7ff9
Deleted: sha256:af161a47bb22852e9e3caf39f1dcd590b64bb8fae54315f9c2e7dc35b025e4e3
Deleted: sha256:feff1495e6982a7e91edc59b96ea74fd80e03674d92c7ec8a502b417268822ff
```

查看本地镜像，发现确实id为3218b38490ce的镜像被删除了：

![image-20220110151908903](docker.assets/image-20220110151908903.png)

---

使用参数来删除

- -f 表示force，就强制删除
- $()表示给docker命令传参。这里使用$(docker images -aq)相当于把所有docker镜像的id都传给docker rmi，即**删除所有镜像**。有套娃那味。

```bash
[root@rootuser ~]# docker rmi -f $(docker images -aq)
Untagged: mysql:5.7
Untagged: mysql@sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Deleted: sha256:c20987f18b130f9d144c9828df630417e2a9523148930dc3963e9d0dab302a76
Deleted: sha256:6567396b065ee734fb2dbb80c8923324a778426dfd01969f091f1ab2d52c7989
Deleted: sha256:0910f12649d514b471f1583a16f672ab67e3d29d9833a15dc2df50dd5536e40f
Deleted: sha256:6682af2fb40555c448b84711c7302d0f86fc716bbe9c7dc7dbd739ef9d757150
Deleted: sha256:5c062c3ac20f576d24454e74781511a5f96739f289edaadf2de934d06e910b92
Deleted: sha256:8805862fcb6ef9deb32d4218e9e6377f35fb351a8be7abafdf1da358b2b287ba
Deleted: sha256:872d2f24c4c64a6795e86958fde075a273c35c82815f0a5025cce41edfef50c7
Deleted: sha256:6fdb3143b79e1be7181d32748dd9d4a845056dfe16ee4c827410e0edef5ad3da
Deleted: sha256:b0527c827c82a8f8f37f706fcb86c420819bb7d707a8de7b664b9ca491c96838
Deleted: sha256:75147f61f29796d6528486d8b1f9fb5d122709ea35620f8ffcea0e0ad2ab0cd0
Deleted: sha256:2938c71ddf01643685879bf182b626f0a53b1356138ef73c40496182e84548aa
Deleted: sha256:ad6b69b549193f81b039a1d478bc896f6e460c77c1849a4374ab95f9a3d2cea2
Untagged: hello-world:latest
Untagged: hello-world@sha256:2498fce14358aa50ead0cc6c19990fc6ff866ce72aeb5546e1d59caac3d0d60f
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
Error: No such image: f5a69fceabd2
```

查看删除结果：

![image-20220110152602024](docker.assets/image-20220110152602024.png)

- `docker72590/alpine` 删不掉，貌似是病毒，，，我都是官方渠道，，怎么这样。先不管了。



### 容器命令

说明∶我们有了镜像才可以创建容器，linux中下载一个centos镜像来测试学习

```bash
[root@rootuser ~]# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
You have new mail in /var/spool/mail/root

```

- 这里相当于在centos中下载centos的docker镜像，属于套娃了。

#### 新建容器并启动

新建容器并启动：

```bash
docker run [可选参数] imageid
```

参数说明：

- --name="name":是容器的名字，比如可以为”tomcat1“或”tomcat2“来区分不同的tomcat容器。

- -d: 后台方式运行
- -it：使用交互方式运行，进入容器查看内容
- -p：指定容器的端口，如`-p 8080`
  - `-p 主机端口:容器端口`：可以从外界访问。最常用！
  - `-p ip:主机端口:容器端口`：可以带上ip。
  - `-p 容器端口`：不从外界访问。
- -P：大写P，即随机指定端口

---

实战：

```bash
[root@rootuser ~]# docker run -it centos /bin/bash
# 通过ls查看容器内部的centos的目录，可以看到和容器外部的centos很相似，但是两者没有任何关系。
[root@29e9814941c2 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
# 退出centos容器
[root@29e9814941c2 /]# exit
exit
# 查看容器外部的centos根目录，与容器内部的目录很相似。
[root@rootuser ~]# ls /
bin   data  etc   lib    lost+found  mnt  proc  run   srv  tmp  var
boot  dev   home  lib64  media       opt  root  sbin  sys  usr

```

- -it表示以交互方式运行容器，交互方式被指定为"/bin/bash"
- /bin/bash表示通过bash命令的方式进入容器

#### 退出容器

```bash
# 容器停止并退出
exit
# 容器不停止，且退出
ctrl+P（大写）+Q(大写)
```

#### 查看运行（过）的容器

```bash
# 正在运行的容器
docker ps
# 正在运行+未运作的容器（被删除的容器不会显示），a表示all
docker ps -a
# 显示正在运行的最新的若干个容器，如下显示最新的一个容器
docker ps -n=1
# 显示正在运行的容器，只显示容器的编号
docker ps -p

```

#### 删除容器

```bash
# 删除指定的单个容器。没使用-f不能删除正在运行的容器。
docker rm 容器id
# 强制删除所有容器,$(docker ps -aq)把所有容器的id传给rm。用了-f可以删除正在运行的容器。
docker rm -f $(docker ps -aq)
# 删除所有容器。使用了管道符"|"
docker ps -a -q|xargs docker rm
```

#### 启动重启停止容器

```bash
# 启动已有的被关闭的容器
docker start 容器id
# 重启容器
docker restart 容器id
# 停止当前正在运行的容器
docker stop 容器id
# 强制停止当前正在运行的容器。docker stop报错的话可以用dock kill关闭这个顽固的容器。
docker kill 容器id
```



### 常用的其他命令

#### 后台启动容器

```bash
[root@rootuser ~]# docker run -d centos
4bbeb1c1cbadba1020b47b82f58f2362ee87d6d2dd54ea1d4eb8893899458a13
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

常见的坑：

- 后台启动容器后，使用docker ps发现centos停止了。
  - 原因：docker容器在后台运行就必须要有一个前台进程（比如-it进入容器就相当于有前台进程），docker发现没有前台进程就会自动停止。
  - -tomcat的时候可以这么用（？不理解）

#### 查看日志命令

```bash
# 先看有哪些容器存在
[root@rootuser ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                      PORTS     NAMES
4bbeb1c1cbad   centos    "/bin/bash"   10 minutes ago   Exited (0) 10 minutes ago             elastic_babbage
9d987a3f0bca   centos    "/bin/bash"   28 minutes ago   Exited (0) 11 minutes ago             stoic_sammet
# 查看容器id为“9d987a3f0bca”的容器的日志
[root@rootuser ~]# docker logs -f -t --tail 10 9d987a3f0bca
[root@9d987a3f0bca /]# exit
2022-01-10T09:28:41.539285540Z exit
2022-01-10T09:44:44.198722927Z [root@9d987a3f0bca /]# exit
```

讲解：

- -f：是跟踪日志输出，相当于实时显示。
- -t：展示时间戳
- --tail n：展示日志的后n条数据
- 9d987a3f0bca：被查询日志的容器的id



使用docker help查看logs的用法：

```bash
[root@rootuser ~]# docker help logs

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
```

- 第一列是第二列的简称



#### 查看容器内部进程信息

```bash
# 因为之前把容器全关了，查看有哪些关闭着的容器
[root@rootuser ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS                    PORTS     NAMES
4bbeb1c1cbad   centos    "/bin/bash"   23 hours ago   Exited (0) 23 hours ago             elastic_babbage
9d987a3f0bca   centos    "/bin/bash"   23 hours ago   Exited (0) 23 hours ago             stoic_sammet
# 启动容器
[root@rootuser ~]# docker start 9d987a3f0bca
9d987a3f0bca
# 确认容器正在启动
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS         PORTS     NAMES
9d987a3f0bca   centos    "/bin/bash"   23 hours ago   Up 4 seconds             stoic_sammet
# 查看docker top的用法
[root@rootuser ~]# docker top --help

Usage:  docker top CONTAINER [ps OPTIONS]

Display the running processes of a container
# 使用docker top查看容器id为9d987a3f0bca的容器内部的进程信息
[root@rootuser ~]# docker top 9d987a3f0bca 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                13982               13963               0                   16:50               pts/0               00:00:00            /bin/bash
```

- uid：用户id
- pid：父id
- ppid：进程id

可以通过进程信息杀进程。

#### 查看Docker objects（常为容器）的元数据(low-level information)

```bash
# 查看docker inspect的用法
[root@rootuser ~]# docker inspect --help

Usage:  docker inspect [OPTIONS] NAME|ID [NAME|ID...]

Return low-level information on Docker objects

Options:
  -f, --format string   Format the output using the given Go template
  -s, --size            Display total file sizes if the type is container
      --type string     Return JSON for specified type
# 查看容器id为9d987a3f0bca的容器的底层信息
[root@rootuser ~]# docker inspect 9d987a3f0bca
[
    {
    # id的前若干位是本容器的容器id
        "Id": "9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a",
        "Created": "2022-01-10T09:28:25.577866758Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
        # 现状是运行中
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 13982,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-01-11T08:50:15.154996662Z",
            "FinishedAt": "2022-01-10T09:44:44.211896577Z"
        },
        # 容器所来源的镜像
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a/hostname",
        "HostsPath": "/var/lib/docker/containers/9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a/hosts",
        "LogPath": "/var/lib/docker/containers/9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a/9d987a3f0bca702e07dbcc19f6794a8e3499a2c57e795a0af356eab35dbcd90a-json.log",
        "Name": "/stoic_sammet",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/d0e170b78fa45ed94d897a26006880286a867646064370f6979f1f86a6249952-init/diff:/var/lib/docker/overlay2/d3d67a249fb5a65782e888049cd6b57e28a9e2b111d1f5e8988777490adbe956/diff",
                "MergedDir": "/var/lib/docker/overlay2/d0e170b78fa45ed94d897a26006880286a867646064370f6979f1f86a6249952/merged",
                "UpperDir": "/var/lib/docker/overlay2/d0e170b78fa45ed94d897a26006880286a867646064370f6979f1f86a6249952/diff",
                "WorkDir": "/var/lib/docker/overlay2/d0e170b78fa45ed94d897a26006880286a867646064370f6979f1f86a6249952/work"
            },
            "Name": "overlay2"
        },
        # 挂载，目前没用上
        "Mounts": [],
        # 容器的一些基本配置
        "Config": {
        # 默认的容器名字
            "Hostname": "9d987a3f0bca",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            # 环境变量里没有java，所以容器中目前不能用java，只能用一些基本命令。
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        # 容器的网络配置
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "426d831c780d3ed860b675adf4bbe8dcede81f50afaaae846de777cab3fe3849",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/426d831c780d",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "a16d244d71807b90fd70e40c8b6ed2e7be0ff267aec50265108b33e170fefa50",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
            # 当前使用的是“桥接网卡”
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "5f9bc0a0faaa995d376fd6edbac2b55e086fdd4382c7b5219eae78c967168a1c",
                    "EndpointID": "a16d244d71807b90fd70e40c8b6ed2e7be0ff267aec50265108b33e170fefa50",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
You have new mail in /var/spool/mail/root
```



#### 进入当前正在运行的容器

容器通常都是后台方式运行的，我们有时需要进入容器，修改一些配置

方式1，

```bash
# 命令
docker exec -it 容器id bashshell(默认的命令行方式)
```

实战：

```bash
# 查看有什么容器正在运行
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS       PORTS     NAMES
9d987a3f0bca   centos    "/bin/bash"   27 hours ago   Up 4 hours             stoic_sammet
You have new mail in /var/spool/mail/root
# 进入容器9d987a3f0bca
[root@rootuser ~]# docker exec -it 9d987a3f0bca /bin/bash
# 在容器内部执行各种命令
[root@9d987a3f0bca /]# ls 
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@9d987a3f0bca /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:50 pts/0    00:00:00 /bin/bash
root        15     0  0 12:43 pts/1    00:00:00 /bin/bash
root        31    15  0 12:43 pts/1    00:00:00 ps -ef
# 退出容器
[root@9d987a3f0bca /]# exit
exit
[root@rootuser ~]# 
```

---

方式2，

命令：

```bash
docker attach 容器id
```

实战：

```bash
# 进入容器
[root@rootuser ~]# docker attach 9d987a3f0bca
# 退出容器
[root@9d987a3f0bca /]# exit
exit
[root@rootuser ~]# 

```

---

方式1（exec）和方式2（attach）对比：

- docker exec是进入容器后开启一个新的终端，可以在里面操作（常用）。
- docker attach是进入容器中正在执行的终端，不会启动新的进程。

#### 容器和主机文件互相拷贝

命令：

```bash
# 从容器拷贝文件到物理主机。前面的地址是源，后面的地址是目的地。
docker cp 容器id:容器内路径 物理主机路径
# 从物理主机拷贝文件到容器
docker cp 物理主机路径 容器id:容器内路径
```

实战，从容器拷贝文件到主机：

```bash
# 因为没有容器正在开启，查看有哪些可用容器
[root@rootuser ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS                      PORTS     NAMES
4bbeb1c1cbad   centos    "/bin/bash"   28 hours ago   Exited (0) 28 hours ago               elastic_babbage
9d987a3f0bca   centos    "/bin/bash"   28 hours ago   Exited (0) 26 minutes ago             stoic_sammet
# 开启一个容器
[root@rootuser ~]# docker start 9d987a3f0bca
9d987a3f0bca
# 确认容器已经开启
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS         PORTS     NAMES
9d987a3f0bca   centos    "/bin/bash"   28 hours ago   Up 3 seconds             stoic_sammet
# 进入容器
[root@rootuser ~]# docker exec -it 9d987a3f0bca /bin/bash
# 在容器内存入文件zhangyun.java
[root@9d987a3f0bca /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@9d987a3f0bca /]# cd home
[root@9d987a3f0bca home]# ls
[root@9d987a3f0bca home]# touch zhangyun.java
[root@9d987a3f0bca home]# ls
zhangyun.java
# 退出容器
[root@9d987a3f0bca home]# exit
exit
[root@rootuser ~]# pwd
/root
[root@rootuser ~]# ls
packages  zhangyunroot
# 把容器内的zhangyun.java文件拷贝到主机指定目录中
[root@rootuser ~]# docker cp 9d987a3f0bca:/home/zhangyun.java /root
# 拷贝成功
[root@rootuser ~]# ls
packages  zhangyun.java  zhangyunroot
[root@rootuser ~]# 

```

- 拷贝是一个手动的过程，未来我们使用-v卷的技术，可以实现自动同步”容器的指定目录“和“主机的指定目录”

### 小结

docker命令逻辑示意图：

![See the source image](docker.assets/u7520828501267080632fm11gp0.png)

- 可以看到exec命令的图标是一个”终端“，对应“进入容器并开启新终端”。
- 可以看到attach命令的图标是“进入符号”，对应“进入容器的当前运行终端”。



docker的命令十分多，目前学的都是最常用的命令，后面会学习更多。

接下来就是一堆练习。



### 作业练习

#### docker安装nginx

nginx可以做负载均衡和反向代理。



1，docker搜索nignx镜像

```bash
[root@rootuser ~]# docker search nginx
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        16113     [OK]       
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2107                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   820                  [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   309                  
```

2，dockerhub官网找到合适的[版本](https://hub.docker.com/_/nginx)，可以看到很多帮助文档和信息。版本无特殊要求，用最新版就行。

- alpine是瘦身版的镜像，很小。

![image-20220112111623416](docker.assets/image-20220112111623416.png)

3，下载，这里直接下载最新版nginx镜像

```bash
[root@rootuser ~]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
a2abf6c4d29d: Pull complete 
a9edb18cadd1: Pull complete 
589b7251471a: Pull complete 
186b1aaa4aa6: Pull complete 
b4df32aa5a72: Pull complete 
a0bcbecc962e: Pull complete 
Digest: sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

4，查看本地所有镜像，确认nginx镜像已被下载：

```bash
[root@rootuser ~]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
docker72590/alpine   latest    f5a69fceabd2   11 days ago    5.59MB
nginx                latest    605c77e624dd   13 days ago    141MB
centos               latest    5d0da3dc9764   3 months ago   231MB
```

5，利用nginx镜像启动nignx容器

参数讲解：

- -d：以后台的方式启动nginx

- --name：给启动的nginx容器取个名字“nginx01”

- -p 宿主机port:容器port：

  - 容器port暴露nginx容器的指定端口号，默认为80。

  - 宿主机port暴露宿主机的指定端口号。暴露后要确保腾讯云的**”防火墙（安全组）**“也开放了该宿主机端口号，才能从外界访问。

    ![image-20220112114744114](docker.assets/image-20220112114744114.png)

实战：

```bash
# 最后的”nginx“是创建nginx容器所依托的镜像名
[root@rootuser ~]# docker run -d --name nginx01 -p 3344:80 nginx
4a9b60af741dd918d8ccd71b35c79d1ae0c4a54333d9d57f01b6256f28550e7e
# 确认nginx容器已启动
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
4a9b60af741d   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
9d987a3f0bca   centos    "/bin/bash"              42 hours ago         Up 14 hours                                                 stoic_sammet
```

6，用curl使本机自测主机端口

curl讲解：

- 在Linux中curl是一个**利用URL规则在命令行下工作的文件传输工具**，可以说是一款很强大的http命令行工具。
- 基本用法`# curl http://www.linux.com`。执行后，www.linux.com 的html就会显示在屏幕上了。
  Ps：由于安装linux的时候很多时候是没有安装桌面的，也意味着没有浏览器，**因此这个方法也经常用于测试一台服务器是否可以到达一个网站**

实战：

```bash
[root@rootuser ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
You have new mail in /var/spool/mail/root
```

- 可以看到访问本机3344端口，自动被映射为nginx容器的80端口。读取了nginx容器80端口对应的html页面。

图解宿主机端口vs容器端口：

![image-20220112114218249](docker.assets/image-20220112114218249.png)

7，外网测试访问宿主机内部的nginx容器

浏览器访问`腾讯云服务器公网ip:3344`，成功访问:

![image-20220112114553273](docker.assets/image-20220112114553273.png)

8，进入nginx容器看看

```bash
# 进入nginx容器
[root@rootuser ~]# docker exec -it nginx01 /bin/bash
# 查看nginx存在的目录，依次找到nginx配置文件地址
root@4a9b60af741d:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
# /etc/nginx是nginx容器配置文件的地址，查看有哪些配置文件
root@4a9b60af741d:/# ls /etc/nginx
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params
```

9，关闭nginx容器后尝试从外网访问nginx

```bash
# 离开容器
root@4a9b60af741d:/# exit
exit
# 停止nginx容器
[root@rootuser ~]# docker stop nginx01
nginx01
# 确认nginx容器已停止
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS        PORTS     NAMES
9d987a3f0bca   centos    "/bin/bash"   43 hours ago   Up 15 hours             stoic_sammet
```

不出所料，关闭nginx后访问不了了。

![image-20220112120039303](docker.assets/image-20220112120039303.png)

- 思考问题∶我们每次改动nginx配置文件，都需要进入容器内部？十分的麻烦，我要是可以在容器外部提供一个映射路径，达到在容器外部的宿主机修改文件，容器内部就 可以自动修改就好了：**-v数据卷**Ⅰ

#### docker安装tomcat

1，dockerhub查看心仪的tomcat版本

就使用官网教程展示的9.0版本吧：

![image-20220112121633966](docker.assets/image-20220112121633966.png)

2，尝试直接运行tomcat容器

- `docker run`其实会自动帮下载镜像，运行容器前下载镜像不是必须的。

命令（官网）：

```bash
docker run -it --rm tomcat:9.0
```

- -it：表示运行容器后进入容器
- --rm：我们之前的启动都是后台启动，停止了容器后容器还是能查到的；但是--rm表示”**用完即删**“，一般用来测试。

实战：

```bash
# 根据tomcat镜像启动一个容器，容器名不设定而用默认的，用完即删
[root@rootuser ~]# docker run -it --rm tomcat:9.0
# 运行容器时找不到镜像，自动下载镜像
Unable to find image 'tomcat:9.0' locally
9.0: Pulling from library/tomcat
0e29546d541c: Pull complete 
9b829c73b52b: Pull complete 
cb5b7ae36172: Pull complete 
6494e4811622: Pull complete 
668f6fcc5fa5: Pull complete 
dc120c3e0290: Pull complete 
8f7c0eebb7b1: Pull complete 
77b694f83996: Pull complete 
7662046c36cb: Pull complete 
b93639122cb4: Pull complete 
Digest: sha256:cd96d4f7d3f5fc4d3bc1622ec678207087b8215d55021a607ecaefba80b403ea
# 下载镜像完成
Status: Downloaded newer image for tomcat:9.0
# 启动tomcat容器，控制台的显示就是熟悉的标准的tomcat启动时的显示
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-11
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
12-Jan-2022 04:18:22.892 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name:   Apache Tomcat/9.0.56
12-Jan-2022 04:18:22.895 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Dec 2 2021 14:30:07 UTC
12-Jan-2022 04:18:22.895 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 9.0.56.0
12-Jan-2022 04:18:22.895 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
12-Jan-2022 04:18:22.896 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Version:            3.10.0-1160.45.1.el7.x86_64
12-Jan-2022 04:18:22.896 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Architecture:          amd64
12-Jan-2022 04:18:22.898 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Java Home:             /usr/local/openjdk-11
12-Jan-2022 04:18:22.898 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Version:           11.0.13+8
12-Jan-2022 04:18:22.898 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log JVM Vendor:            Oracle Corporation
12-Jan-2022 04:18:22.898 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_BASE:         /usr/local/tomcat
12-Jan-2022 04:18:22.898 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log CATALINA_HOME:         /usr/local/tomcat
12-Jan-2022 04:18:22.937 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.lang=ALL-UNNAMED
12-Jan-2022 04:18:22.937 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.io=ALL-UNNAMED
12-Jan-2022 04:18:22.937 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util=ALL-UNNAMED
12-Jan-2022 04:18:22.937 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.base/java.util.concurrent=ALL-UNNAMED
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djdk.tls.ephemeralDHKeySize=2048
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dorg.apache.catalina.security.SecurityListener.UMASK=0027
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dignore.endorsed.dirs=
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.base=/usr/local/tomcat
12-Jan-2022 04:18:22.938 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Dcatalina.home=/usr/local/tomcat
12-Jan-2022 04:18:22.939 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Command line argument: -Djava.io.tmpdir=/usr/local/tomcat/temp
12-Jan-2022 04:18:22.962 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded Apache Tomcat Native library [1.2.31] using APR version [1.7.0].
12-Jan-2022 04:18:22.962 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true], UDS [true].
12-Jan-2022 04:18:22.963 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR/OpenSSL configuration: useAprConnector [false], useOpenSSL [true]
12-Jan-2022 04:18:22.974 INFO [main] org.apache.catalina.core.AprLifecycleListener.initializeSSL OpenSSL successfully initialized [OpenSSL 1.1.1k  25 Mar 2021]
12-Jan-2022 04:18:24.227 INFO [main] org.apache.coyote.AbstractProtocol.init Initializing ProtocolHandler ["http-nio-8080"]
12-Jan-2022 04:18:24.332 INFO [main] org.apache.catalina.startup.Catalina.load Server initialization in [2127] milliseconds
12-Jan-2022 04:18:24.553 INFO [main] org.apache.catalina.core.StandardService.startInternal Starting service [Catalina]
12-Jan-2022 04:18:24.554 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet engine: [Apache Tomcat/9.0.56]
12-Jan-2022 04:18:24.585 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
# 启动tomcat容器完成
12-Jan-2022 04:18:24.610 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [278] milliseconds
```

新开一个页面，关闭tomcat：

```bash
# 查看当前运行的docker容器
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE        COMMAND             CREATED        STATUS        PORTS      NAMES
1db32e6040aa   tomcat:9.0   "catalina.sh run"   2 hours ago    Up 2 hours    8080/tcp   sleepy_robinson
9d987a3f0bca   centos       "/bin/bash"         45 hours ago   Up 17 hours              stoic_sammet
You have new mail in /var/spool/mail/root
# 停止tomcat容器
[root@rootuser ~]# docker stop 1db32e6040aa 
1db32e6040aa
# tomcat容器停止后，不仅是停止，而且也被删除。这就是docker run --rm的效果
[root@rootuser ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS                    PORTS     NAMES
4a9b60af741d   nginx     "/docker-entrypoint.…"   3 hours ago    Exited (0) 2 hours ago              nginx01
4bbeb1c1cbad   centos    "/bin/bash"              45 hours ago   Exited (0) 45 hours ago             elastic_babbage
9d987a3f0bca   centos    "/bin/bash"              45 hours ago   Up 17 hours                         stoic_sammet
```

以--rm方式运行容器仅用于测试，接下来我们还是老实点下载镜像后运行容器。

3，下载tomcat镜像

命令：

```bash
[root@rootuser ~]# docker pull tomcat:9.0
9.0: Pulling from library/tomcat
Digest: sha256:cd96d4f7d3f5fc4d3bc1622ec678207087b8215d55021a607ecaefba80b403ea
Status: Image is up to date for tomcat:9.0
docker.io/library/tomcat:9.0
You have new mail in /var/spool/mail/root
```

- 发现镜像已经存在，说明docker run --rm在关闭容器后仅仅删除容器，并不删除容器对应的镜像。

4，根据tomcat镜像运行容器，设定容器名为”tomcat01“

命令：

```bash
# 根据tomcat:9.0镜像运行一个名为“tomcat01”的容器
[root@rootuser ~]# docker run -d -p 3355:8080 --name tomcat01 tomcat:9.0
a3107503c6ff695724fb39c89dcebf188ec1f199406f585a6dc8692090eddb63

```

- -d：后台运行，容器启动后不会进入容器，而是停留在宿主机

- -p 宿主机端口:容器端口：把宿主机的3355端口和tomcat01容器的8080端口绑定。注意腾讯云防火墙要开启3355端口，才能从外网访问tomcat01容器。

  ![image-20220112144609905](docker.assets/image-20220112144609905.png)

- --name 容器名：给启动的容器命名，不能与已存在的容器同名

5，外网访问容器tomcat01

![image-20220112144909184](docker.assets/image-20220112144909184.png)

貌似失败了，但是这是正常现象，因为官方的docker tomcat是阉割版

6，进入tomcat01容器

实操：

```bash
# 进入tomcat容器
[root@rootuser ~]# docker exec -it tomcat01 /bin/bash
# webapps下没有文件，所以外界访问tomcat01容器显示404
root@a3107503c6ff:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
root@a3107503c6ff:/usr/local/tomcat# ls webapps
root@a3107503c6ff:/usr/local/tomcat# 
```

原因：阿里云镜像的原因，默认是最小镜像，所有不必要的都剔除，保证最小可运行的环境：

- tomcat01容器内的linux命令少了，比如`ll`l用不了
- webapps是空的

7，把webapps.dist下的文件夹全部复制到webapps下

webapps.dist下有root文件夹等webapps中必须有的文件夹。

实战：

```bash
# 把webapps.dist下的文件夹全部复制到webapps下
root@a3107503c6ff:/usr/local/tomcat# cp -r webapps.dist/* webapps
# 确认复制完成
root@a3107503c6ff:/usr/local/tomcat# ls webapps
ROOT  docs  examples  host-manager  manager
```

- -r：表示递归复制，文件夹连同文件夹内部全部复制。

8，刷新访问tomcat01容器的网页，发现可以访问了

![image-20220112152944360](docker.assets/image-20220112152944360.png)

#### docker安装ElasticSearch+Kibana

ElasticSearch+Kibana在大数据开发中很重要。

es特点：

- esI暴露的端口很多。
- es 十分的耗内存。
- es的数据一般需要放置到安全目录!挂载。

1，dockerhub网站搜索[elasticsearch](https://hub.docker.com/_/elasticsearch)

```bash
# 找到docker官方启动elasticsearch的命令，稍作删改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
```

- -e：设置docker容器环境变量

elasticsearch非常占内存，启动后宿主机都会卡。但是因为我的宿主机没有2g可用的内存，所以启动失败，要修改启动命令来限制elasticsearch占用的内存。

2，限制es占用的内存并启动

使用-e来修改配置文件，从而限制es的内存。

实战：

```bash
# 启动es
[root@rootuser ~]# docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
f4ba51ed1aae2f871b279380476ae87a07c2823f4d4612e6eeef49ea75da39df
# 确认Es是否启动
[root@rootuser ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                                                                  NAMES
f4ba51ed1aae   elasticsearch:7.6.2   "/usr/local/bin/dock…"   5 seconds ago   Up 4 seconds   0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 0.0.0.0:9300->9300/tcp, :::9300->9300/tcp   elasticsearch02

```

- ES_JAVA_OPTS：涉及jvm的知识，这里最小64m，最多512m

可以看到这次限制了ES占用给的内存后，ES成功启动了。

3，查看docker容器占用的资源

命令：

```bash
docker stats
```

结果：

![image-20220112173910253](docker.assets/image-20220112173910253.png)

可以看出ES占用的内存在64m到512m之间，符合我们的设置。

4，curl测试宿主机对容器的端口连接

实战：

```bash
[root@rootuser ~]# curl localhost:9200
{
  "name" : "f4ba51ed1aae",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "Qobvm-7RSXOGb0xCc7Y14A",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

成功访问了ES的9200端口。



#### docker中kibana连接ES思路

示意图：

![image-20220112174609934](docker.assets/image-20220112174609934.png)



### 可视化

可视化工具：

- portainer：先用这个。
- Rancher：CI/CD（持续集成/持续部署）的时候再用

#### 什么是portainer

Docker图形化界面管理工具，提供一个后台面板供我们操作!

docker安装portainer容器：

```bash
[root@rootuser ~]# docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
94cfa856b2b1: Pull complete 
49d59ee0881a: Pull complete 
a2300fd28637: Pull complete 
Digest: sha256:fb45b43738646048a0a0cc74fcee2865b69efde857e710126084ee5de9be0f3f
Status: Downloaded newer image for portainer/portainer:latest
3697145e63bef7400c9dc6af042ee448f48082d5b85f9681396f31e5c087a8ec
```

- -v：是挂载，目前不用深究。

- --privileged：授权访问。

- -p 8088:9000：把宿主机的8088端口，映射到portainer容器的9000端口。请打开腾讯云安全组的8088端口。

  ![image-20220112202140844](docker.assets/image-20220112202140844.png)

外网访问`服务器ip:8088`：

![image-20220112203132067](docker.assets/image-20220112203132067.png)

- 第一次访问的话设置一下密码

进入页面，选择local（即宿主机）的docker环境：

![image-20220112203431325](docker.assets/image-20220112203431325.png)

进入local环境后，即可图形化查看和配置宿主机的docker：

![image-20220112203731890](docker.assets/image-20220112203731890.png)

不过可视化面板平时不会用，测试玩玩了解即可。



## Docker镜像讲解

### 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

得到镜像的方式：

- 远程仓库下载
- 朋友拷贝过来
- 自己制作一个镜像DockerFile



### Docker镜像加载原理

#### UnionFS(联合文件系统)

UnionFS(联合文件系统）) :Union文件系统(UnionFS )是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtualfilesystem)。Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性:一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

#### Docker镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS.

bootfs(boot file system)主要包含bootloader和kernel（内核）, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux或Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system)，在bootfs之上。包含的就是典型Linux.系统中的/dev,/proc,/bin, /etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu , Centos等等。

分层安装镜像示意图：

![image-20220112205838281](docker.assets/image-20220112205838281.png)

docker镜像很小，centos虚拟机压缩包1个g起步，但是centos的docker镜像200M就够了。

对于一个精简的os , rootfs 可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别,因此不同的发行版可以公用bootfs.

虚拟机是分钟级别的，容器是秒级的。



### 分层理解

https://www.bilibili.com/video/BV1og4y1q7M4?p=19&spm_id_from=pageDriver

