---
title: 远程服务器安装Docker并配置数据库
date: 2022-03-05 11:02:36
categories: 实战
tags: ['Docker','数据库']
---

## Docker命令

### 安装docker-desktop

[点击前往官网下载，一直下一步安装即可](https://www.docker.com/get-started)

[如果出现错误，说明wsl内核未更新到wsl2，点击下载安装之后重启docker-desktop即可](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

### 查看已安装docker版本

```shell
docker --version
```
### 安装docker-compose
>Docker Compose是一个工具，旨在帮助定义和共享多容器应用程序。使用Compose，我们可以创建一个YAML文件来定义服务，并且使用单个命令，可以启动所有内容或将其全部拆解。
>使用 Compose的最大优点是，您可以在文件中定义应用程序堆栈，将其保留在项目存储库的根目录下（现在是受版本控制的），并轻松地让其他人能够为您的项目做出贡献。有人只需要克隆你的存储库并启动撰写应用。事实上，你现在可能会在GitHub/GitLab上看到相当多的项目在做这件事。

简单来说就是 docker-compose能够让你自定义一个YAML配置文件，能够一键启动所有任务
<mark>安装了docker desktop的自带docker-compose不需要再安装了</mark>

- Linux安装教程 https://docs.docker.com/compose/install/



### 安装mongo

使用Docker Hub搜索mongo [点击进入mongo-Docker Hub](https://hub.docker.com/_/mongo?tab=tags)

```shell
docker pull mongo:4  #可接版本号也可不接
```

### 查看本地下载了哪些镜像

```shell
docker images
```

### 运行mongo映射到宿主机上

```shell
docker run -d --name some-mongo -p 10050:27017 mongo:4
```

### 运行MYSQL映射到宿主机上

```shell
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 -p 10051:3306 -d mysql:5.6
```

### 查看当前运行的服务

```shell
docker ps
```

### 在Linux机器中需要放行端口 10050 

#### 方案一 直接关闭防火墙

```shell
#ubuntu
service ufw stop
#centos
service firewalld stop
```

#### 方案二 放行指定端口

```shell
# ubuntu
ufw allow Port 端口号

#centos
firewall-cmd --zone=public --add-port=10050/tcp --permanent
```

#### 重载防火墙

```shell
firewall-cmd --reload
```

### 提交自己的images

#### 提交到docker仓库

```shell
docker commit id号  liyunfu/mysql:1.0
docker push liyunfu/mysql:1.0 
```

#### 拉取自己的images

```shell
docker pull liyunfu/mysql:1.0
```

#### 删除images

```shell
docker image rm id号
```



## docker-compose

#### 编写YML文件

```yaml
version: '3'
services:
  mysql1:
    image: mysql
    environment:
    - MYSQL_ROOT_PASSWORD=123456
    ports:
    - 10052:3306

  mysql2:
    image: mysql
    environment:
    - MYSQL_ROOT_PASSWORD=123456
    ports:
    - 10053:3306
```

#### 执行YML

```shell
docker-compose up
```



## Linux

### 连接远程服务器

```shell
ssh -p 27822 root@server.bontor.cn   # ssh -p 端口  用户名@服务器地址
```

### 查看操作系统版本

```shell
lsb_release -a
```

### 查看内核版本

```shell
uname -a
```

### 检查文件系统磁盘空间占用情况

```shell
df -Th  # 后缀变为以G为单位 不加以字节为单位
```

### 目录结构

```shell
/home  主目录
/etc   软件配置文件
/sys   系统目录
/usr   系统可执行文件
/var   日志文件 不断增长大小
```

### CPU和内存

```shell
top #查看正在运行的进程 已经cpu占用情况 和内存使用情况
```

### 文档型：文件相关命令(touch，cat，echo，rm，vi，cd)

```shell
touch test.txt 
vi test.txt
cd /home
cat test.txt
echo "123123">>test.txt  #两个箭头追加 一个箭头覆盖  
```

### 硬件型：磁盘/进程/服务/网络

#### 查看进程

```shell
ps -ef | grep docker  #查询并使用grep筛选 docker进程
```

#### 强制关闭进程

```shell
kill -9 进程的PID
```

#### 查看服务的状态

```shell
service sshd status
```

#### 关闭服务

```shell
service sshd stop  #关闭sshd服务
```

#### 重启服务

```shell 
service sshd restart
```

### 功能型：压缩/解压，下载，远程

#### 下载

```shell
wget 资源地址
```

#### 解压

```shell
tar zxvf app.tar.gz
```

#### 压缩

```shell
tar zcvf app.tar.gz app
```



### 修改默认SSH端口

#### 查看默认监听端口

```shell
netstat -anlp | grep sshd
```

#### 修改默认监听端口

```shell
vi /etc/ssh/sshd_config
#删除#  修改port 22 为 port 27001

#centos修改之后必须运行
semanage port -a -t ssh_port_t -p tcp 27001   
#提示没有该命令
yum whatprovides semanage
#找到拥有命令的包安装
yum install -y policycoreutils-python
#查看端口
semanage port -l | grep ssh

#删除端口
semanage port -d -t ssh_port_t -p tcp 22
```
