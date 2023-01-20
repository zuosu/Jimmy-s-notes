## QFile类
在widget.ui中创建一个lineEdit、pushButton和一个textEdit。如下图所示

![](https://files.mdnice.com/user/25190/a5e68a98-c015-4c32-b19a-249c83823e46.png)

在widget.cpp中编辑如下代码：

```C++
#include "widget.h"
#include "./ui_widget.h"

#include <QPushButton>
#include <QFileDialog>
#include <QLineEdit>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    resize(600,400);

    connect(ui->pushButton,&QPushButton::clicked,this,[=](){
        QString path = QFileDialog::getOpenFileName(this,"打开文件","D:\\Projects\\Qt\\widgetDemo");
        ui->lineEdit->setText(path);

        QFile file(path);

        file.open(QIODevice::ReadOnly);

		//一次性全部读取
//        QByteArray array = file.readAll();
        QByteArray array;
        while(!file.atEnd())
        {
            array+=file.readLine();
        }
        //将读取到的数据放入textEdit中
        ui->textEdit->setText(array);

        //对文件对象进行关闭
        file.close();

        //进行写文件
        file.open(QIODevice::Append);//追加的方式打开
        file.write("\n dhaio上帝啊哈才能");//追加字符串\n表示回车
        file.close();
    });
}

Widget::~Widget()
{
    delete ui;
}
```
## QFileInfo类
在上述widget.cpp中添加如下代码
```C++
#include <QDebug>
#include <QDateTime>

QFileInfo info(path);

        qDebug()<<"大小："<<info.size()<<"后缀名:"<<info.suffix()<<"文件名称："<<info.fileName()<<"文件路径"<<info.absoluteFilePath();

        qDebug()<<"创建日期:"<<info.birthTime().toString("yyyy/MM/dd hh:mm:ss");

        qDebug()<<"最后修改日期："<<info.lastModified().toString("yyyy-MM-dd hh:mm:ss");
```

输出结果为：
```
大小： 4706 后缀名: "txt" 文件名称： "test.txt" 文件路径 "D:/Projects/Qt/widgetDemo/test.txt"
创建日期: "2022/07/17 11:47:19"
最后修改日期： "2022-07-17 22:59:26"
```
