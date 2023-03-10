## Vi和Vim编辑器的介绍
Linux系统会内置vi文本编辑器，相当于Windows当中的记事本。Vim是Vi的增强版，二者的使用几乎相同，Vim具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补完、编译及错误跳转等方便编程功能特别丰富，在程序员中广泛使用。

## Vi和Vim常用的三种模式
1. 正常模式
以vim打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中，你可以使用 「. 上下左右」按键来移动光标，你可以使用「删除字符」或「删除整行」来处理档案内容，也可以使用 「复制、粘贴」来处理你的文件数据。
2. 插入模式
按下i，I，o，O，a，A，r，R等任何一个字母之后才会进入编辑模式，一般来说按i即可。
3. 命令行模式
按下Esc键，在输入”:“，在这个模式当中可以提供你相关指令，完成读取，存盘，替换，离开Vim，显示行号等的动作都是在此模式中达成的。

![三种模式之间的切换](https://files.mdnice.com/user/25190/98551baa-9b93-4796-afce-0bc6e68fb313.png)

## Vi和Vim的快捷键
1. 拷贝当前行：一般模式下输入``yy``，拷贝当前行向下的5行：一般模式下输入``5yy``，并粘贴：``p``
2. 删除当前行：一般模式下输入``dd``，删除当前行向下的5行：一般模式下输入``5dd``
3. 在文件中查找某个单词：命令行模式下：``/关键词``，回车查找，输入``n``就是查找下一个
4. 设置文件的行号，取消文件的行号：命令行模式下输入``:set nu``和``set nonu``
5. 到文件的最末行和最首行：一般模式下输入``G``和``gg``
6. 在一个文件中输入”hello“，又想撤销这个动作：在一般模式下输入``u``
7. 将光标移动到指定行：一般模式下输入``20``+``shift+g``便会跳转到20行
8. 快捷键的键盘对应图

![快捷键的键盘对应图](https://files.mdnice.com/user/25190/0d200c0a-8558-45ae-a6c6-71592bce76bd.png)