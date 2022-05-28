---
title: Flutter01环境配置
date: 2022-05-28 19:49:05
tags: ['flutter','多端']
---

### 安装flutter

`flutter3 还不支持交叉编译`

<mark>学了一段时间的flutter准备从0开始记录flutter3的学习过程</mark>

#### 安装SDK

进入<https://docs.flutter.dev/get-started/install/windows> 下载压缩包，将压缩包解压到C盘根目录下，不要解压到Program files或Program Files(x86)这两个高权限目录下。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/895fe64fbbbb4e23a3ca24dc89279d90~tplv-k3u1fbpfcp-zoom-1.image)

#### 配置环境变量

将flutter目录下bin的地址填入环境变量path中，重启电脑或注销。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b4e90690088549cb93d1bdc1fc4416b0~tplv-k3u1fbpfcp-zoom-1.image)

#### 查看是否配置成功

命令行键入flutter查看是否配置成功

报错

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1bbff8905cd4a8680df525c2e79985f~tplv-k3u1fbpfcp-zoom-1.image)

使用`git clone -b stable https://github.com/flutter/flutter.git`重新克隆下来，将原先的删除

#### 安装Dart SDK

键入flutter等待dart sdk下载完成

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d4ea54c4e25e43e994d9712da22387ca~tplv-k3u1fbpfcp-zoom-1.image)

\


### 安装其他

键入flutter doctor查看还有以下几种没有解决的，下面我们一步一步来解决。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2638c61b806145cf89ddb09a4838dd57~tplv-k3u1fbpfcp-zoom-1.image)

#### 安装chrome

进入<https://www.google.com/chrome/> 下载安装即可

#### 安装visual studio 2022

进入<https://visualstudio.microsoft.com/zh-hans/downloads/> 下载社区版即可，勾选`使用C++的桌面开发`点击安装，等待安装完成。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e777567d3cb467d88105013148bb596~tplv-k3u1fbpfcp-zoom-1.image)

#### 安装Andriod Studio

进入<https://developer.android.com/studio> 下载，勾选`Andriod Virtual Device`，一路默认勾选，等待安装完成，如出现红色，尝试科学上网。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/552b0a08938c486d8dadb5e2e6b15668~tplv-k3u1fbpfcp-zoom-1.image)

#### 安装Andriod SDK

选择`SDK Manager`，勾选`Andriod SDK Command-line Tools`，如果是需要的话可以将下面勾选的`HAXM`一起安装，

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1d726e8cbfc434d96705bb32048cb1c~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fce766d0e1c9420784ab70348dbeff70~tplv-k3u1fbpfcp-zoom-1.image)

#### 安装安卓虚拟机

选择`Virtual Device Manager`，选择`Pixel 5`，下一步，选择推荐的安卓版本`R`下一步 等待安装成功。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4b7526d8ba6b4d0ea6348a607c79ac47~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1aa5e5e157064317ac3b6a8732cb7003~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88aaa6dc9343450f8a59986f1c44f5c3~tplv-k3u1fbpfcp-zoom-1.image)

#### 查看是否配置成功

运行flutter doctor

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/415044a34a834560b0929c67c091460a~tplv-k3u1fbpfcp-zoom-1.image)

### 其他问题

#### 解决Visual Studio报错

这个错是由于2022这个版本出现的错误，可以进入<https://github.com/flutter/flutter/issues/102451> 查看，只有临时解决方案，根据指引下载`vswhere.exe`发放入指定目录下，再使用flutter doctor测试。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/84d22c24b7bc4c899bf05d85e3470d9d~tplv-k3u1fbpfcp-zoom-1.image)

#### 添加flutter国内镜像源

给flutter添加国内的镜像，添加到环境变量中

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56e499998a8b440b95aefd271277c733~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/462c095fe9e4473693bf9af12b44a5eb~tplv-k3u1fbpfcp-zoom-1.image)

\


#### 解决信号灯超时时间已到(网络问题)

<https://stackoverflow.com/questions/71063780/how-is-http-host-availability-in-flutter-2-10> 根据回答，修改maven地址。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eaf44b73e2ce4018b5c30ed78eee86bc~tplv-k3u1fbpfcp-zoom-1.image)

#### 测试：

非常棒，可以愉快开发了。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/62ac5c26ad644978bf092014c1c4503c~tplv-k3u1fbpfcp-zoom-1.image)
