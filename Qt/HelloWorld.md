## [新建项目](http://c.biancheng.net/view/1817.html "编写第一个Qt程序 ")
1. 点击文件->new project，进行新建。
Qt Creator 可以创建多种项目，在最左侧的列表框中单击“Application”，中间的列表框中列出了可以创建的应用程序的模板，各类应用程序如下：

-   Qt Widgets Application，支持桌面平台的有图形用户界面（Graphic User Interface，GUI） 界面的应用程序。GUI 的设计完全基于 C++ 语言，采用 Qt 提供的一套 C++ 类库。
-   Qt Console Application，控制台应用程序，无 GUI 界面，一般用于学习 C/C++ 语言，只需要简单的输入输出操作时可创建此类项目。
-   Qt Quick Application，创建可部署的 Qt Quick 2 应用程序。Qt Quick 是 Qt 支持的一套 GUI 开发架构，其界面设计采用 QML 语言，程序架构采用 C++ 语言。利用 Qt Quick 可以设计非常炫的用户界面，一般用于移动设备或嵌入式设备上无边框的应用程序的设计。
-   Qt Quick Controls 2 Application，创建基于 Qt Quick Controls 2 组件的可部署的 Qt Quick 2 应用程序。Qt Quick Controls 2 组件只有 Qt 5.7 及以后版本才有。
-   Qt Canvas 3D Application，创建 Qt Canvas 3D QML 项目，也是基于 QML 语言的界面设计，支持 3D 画布。

2. 定义项目名称和路径
选择一个目录，如“E:\QtDemo”，再设置项目名称为 Demo， 这样新建项目后，会在“E:\QtDemo”目录下新建一个目录，项目所有文件保 存在目录“E:\QtDemo\Demo\”下。

3. 选择编译工具
可以将这几个编译工具都选中，在编译项目时再选择一个作为当前使用的编译工具，这样可以编译生成不同版本的可执行程序。

4. 选择需要创建界面的基类（base class）。有 3 种基类可以选择：  
*  QMainWindow 是主窗口类，主窗口具有主菜单栏、工具栏和状态栏，类似于一般的应用程序的主窗口；
* QWidget 是所有具有可视界面类的基类，选择 QWidget 创建的界面对各种界面组件都可以支持；
* QDialog 是对话框类，可建立一个基于对话框的界面；

  
在此选择 `QMainWindow` 作为基类，自动更改的各个文件名不用手动去修改。勾选“创建界面”复选框。这个选项如果勾选，就会由 Qt Creator 创建用户界面文件，否则，需要自己编程手工创建界面。为编写简易的程序，在此先不勾选“创建界面”选项。
  
然后单击“Next”按钮，出现一个页面，总结了需要创建的文件和文件保存目录，单击“完成”按钮就可以完成项目的创建。

修改``main.cpp``如下
```cpp
#include <QApplication>  
#include <QLabel>  
int main(int argc, char *argv[])  
{  
	QApplication app(argc, argv);  
	QLabel label("Hello, world");  
	label.show();  
	return app.exec();  
}
```
前两行是 C++ 的 include 语句，这里我们引入的是QApplication以及QLabel这两个类。  main()函数中第一句是创建一个 QApplication 类的实例。对于 Qt 程序来说， main()函数一般以创建 application 对象（GUI 程序是QApplication，非 GUI 程序是QCoreApplication。 QApplication 实际上是 QCoreApplication 的子类。）开始，后面才是实际业务的代码。这个对象用于管理 Qt 程序的生命周期，开启事件循环，这一切都是必不可少的。在我们创建了 QApplication 对象之后，直接创建一个 QLabel 对象，构造函数赋值“Hello, world”，当然就是能够在 QLabel上面显示这行文本。最后调用 QLabel的 show()函数将其显示出来。 main()函数最后，调用 app.exec()，开启事件循环。我们现在可以简单地将事件循环理解成一段无限循环。正因为如此，我们在栈上构建了 QLabel 对象，却能够一直显示在那里（试想，如果不是无限循环， main()函数立刻会退出， QLabel 对象当然也就直接析构了）。

## 编译调试工具栏的作用

|图标|作用|快捷键|
|:--|:--|:--|
|![](http://c.biancheng.net/uploads/allimg/181228/2-1Q22Q52043426.gif)|弹出菜单选择编译工具和编译模式，如 Debug或 Release模式|
|![](http://c.biancheng.net/uploads/allimg/181228/2-1Q22Q52144152.gif)|直接运行程序，如果修改后未编译，会先进行编译。即使在程序中设置了断点，此方式运行的程序也无法调试。|Ctrl+R|
|![](http://c.biancheng.net/uploads/allimg/181228/2-1Q22Q52211343.gif)|项目需要以Debug模式编译，点此按钮开始调试运行，可以在程序中设置断点。若是以 Release模式编译，点此按钮也无法进行调试。|F5|
|![](http://c.biancheng.net/uploads/allimg/181228/2-1Q22Q52230b6.gif)|编译当前项目|Ctrl+B|

在 Qt Creator 中也可以对程序设置断点进行调试，但是必须以 Debug 模式编译，并以“Start Debugging”（快捷键 F5）方式运行程序。