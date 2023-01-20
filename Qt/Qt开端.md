## Qt简介
Qt 是一个著名的 C++ 应用程序框架。你并不能说它只是一个 GUI 库，因为 Qt 十分庞  大，并不仅仅是 GUI 组件。使用 Qt，在一定程度上你获得的是一个“一站式”的解决方案：  不再需要研究 STL，不再需要 C++ 的<string>，不再需要到处去找解析 XML、连接数据库、访问网络的各种第三方库，因为 Qt 自己内置了这些技术。

Qt 和 wxWidgets 一样，也是一个标准的 C++ 库。但是它的语法类似于 Java 的 Swing，十分清晰，而且使用信号槽（signal/slot）机制，让程序看起来很明白——这也是很多人优先选择 Qt 的一个很重要的原因。不过，所谓“成也萧何，败也萧何”。这种机制虽然很清楚，但是它所带来的后果是你需要使用 Qt 的 moc 对程序进行预处理，才能够再使用标准的  make 或者 nmake 进行正常的编译，并且信号槽的调用要比普通的函数调用慢大约一个数量级（Qt 4 文档中说明该数据，但 Qt 5 尚未有官方说明）。 Qt 的界面也不是原生风格的，  尽管 Qt 使用 style 机制十分巧妙地模拟了原生界面。另外值得一提的是， Qt 不仅仅能够运行在桌面环境中，还可以运行在嵌入式平台以及手机平台。

## Qt的安装
### [在安装Qt之前该选择哪个版本呢？](https://www.cnblogs.com/chinasoft/p/15226293.html)
Qt 6 已经在2020年12月8日发布了。  但你没有看错，这篇是谈 Qt 5 攻略。  
毕竟 Qt 6 在 Win 平台将只支持 Win10 及其以上。所以大批 Win7、XP 党 无缘 Qt 6。而且 Qt6 为了赶进度，早期版本里缺少了很多模块，例如图表、数据可视化、WebEngine，所以也没必要急着尝鲜 Qt 6 ，建议等完整版出来后，再升级也不迟。  
  
那么 Qt 5 的各个版本，该如何选择呢？  
Qt 5.9 作为LTS也已经在2020年5月31日停止更新了，所以建议使用目前依旧在更新的LTS：5.12 与 5.15。  
能直接使用 Qt 5.15 当然是最好的，若实在有难处，就用 5.12 吧，但至少别再用 Qt 5.9 之前的版本了。  
  
如果你需要用到 QtWebkit，则只能用 Qt5.5及其以前的版本。  
如果你需要 PDF 的支持，建议升级至 5.15，因为新增模块 Qt PDF  
如果你需要 SSL 的支持，建议升级至 5.15，因为 5.13 开始已自支持 OpenSSL 1.1 及其以上。  
如果你常用 QImage，建议升级至 5.15，因为缩放和转换的许多方法都升级成多线程的。  
如果你常用 QtQuick，建议升级至 5.15，因为 5.14 开始 QtQuick 不再局限于 OpenGL 引擎加速。  
如果你常用 QNetworkAccessManager，建议升级至 5.15，因为开始支持超时设置 setTransferTimeout  
如果你发行在 Windows 平台，建议升级至 5.15，因为 5.14 开始对高DPI的设备有更好的支持。  
如果你需要开发 安卓APP，建议升级至 5.15，因为该版本完善了安卓开发文档。  
  
特别说明  
已自支持 OpenSSL 是很实用很实用的。  
QNetworkAccessManager 的 setTransferTimeout 超时设置 是很实用很实用的。  
现在很多设备，特别是笔记本，都是高分屏设备，对高分屏的良好支持是 5.14 开始的。  
对触摸屏设备的良好支持，是 Qt 5.12 开始的。  
有些高富帅设备，既是高分屏，又是触摸屏，例如 surface ，那必须 5.14 至少。  
有些对话框的标题栏，会出现“?”按钮，叫“这是什么”的提示，很讨厌。 5.10 支持移除。

**最终我选择了5.15.2版本**，.2指补丁到了2。

### Qt的安装
从 Qt5.15 开始，官方网站上已经不再提供离线安装包了，也就是说，官方建议我们使用 Qt 维护工具（ Qt Maintenance Tool）在线安装 Qt。
具体Qt的安装流程参见([Qt6 安装 | 爱编程的大丙 (subingwen.cn)](https://subingwen.cn/qt/qt6-install/))。

MaintenanceTool.exe ，在QT一级安装目录下，使用此工具可以管理开发环境组件和升级组件。然而对于离线安装包，它只能用于删除软件包。

### Qt安装目录的结构
[解密Qt安装目录的结构 (biancheng.net)](http://c.biancheng.net/view/3866.html)
## Qt用到的开发工具
### GNU工具集
GNU 项目是为了创建自由的类 Unix 系统，（Linux就被称为类Unix系统）也因此开发出来很多开源的系统工具，其中非常著名的就是 [GCC](http://c.biancheng.net/gcc/) （GNU Compiler Collection，GNU编译器套件）。

> 现在我们知道，GUN 开发类 Unix 系统的项目失败了，但是它开发的一系列工具集却用到了后来的 Linux 内核上，两者结合形成了今天的各种 Linux 发行版，有兴趣的读者请转到《[Linux和UNIX的关系及区别](http://c.biancheng.net/view/707.html)》了解更多。

在 GNU 工具集里面，开发时常见到的几个罗列如下（这些工具通常位于 Linux 或 Unix 系统里的 /usr/bin/ 目录）：  
  
|工具|说明|
|:----|:--|
|gcc|GNU C 语言编译器。|
|g++|GNU [C++](http://c.biancheng.net/cplus/) 语言编译器。|
|ld|GNU 链接器，将目标文件和库文件链接起来，创建可执行程序和动态链接库。|
|ar|生成静态库 .a ，可以编辑和管理静态链接库。|
|make|生成器，可以根据 makefile 文件自动编译链接生成可执行程序或库文件。|
|gdb|调试器，用于调试可执行程序。|
|ldd|查看可执行文件依赖的共享库（扩展名 .so，也叫动态链接库）。|

### MinGW
原本 GNU 工具只在 Linux/Unix 系统里才有，随着 Windows 系统的广泛使用， 为了在 Windows 系统里可以使用 GNU 工具，诞生了 MinGW（Minimalist GNU for Windows） 项目，利用 MinGW 就可以生成 Windows 里面的 exe 程序和 dll 链接库。  
  
需要注意的是，MinGW 与 Linux/Unix 系统里 GNU 工具集的有些区别：

-   MinGW 里面工具带有扩展名 .exe， Linux/Unix 系统里工具通常都是没有扩展名的。
-   MinGW 里面的生成器文件名为 mingw32-make.exe，Linux/Unix 系统里就叫 make。
-   MinGW 在链接时是链接到 *.a 库引用文件，生成的可执行程序运行时依赖 *.dll，而 Linux/Unix 系统里链接时和运行时都是使用 *.so 。

  
另外 MinGW 里也没有 ldd 工具，因为 Windows 不使用 .so 共享库文件。如果要查看 Windows 里可执行文件的依赖库，需要使用微软自家的 Dependency Walker 工具。Windows 里面动态库扩展名为 .dll，MinGW 可以通过 dlltool 来生成用于创建和使用动态链接库需要的文件，如 .def 和 .lib。  
  
MinGW 原本是用于生成 32 位程序的，随着 64 位系统流行起来， 从 MinGW 分离出来了 MinGW-w64 项目，该项目同时支持生成 64 位和 32 位程序。Qt 的 MinGW 版本库就是使用 MinGW-w64 项目里面的工具集生成的。

#### MSYS（Minimal SYStem）

另外提一下，由于 MinGW 本身主要就是编译链接等工具和头文件、库文件，并不包含系统管理、文件操作之类的 Shell 环境， 这对希望用类 Unix 命令的开发者来说还是不够用的。 所以 MinGW 官方又推出了 MSYS（Minimal SYStem），相当于是一个部署在 Windows 系统里面的小型 Unix 系统环境， 移植了很多 Unix/Linux 命令行工具和配置文件等等，是对 MinGW 的扩展。  
  
MSYS 对于熟悉 Unix/Linux 系统环境或者要尝试学习 Unix/Linux 系统的人都是一种便利。MSYS 和 MinGW 的安装升级都是通过其官方的 mingw-get 工具实现，二者是统一下载安装管理的。  
  
对于 MinGW-w64 项目，它对应的小型系统环境叫 MSYS2（Minimal SYStem 2），MSYS2 是 MSYS 的衍生版，不仅支持 64 位系统和 32 位系统，还有自己的独特的软件包管理工具，它从 Arch Linux 系统里移植了 pacman 软件管理工具，所以装了 MSYS2 之后，可以直接通过 pacman 来下载安装软件，而且可以自动解决依赖关系、方便系统升级等。装了 MSYS2 之后，不需要自己去下载 MinGW-w64，可以直接用 pacman 命令安装编译链接工具和 git 工具等。  
  
MinGW 项目主页（含 MSYS）： [http://www.mingw.org/](http://www.mingw.org/)  
MinGW-w64 项目主页： [https://sourceforge.net/projects/mingw-w64/](https://sourceforge.net/projects/mingw-w64/)  
MSYS2 项目主页： [https://sourceforge.net/projects/msys2/](https://sourceforge.net/projects/msys2/)

### CMake

CMake（Cross platform Make）是一个开源的跨平台自动化构建工具， 可以跨平台地生成各式各样的 makefile 或者 project 文件， 支持利用各种编译工具生成可执行程序或链接库。  
  
CMake 自己不编译程序， 它相当于用自己的构建脚本 CMakeLists.txt，叫各种编译工具集去生成可执行程序或链接库。  
  
一般用于编译程序的 makefile 文件比较复杂，自己去编写比较麻烦， 而利用 CMake ，就可以编写相对简单的 CMakeLists.txt ，由 CMake 根据 CMakeLists.txt 自动生成 makefile，然后就可以用 make 生成可执行程序或链接库。  
  
本教程里面是使用 Qt 官方的 qmake 工具生成 makefile 文件，没有用 CMake。这里之所以提 CMake，是因为整个 KDE 桌面环境的茫茫多程序都是用 CMake 脚本构建的，另外跨平台的程序/库如 Boost C++ Libraries、[OpenCV](http://c.biancheng.net/opencv/)、LLVM、Clang 等也都是用 CMake 脚本构建的。以后如果接触到这些东西，是需要了解 CMake 的。  
  
CMake 项目主页：[https://cmake.org/](https://cmake.org/)  
KDE 项目主页：[https://www.kde.org/](https://www.kde.org/)

### Qt 工具集

Qt 官方的开发环境安装包里有自己专门的开发工具，之前用过 qmake 命令。qmake 是 Qt 开发最核心的工具，既可以生成 Qt 项目文件 .pro ，也可以自动生成项目的 Makefile 文件。  
  
这里将常用的 Qt 开发工具列表如下：

|工具|说明|
|:--|:--|
|qmake|	核心的项目构建工具，可以生成跨平台的 .pro 项目文件，并能依据不同操作系统和编译工具生成相应的 Makefile，用于构建可执行程序或链接库。|
|uic|	User Interface Compiler，用户界面编译器，Qt 使用 XML 语法格式的 .ui 文件定义用户界面，uic 根据 .ui 文件生成用于创建用户界面的 C++ 代码头文件，比如 ui_*****.h 。|
|moc	|Meta-Object Compiler，元对象编译器，moc 处理 C++ 头文件的类定义里面的 Q_OBJECT 宏，它会生成源代码文件，比如 moc_*****.cpp ，其中包含相应类的元对象代码，元对象代码主要用于实现 Qt 信号/槽机制、运行时类型定义、动态属性系统。|
|rcc	|Resource Compiler，资源文件编译器，负责在项目构建过程中编译 |.qrc |资源文件，将资源嵌入到最终的 Qt 程序里。|
|qtcreator|	集成开发环境，包含项目生成管理、代码编辑、图形界面可视化编辑、 编译生成、程序调试、上下文帮助、版本控制系统集成等众多功能， 还支持手机和嵌入式设备的程序生成部署。|
|assistant|	Qt 助手，帮助文档浏览查询工具，Qt 库所有模块和开发工具的帮助文档、示例代码等都可以检索到，是 Qt 开发必备神器，也可用于自学 Qt。|
|designer|	Qt 设计师，专门用于可视化编辑图形用户界面（所见即所得），生成 .ui 文件用于 Qt 项目。|
|linguist|	Qt 语言家，代码里用 tr() 宏包裹的就是可翻译的字符串，开发人员可用 lupdate 命令生成项目的待翻译字符串文件 .ts，用 linguist 翻译多国语言 .ts ，翻译完成后用 lrelease 命令生成 .qm 文件，然后就可用于多国语言界面显示。|
|qmlscene|	在 Qt 4.x 里是用 qmlviewer 进行 QML 程序的原型设计和测试，Qt 5 用 qmlscene 取代了旧的 qmlviewer。新的 qmlscene 另外还支持 Qt 5 中的新特性 scenegraph 。|

## Qt编程涉及的术语和名词
[Qt编程涉及的术语和名词 (biancheng.net)](http://c.biancheng.net/view/3871.html)