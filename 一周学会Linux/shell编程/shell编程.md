## 为什么要学习Shell 编程

1) Linux 运维工程师在进行服务器集群管理时，需要编写 Shell 程序来进行服务器管理。

2) 对于 JavaEE 和 Python 程序员来说，工作的需要，你的老大会要求你编写一些 Shell 脚本进行程序或者是服务器的维护，比如编写一个定时备份数据库的脚本。

3) 对于大数据程序员来说，需要编写 Shell 程序来管理集群

## shell是什么
Shell 是一个命令行解释器，它为用户提供了一个向 Linux 内核发送请求以便运行程序的界面系统级程序，用户可以用 Shell 来启动、挂起、停止甚至是编写一些程序。
![](https://files.mdnice.com/user/25190/7387a484-021f-4ec6-a8e2-bbf2e39d79ef.png)
## shell脚本的执行方式
### 脚本格式要求
1. 脚本以``#!/bin/bash``开头
2. 脚本须有可执行权限

### 编写一个shell脚本
需求说明：创建一个 Shell 脚本，输出 hello world! vim hello.sh
```sh
#!/bin/bash

echo "hello,world~"
```
### 脚本的常用执行方式
1. 方式一： (输入脚本的绝对路径或相对路径)

说明：首先要赋予 helloworld.sh 脚本的+x 权限， 再执行脚本比如 ./hello.sh 或者使用绝对路径 /root/shcode/hello.sh
2. 方式二：（sh+脚本）
说明：不用赋予脚本+x 权限，直接执行即可。比如 sh hello.sh ,  也可以使用绝对路径。

## shell变量
### shell变量介绍
1)      Linux Shell 中的变量分为系统变量和用户自定义变量。
2)      系统变量：\$HOME、\$PWD、\$SHELL、\$USER 等等，比如： ``echo $HOME`` 等等..
3)      显示当前 shell 中所有变量：``set``

### shell变量的定义
1. 基本语法
定义变量：``变量名=值``（注意：等号两边不能有空格）
撤销变量：``unset 变量``
声明静态变量：``readonly 变量``，注意：不能 unset
2. 应用案例
1)      案例 1：定义变量 A
```sh
#!/bin/bash

#案例 1：定义变量 A 
A=100
#输出变量需要加上$ echo A=$A
echo "A=$A"
```
2)      案例 2：撤销变量 A
```sh
#!/bin/bash
unset A

echo "A=$A"
#撤销变量A后，输出的结果为"A= "
```

3. 定义变量的规则

1) 变量名称可以由字母、数字和下划线组成，但是不能以数字开头。5A=200(×)
2) 等号两侧不能有空格
3) 变量名称一般习惯为大写， 这是一个规范，我们遵守即可

4. 将命令的返回值赋给变量

1) A=\`date\`反引号，运行里面的命令，并把结果返回给变量 A
2) A=$(date) 等价于反引号


3) 案例 3：声明静态的变量 B=2，不能 unset
```sh
#!/bin/bash
readonly B=2

echo "B=$B" #unset B会报错

#将指令返回的结果赋给变量

C=`date` #``号不能少
D=$(date) 
echo "C=$C" 
echo "D=$D"

#使用环境变量 TOMCAT_HOME

echo "tomcat_home=$TOMCAT_HOME"
```
在shell编程中的多行注释是使用的``:<<! 内容 !``。如
```sh
#!/bin/bash
:<<! 内容
C=`date` #``号不能少
D=$(date) 
echo "C=$C" 
echo "D=$D"
!
```
### 设置环境变量/全局变量
1. 基本语法
1) ``export 变量名=变量值`` （功能描述：在/etc/profile文件中定义，将 shell 变量输出为环境变量/全局变量）
2) ``source 配置文件`` （功能描述：更新，让修改后的配置信息立即生效）
3) ``echo $变量名``（功能描述：查询环境变量的值）

2. 快速入门
1) 在/etc/profile 文件中定义 TOMCAT_HOME 环境变量
2) 查看环境变量 TOMCAT_HOME 的值
3) 在另外一个 shell 程序中使用 TOMCAT_HOME

注意：在输出 TOMCAT_HOME 环境变量前，需要让其生效，使用指令``source /etc/profile``

### 位置参数变量
当我们执行一个 shell 脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量

比如 ： ./myshell.sh 100 200 , 这个就是一个执行 shell 的命令行，可以在 myshell 脚本中获取到参数信息。myshell.sh中的内容为
```sh
#!/bin/bash
echo "0=$0 1=$1 2=$2"
echo "所有的参数=$*"
echo "$@"
echo "参数的个数=$#"
```
![](https://files.mdnice.com/user/25190/d22c6828-7ec7-4706-8e11-518a95d90c0d.png)

### 预定义变量
1. 基本介绍

就是 shell 设计者事先已经定义好的变量，可以直接在 shell 脚本中使用

2. 基本语法

1) \$\$ （功能描述：当前进程的进程号（PID））
2) \$! （功能描述：后台运行的最后一个进程的进程号（PID））
3)  \$？（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非 0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）

3. 应用实例

在一个 shell 脚本中简单使用一下预定义变量
```sh
#!/bin/bash

echo "当前执行的进程 id=$$"

#以后台的方式运行一个脚本，并获取他的进程号

/root/shcode/myshell.sh &

echo "最后一个后台方式运行的进程 id=$!" 
echo "执行的结果是=$?"
```
## 运算符
1. 基本介绍

学习如何在 shell 中进行各种运算操作。

2. 基本语法

1) “\$((运算式))”或“\$[运算式]”或者 ”expr m + n“ //expression 表达式
2) 注意 expr 运算符间要有空格, 如果希望将 expr 的结果赋给某个变量，使用 \`\`
3) expr m - n
4) expr    \\\*, /, %     乘，除，取余（注意乘的写法）

3. 应用实例 oper.sh

案例 1：计算（2+3）* 4 的值
案例 2：请求出命令行的两个参数[整数]的和 20 50
```sh
#!/bin/bash

#案例 1：计算（2+3）X4 的值#使用第一种方式
RES1=$(((2+3)*4))
echo "res1=$RES1"

#使用第二种方式, 推荐使用

RES2=$[(2+3)*4]
echo "res2=$RES2"

#使用第三种方式 expr 
TEMP=`expr 2 + 3` #使用expr时，空格不能少
RES4=`expr $TEMP \* 4` 
echo "temp=$TEMP"
echo "res4=$RES4"

#案例 2：请求出命令行的两个参数[整数]的和 20 50 
SUM=$[$1+$2]
echo "sum=$SUM"
```
## 条件判断
### 判断语句
1. 基本语法
``[ condition ]``注意condition前后要空格，非空返回true，可使用$?验证（0为true，>1为false）。
2. 应用实例
\[ hspEdu \] 返回 true ，条件非空就会返回true
\[  \] 返回 false
\[ condition \] && echo OK || echo notok      条件满足才会执行后面的语句
3. 常用判断条件
|判断情形|语法|
|:--:|:--|
|字符串比较|=|
|两个整数的比较|-lt 小于<br>-le 小于等于，le指little equal<br>-eq 等于<br>-gt 大于<br>-ge 大于等于<br>-ne 不等于|
|按照文件权限进行判断|-r 有读的权限<br>-w 有写的权限<br>-x 有执行的权限|
|按照文件类型进行判断|-f 文件存在并且是一个常规的文件<br>-e 文件存在<br>-d 文件存在并是一个目录|
4. 应用实例
案例 1："ok"是否等于"ok"
判断语句：使用 =

案例 2：23 是否大于等于 22
判断语句：使用 -ge

案例 3：/root/shcode/aaa.txt 目录中的文件是否存在判断语句： 使用 -f
代码如下:
![](https://files.mdnice.com/user/25190/4a557fe8-280e-4769-86c6-36726e51d632.png)
## 流程控制
### if判断
1. 基本语法
```sh
if [ 条件判断式 ] then
代码
fi
```
或者 , 多分支
```sh
if [ 条件判断式 ] then
代码
elif [ 条件判断式 ] then
代码
fi
```
注意事项：[ 条件判断式 ]，中括号和条件判断式之间必须有空格
2. 应用实例
案例：请编写一个shell程序，如果输入的参数，大于等于60，则输出“及格了”，如果小于60，则输出“不及格”
![](https://files.mdnice.com/user/25190/b032ad40-7fa6-4127-a7e6-5d25b417efae.png)
### case语句
1. 基本语法
```sh
case $变量名 in
"值 1")
如果变量的值等于值 1，则执行程序 1
;;
"值 2"）
如果变量的值等于值 2，则执行程序 2
;;
…省略其他分支…
*)
如果变量的值都不是以上的值，则执行此程序
;;
esac
```
2. 应用实例
当命令行参数是1时，输出"周一"，是2时，就输出"周二"， 其它情况输出"other"。
```sh
#!/bin/bash
case $1 in
"1")
echo "周一"
;;
"2")
echo "周二"
;;
"*")
echo "other..."
;;
esac
```
### for循环
1. 基本语法一
```sh
for 变量 in 值1 值2 值3
do
程序/代码
done
```
2. 基本语法二
```sh
for(( 初始值;循环控制条件;变量变化 ))
do
程序/代码
done
```
3. 应用实例
案例1：打印命令行输入的参数 （这里可以看出$* 和 $@ 的区别）
![](https://files.mdnice.com/user/25190/0238eda0-2db1-4d46-be1f-74e81a0e3ef3.png)

案例2：从 1 加到 100 的值输出显示
![](https://files.mdnice.com/user/25190/4e052006-1641-4a2f-94d9-0c01f6ff828a.png)
### while循环
1. 基本语法
```sh
while [ 条件判断式 ]
do
程序 /代码
done
```
注意：while 和 \[有空格，条件判断式和 \[也有空格
2. 应用案例
从命令行输入一个数 n，统计从 1+..+ n 的值是多少？
```sh
#!/bin/bash
#案例 1 ：从命令行输入一个数 n，统计从 1+..+ n 的值是多少？
SUM=0
i=0
while [ $i -le $1 ] do
SUM=$[$SUM+$i]
#i 自增
i=$[$i+1]
done
echo "执行结果=$SUM"
```
## read读取控制台输入
1. 基本语法
``read(选项)(参数)``

|选项|说明|
|:--:|:--|
|-p|指定读取值的提示符|
|-t|指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了。。|
参数：变量（指定读取的变量名）。
2. 应用实例
案例 1：读取控制台输入一个 NUM1 值
案例 2：读取控制台输入一个 NUM2 值，在 10 秒内输入。
```sh
#!/bin/bash

#案例 1：读取控制台输入一个 NUM1 值
read -p "请输入一个数 NUM1=" NUM1 
echo "你输入的 NUM1=$NUM1"

#案例 2：读取控制台输入一个 NUM2 值，在 10 秒内输入。

read -t 10 -p "请输入一个数 NUM2=" NUM2 
echo "你输入的 NUM2=$NUM2"
```
## 函数
### 函数介绍
shell 编程和其它编程语言一样，有系统函数，也可以自定义函数。系统函数中，我们这里就介绍两个。
### 系统函数
1.1. basename 基本语法
功能：返回完整路径最后的/部分，常用于获取文件名
```sh
basename [pathname] [suffix]
basename [string] [suffix]
```
功能描述：basename命令会删掉所有的前缀包括最后一个"/"字符，然后将字符串显示出来。
选项：suffix为后缀，如果suffix被指定了，basename会将pathname或string 中的 suffix 去掉。

1.2. 应用案例
案例 1：请返回 /home/aaa/test.txt 的 "test.txt" 部分
``basename /home/aaa/test.txt``

2.1. dirname 基本语法
功能：返回完整路径最后 / 的前面的部分，常用于返回路径部分。
``dirname 文件绝对路径`` （功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分））

2.2. 应用实例
案例 1：请返回 /home/aaa/test.txt 的 /home/aaa 
``dirname /home/aaa/test.txt``

### 自定义函数
1. 基本语法
```sh
[ function ] funname[()]
{
Action;
[return int;]
}
```
调用直接写函数名：``funname [值]``

2. 应用实例
案例 1：计算输入两个参数的和(动态的获取)， getSum
```sh
#!/bin/bash

#案例 1：计算输入两个参数的和(动态的获取)， getSum
#定义函数 getSum 
function getSum() {

SUM=$[$n1+$n2] echo "和是=$SUM"

}

#输入两个值

read -p "请输入一个数 n1=" n1 read -p "请输入二个数 n2=" n2 #调用自定义函数

getSum $n1 $n2
```

## shell编程综合案例
### 需求分析
1) 每天凌晨 2:30 备份 数据库 hspedu 到 /data/backup/db
2) 备份开始和备份结束能够给出相应的提示信息
3) 备份后的文件要求以备份时间为文件名，并打包成 .tar.gz 的形式，比如：2021-03-12_230201.tar.gz
4) 在备份的同时，检查是否有 10  天前备份的数据库文件，如果有就将其删除。
5) 画一个思路分析图
![](https://files.mdnice.com/user/25190/10657a2f-fe9a-4c33-be76-2dd951b3c239.png)
### 代码 /usr/sbin/mysql_db.backup.sh
```sh
#备份目录BACKUP=/data/backup/db #当前时间

DATETIME=$(date +%Y-%m-%d_%H%M%S)

echo $DATETIME #数据库的地址HOST=localhost

#数据库用户名DB_USER=root #数据库密码

DB_PW=hspedu100 #备份的数据库名DATABASE=hspedu

#创建备份目录, 如果不存在，就创建

[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"
#备份数据库

mysqldump    -u${DB_USER}    -p${DB_PW}    --host=${HOST}                             -q                        -R                       --databases             ${DATABASE}                       |                               gzip                    >

${BACKUP}/${DATETIME}/$DATETIME.sql.gz

#将文件处理成 tar.gz cd ${BACKUP}

tar -zcvf $DATETIME.tar.gz ${DATETIME} #删除对应的备份目录

rm -rf ${BACKUP}/${DATETIME}

#删除 10 天前的备份文件

find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \; echo "备份数据库${DATABASE} 成功~"
```