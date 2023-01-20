## Ubuntu介绍
Ubuntu（友帮拓、优般图、乌班图）是一个以桌面应用为主的开源 GNU/Linux 操作系统，Ubuntu 是基于 GNU/Linux， 支持 x86、amd64（即 x64）和 ppc 架构，由全球化的专业开发团队（Canonical Ltd）打造的。

专业的 Python 开发者一般会选择 Ubuntu 这款 Linux 系统作为生产平台. 温馨提示：

Ubuntu 和 Centos 都是基于 GNU/Linux 内核的，因此基本使用和Centos 是几乎一样的，它们的各种指令可以通用，同学们在学习和使用 Ubuntu 的过程中，会发现各种操作指令在前面学习CentOS 都使用过。只是界面和预安装的软件有所差别。

Ubuntu [下载地址](http://cn.ubuntu.com/download/)
## Ubuntu的root用户
安装 ubuntu 成功后，都是普通用户权限，并没有最高 root 权限，如果需要使用 root 权限的时候，通常都会在命令前面加上 sudo 。有的时候感觉很麻烦。

我们一般使用 su 命令来直接切换到 root 用户的，但是如果没有给 root 设置初始密码，就会抛出 su : Authentication failure 这样的问题。所以，我们只要给 root 用户设置一个初始密码就好了。

### 给root 用户设置密码并使用
1. 输入``sudo passwd``命令，设定 root 用户密码。
2. 设定 root 密码成功后，输入 su 命令，并输入刚才设定的 root 密码，就可以切换成 root 了。提示符$代表一般用户， 提示符#代表 root 用户。
3. 以后就可以使用 root 用户了
4. 输入 exit 命令，退出 root 并返回一般用户

## APT软件管理
### apt介绍
apt 是 Advanced Packaging Tool 的简称，是一款安装包管理工具，类似于[[RPM与YUM#yum]]。在 Ubuntu 下，我们可以使用 apt 命令进行软件包的安装、删除、清理等，类似于 Windows 中的软件管理工具。Ubuntu软件管理的原理示意图：
![](https://files.mdnice.com/user/25190/87584749-a680-4e9a-8aff-34889c95beda.png)
Ubuntu系统中/etc/apt/sources.list中有服务器地址，Ubuntu系统经过网关，国内网络再到APT服务器获取APT软件。从国内网络到APT服务器这个过程将会很漫长。这时便有了APT的国内镜像网站。
### Ubuntu软件操作的相关命令
```linux
sudo apt-get update 更新源
sudo apt-get install package 安装包
sudo apt-get remove package 删除包
sudo apt-cache search package 搜索软件包
sudo apt-cache show package 获取包的相关信息，如说明、大小、版本等
sudo apt-get install package --reinstall 重新安装包

sudo apt-get -f install 修复安装包
sudo apt-get remove package --purge 删除包，包括配置文件等
sudo apt-get build-dep package 安装相关的编译环境
sudo apt-get upgrade 更新已安装的包
sudo apt-get dist-upgrade 升级系统
sudo apt-cache depends package 了解使用该包依赖那些包
sudo apt-cache rdepends package  查看该包被哪些包依赖
sudo apt-get source package 下载该包的源代码
```
### 更新Ubuntu软件下载地址
#### 寻找国内镜像源
所谓的镜像源：可以理解为提供下载软件的地方，比如 Android 手机上可以下载软件的安卓市场；iOS 手机上可以下载软件的 AppStore。
![](https://files.mdnice.com/user/25190/e2feca11-0de7-457b-98c8-f3cc31e8730e.png)
![](https://files.mdnice.com/user/25190/58aec063-e643-43be-8be7-79218a8ff787.png)
#### 备份Ubuntu默认的源地址
使用指令``sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup``
![](https://files.mdnice.com/user/25190/0fe10770-e66e-430f-a55e-9013f474cf4c.png)

#### 更新源服务器列表
先 sources.list 文件
![](https://files.mdnice.com/user/25190/0e7afb28-ac8d-445f-bca0-2670daa73882.png)
复制镜像网站的地址， 拷贝到 sources.list 文件
![](https://files.mdnice.com/user/25190/29a0ff36-5bcd-4611-be00-c233c78aa96f.png)
#### 更新源
更新源地址：sudo apt-get update
![](https://files.mdnice.com/user/25190/38146f65-827b-468d-b199-a0c50f5beb0f.png)
#### Ubuntu软件安装，卸载的最佳实践
案例说明：使用 apt 完成安装和卸载 vim 软件，并查询 vim 软件的信息：（因为使用了镜像网站， 速度很快）
```
sudo apt-get remove vim //删除
sudo apt-get install vim //安装
sudo apt-cache show vim //获取软件信息
```
## 远程登陆Ubuntu
### ssh介绍
SSH 为 Secure Shell 的缩写，由 IETF 的网络工作小组（Network Working Group）所制定；SSH 为建立在应用层和传输层基础上的安全协议。

SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。常用于远程登录。几乎所有 UNIX/LInux平台都可运行 SSH。

使用 SSH 服务，需要安装相应的服务器和客户端。客户端和服务器的关系：如果，A 机器想被 B 机器远程控制， 那么，A 机器需要安装 SSH 服务器，B 机器需要安装 SSH 客户端。

和 CentOS 不一样，Ubuntu 默认没有安装 SSHD 服务(使用 netstat 指令查看: apt install net-tools)，因此，我们不能进行远程登陆。
### 原理示意图
![](https://files.mdnice.com/user/25190/6d1d5204-7a2f-403f-9415-9616d4539157.png)
### 安装SSH和启用
``sudo apt-get install openssh-server``
执行上面指令后，在当前这台 Linux 上就安装了 SSH 服务端和客户端。
``service sshd restart``
执行上面的指令，就启动了 sshd 服务，会监听端口 22。

### 在Windows 使用XShell6/XFTP6 登录 Ubuntu
先在Ubuntu系统中使用``ifconfig``指令查看本机的IP，然后在Windows的Xshell中输入IP地址、用户名和用户密码进行登陆。
### 从一台linux 系统远程登陆另外一台linux 系统
在创建服务器集群时，会使用到该技术
1. 基本语法：
``ssh 用户名@IP``
例如：``ssh [hspedu@192.168.200.130]``
使用 ssh 访问，如访问出现错误。可查看是否有该文件 ～/.ssh/known_ssh 尝试删除该文件解决，一般不会有问题

登出使用指令``exit``或``logout``。
![](https://files.mdnice.com/user/25190/a4d58ef5-4e8c-4e3b-af20-d9a1c815fc46.png)