---
title: linux
date: 2022-05-02 11:10:45
author: JonQuet
top: true
cover: true
tags:
- linux
categories:
- 服务器
- linux
password: 
summary: linux
typora-copy-images-to: upload
---

# Linux部署java应用

## Linux

windows完成编码然后部署到linux，linux一般作为服务器

阿里云 ，华为云，腾讯云这些都不是免费的

所以安装虚拟机 

centos7就是一个企业级linux，完全开源，完全免费

### 安装软件

- 虚拟机VMware15
- centos7
- 安装java环境，jdk8
- 安装mysql数据库
- 安装tomcat  比如ssm，servlet jsp等都需要，springboot不用
- 安装xshell，执行命令
- 安装xftp，传输文件

#### 安装VM

直接安装包下一步就可以

### 导入centos

centos不是安装时导入压缩包，把centos放入vm文件夹，解压centos

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502125634.png)

将解压后的文件导入vm

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502125857.png)

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502130005.png)

修改网络适配器类型，默认是桥接模式，不建议，如果网络不稳定，ip会变，一会可以连一会不可以

推荐使用NAT模式，ip地址固定不变

1. 打开设置

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502130652.png)

2. 修改nat

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502130804.png)

3. 网络编辑器

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502131037.png)

4. 必须是管理员身份登录

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502131209.png)

然后就可以启动了，但可能由于centos原因我失败了，于是我从官网下了centos镜像，b站up主的视频

<iframe src="//player.bilibili.com/player.html?aid=251108490&bvid=BV1Jv41137To&cid=425287119&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"width="100%" height="720"> </iframe>

#### 测试ping ip

linux 输入命令  ifconfig

![image-20220502195035576](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502195046.png)

可以看到ip了，然后用window看是否可以连通

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502195249.png)

可以ping通过，环境搭建成功。

#### 官网安装xshell和xftp

https://www.xshellcn.com/xiazai.html

都必须是最新版的否则无法运行。

### 安装JDK

管理员登录linux

用户名：root

密码：201314

使用rpm方式

1. 将安装包传入到linux中
2. 打开xshell，新建一个会话

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502210435.png)

4. 输入用户名

   ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502210531.png)

5. 输入密码

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502210659.png)

6. 打开xftp将jdk传到linux上

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502211315.png)

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502211347.png)

左边是windows，右边是linux，把jdk直接拖入linux

7. java -version 显示linux自带jdk删除

![image-20220502212333098](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502212333.png)

8. 使用rpm -qa | grep jdk查询jdk

   ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502213134.png)

9. 删除jdk rpm -e --nodeps+jsk

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502213154.png)

10. 安装jdk rpm -ivh jdk-8u211-linux-x64.rpm

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502214304.png)

11. 再次执行 rpm -qa | grep jdk查询jdk

    ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502214434.png)

发现只有一个jdk，成功

#### 配置jdk环境变量

- 编辑环境变量命令：vim /etc/profile
- 按下键盘 i 切换为输入模式
- 配置环境变量

```java
JAVA_HOME=/usr/java/jdk1.8.0_221-amd64
CLASSPATH=%JAVA_HOME%/lib:%JAVA_HOME%/jre/lib
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export PATH CLASSPATH JAVA_HOME
```

- 让配置生效

```java
source /etc/profile
```

配置成功

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502220505.png)

### 安装tomcat

- 配置tomcat

  ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503102901.png)

- 解压

  ```java
  tar -zxvf apache-tomcat-9.0.34.tar.gz
  ```

  ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503103059.png)

  - 查看解压后的文件夹

    ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503103243.png)

    - 启动tomcat

      启动tomcat linux里直接执行bin文件里面的startup.sh，windows执行startup.bat，不同系统不一样

      ./startup.sh

      ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503103635.png)

      可以打开浏览器看一下是不是启动成功了

      ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503104148.png)

      - centos开放8080端口

        1. 检查防火墙状态 ：firewall-cmd --state  runing表示防火墙是开启的

        1. 如果没有开启要执行开始命令：systemctl start firwalld.service

      -  开放端口

        1. firewall-cmd --zone=public --add-port=8080/tcp --permanent

           ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503105714.png)

        2. 重新启动防火墙 ：systemctl  restart firwalld.service

## 部署springbooot应用

### 本地部署

### 部署在linux上

1. 数据库连接之后，修改配置文件

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503092638.png)

2. 打包

   ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503092721.png)

   3. 打包完成后可以看到

      ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503092950.png)

      4. 在本地启动

         ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503093415.png)

         ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503093445.png)

         5. 部署到linux

            - 直接用xftp放到java里

            ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503100607.png)

            - 查看已经进去了

            ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503100751.png)

            - 执行启动命令 java -jar

              ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503101001.png)

              - 测试

                把本地关了

                ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503101227.png)

                输入linux域名

                ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220503111343.png)

## 安装mysql

1. 先将mysql压缩包导入到linux中去
2. 解压缩

```java
tar -xvf mysql-8.0.20-1.el7.x86_64.rpm-bundle.tar 
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502221853.png)

3. 安装 common、libs、client、server

```java
rpm -ivh mysql-community-common-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-libs-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-client-8.0.20-1.el7.x86_64.rpm --nodeps --force
rpm -ivh mysql-community-server-8.0.20-1.el7.x86_64.rpm --nodeps --force
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502223122.png)

4. 安装前删除自带的 mariadb

查看 rpm -qa | grep mariadb

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502222346.png)

删除 rpm -e --nodeps mariadb-libs-5.5.64-1.el7.x86_64

5. 初始化mysql

```sql
mysqld --initialize
```

6. 授权防火墙

```sql
xxxxxxxxxx chown mysql:mysql /var/lib/mysql -R;
systemctl start mysqld.service;
systemctl enable mysqld;
```

7. 查看数据库的初始化密码

```sql
cat /var/log/mysqld.log | grep password
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502225313.png)

8. 登录数据库

```sql
mysql -uroot -p
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502225342.png)

9. 修改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502225546.png)

此时密码为root

退出 ：**exit**

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502225640.png)

10. 使用新密码登录

    ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502225739.png)

11. 开启远程访问

    本地新建连接

    ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502230250.png)

    测试连接发现连不上，开启远程连接

```sql
create user 'root'@'%' identified with mysql_native_password by 'root';
grant all privileges on *.* to 'root'@'%' with grant option;
flush privileges;
```

12. 开放 3306 端口

    先退出mysql ：exit

    ![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502230847.png)

```sql
firewall-cmd --zone=public --add-port=3306/tcp --permanent
systemctl restart firewalld.service
firewall-cmd --reload
```

13. MySQL 安装默认使用美国的时区，北京时间比美国晚 8 小时

```sql
set global time_zone='+8:00';
```

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502231149.png)

到此为止就可以上了

![](https://cdn.jsdelivr.net/gh/253715/253715-imgs/images/20220502232529.png)

14. 创建数据表

```sql
create database test character set utf8 collate utf8_general_ci;
use test;
create table user(
    id int primary key auto_increment,
    name varchar(22),
    birthday datetime
);
insert into user(name, birthday) VALUES ('小明','1999-01-01');
insert into user(name, birthday) VALUES ('小红','2000-01-01');
```

springboot jar包启动 ：java -jar demo.jar
