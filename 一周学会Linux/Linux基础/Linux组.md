## Linux组基本介绍
在 linux 中的每个用户必须属于一个组，不能独立于组外。在 linux 中每个文件有所有者、所在组、其它组的概念。谁创建的文件就是该文件的所有者，而该所有者所归属的组的其他成员对该文件具有一定权限，其余组别对于该文件来说就属于其他组，其他组对该文件也具有一定权限。
## 查看文件所有者
使用指令：``ls ahl``

![](https://files.mdnice.com/user/25190/4dcf6acf-5036-471a-8548-f446edab5f56.png)

图中画线的为文件所有者，画线后的root为文件所在组。

## 修改文件/目录所有者
修改文件所有者指令：``chown 用户名 文件/目录名``
改变所有者和所在组指令：``chown newowner:newgroup 文件/目录 ``

**-R**如果是目录则使其下所有子文件或目录递归生效

* 应用案例

1. 要求：使用 root 创建一个文件 apple.txt ，然后将其所有者修改成 tom chown tom apple.txt。使用指令``chown tom apple.txt``

2. 请将 /home/test 目录下所有的文件和目录的所有者都修改成 tom，使用指令``chown -R tom /home/test``
## 组的创建
* 基本指令
``groupadd 组名``
* 应用实例
创建一个组，monster使用命令``groupadd monster``

创建一个用户fox，并放入到monster组中，使用命令``useradd -g monster fox``
##  修改文件/目录所在的组
* 基本指令

``chgrp 组名 文件/目录名``

* 应用实例

1. 使用 root 用户创建文件 orange.txt ,看看当前这个文件属于哪个组，然后将这个文件所在组，修改到 fruit 组。

	1.  ``groupadd fruit``
	2.  ``touch orange.txt``
	3.   看看当前这个文件属于哪个组 -> root 组
	4.  ``chgrp fruit orange.txt``
2. 请将 /home/abc .txt 文件的所在组修改成 shaolin (少林) groupadd shaolin，使用指令``chgrp shaolin /home/abc.txt``

3. 请将 /home/test 目录下所有的文件和目录的所在组都修改成 shaolin(少林)，使用指令chgrp -R shaolin /home/test

## 改变用户所在组
在添加用户时，可以指定将该用户添加到哪个组中，同样的用 root 的管理权限可以改变某个用户所在的组。
* 基本指令
``usermod –g 新组名  用户名``
``usermod –d  目录名  用户名`` 改变该用户登陆的初始目录。
**特别说明**：用户需要有进入到新目录的权限。
* 应用实例
将 zwj 这个用户从原来所在组，修改到 wudang 组，使用指令``usermod -g wudang zwj``。

