### 什么是QT
* QT是一个跨平台的C++图像用户界面应用程序框架
* QT在1991年由奇趣科技开发
* QT的优点
    + 跨平台,几乎支持所有平台
    + 接口简单，容易上手
    + 一定程度上简化了内存回收机制
    + 有很好的社区氛围
    + 可以进行嵌入式开发

# QWidget
### QT注意事项
* 命名规范
    + 类名 首字母大写，单词和单词之间首字母大写
    + 函数名 变量名称 首字母小写,单词和单词之间首字母大写

* 快捷键
    + 注释 ctrl + /
    + 运行 ctrl + r
    + 编译 ctrl + b
    + 查找 ctrl + f
    + 帮助文档 F1
    + 自动对齐 ctrl + i
    + 同名的.h和.cpp切换 F4

### 按钮
* 按钮常用API
1. show() 以顶层方式弹出窗口控件
2. setParent() 选择依赖方式
3. setText() 设置文本
3. resize() 重置窗口大小
4. move() 移动
5. setWindowTitle() 设置窗口大小
6. setFixedSize() 设置固定窗口大小

### QT中的对象树
1. 当创建的对象在堆区的时候,如果指定的付钱是QObject 派生下来的类或者子类
2. 派生下来的类,可以不需要管理释放操作,会将对象放入对象树
一定程度上简化了内存回收机制

### QT的窗口坐标系
1. 笛卡尔坐标系[左上角为0,0点]

### QT信号和槽
* connect( 信号的发送者 ,信号的具体信息, 信号的接受者,信号的处理[槽])
* 信号槽的优点 松散耦合
    + 信号发送端 和 接收端本身是没有关联的,通过connectl连接,将两者耦合在一起
    + 信号关键字：Signals
        * chlicked(bool) 点击
        * pressed() 按下
        * released() 释放
        * toggled(bool) 切换状态
    + 槽的关键字：Slots
* 自定义信号和槽位函数
    + 自定义信号
        * 写在类的signals下,返回值为void,可以有参数,支持重载,不需要实现
    + 自定义槽函数
        * 不能写在signals下,public slots[公共的槽函数] 5.4版本以后全局函数或者public都行
        * 返回值也是void,需要声明,也需要实现,可以有参数,支持重载
    + 然后用connect连接信号和槽
    + 触发信号 emit
    + 信号和槽重载，需要函数指针，明确指向函数的地址
    + QString 转char * 使用.toUtf8().data()
    + 信号和槽连接：触发这个信号才能触发槽
        * 一个信号可以连接多个槽
        * 多个信号也可以连接同一个槽函数
        * 信号和槽的参数和类型必须对应
        * 信号的参数个数可以多于槽的参数个数
    + 信号和信号连接 触发一个信号也能触发另外一个信号
    + 断开信号 disconnect(参数一样)

### Lambda表达式
* C++11版本特性 [CONFIG += c++11] 匿名函数对象
    * Lambda表达式函数声明 [](){}
        + [=] 允许使用局部变量
        + [&] 允许使用引用传递变量
        + [变量] 允许变量使用值传递
        + mutable 可修改值传递进来的参数[虽然还是局部变量]
            + [m]()mutable{m+=100;打印}; 不加mutable会报错
        + ->类型 带返回值
            + int ret = []()->int{return 1000}();
    * Lambda表达式函数调用 [](){}()
    * 最常见的[=](){}

# MainWindow
### 菜单栏 QMenuBar
    * 菜单栏最多只能有一个
        + QMenuBar * bar = menuBar(); setMenuBar(bar);
        + 创建菜单
            * QMenu * fileMenu = bar->addMenu("文件");
                + 创建菜单栏目
                    * QAction * newAction =  fileMenu->addAction("新建");
                + 添加分隔符 
                    * fileMenu->addSeparator();

### 工具栏 QToolBar
    * 工具栏可以有多个
        + QToolBar * toolBar = new QToolBar(this);  
        + addToolBar(toolBar);
            * 可选参数 默认停靠范围
                + addToolBar(Qt::BottomToolBarArea,toolBar);
            * 只允许左右停靠
                + toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea);
            * 取消浮动
                + toolBar->setFloatable(false);
            * 设置禁止移动
                + toolBar->setMovable(false);
            * 给工具栏设置栏目
                + toolBar->addAction("绝了"或者QAction);
            * 给工具栏添加控件
                + toolBar->addWidget(QPushButton按钮);

### 状态栏 QStatusBar
    * 状态栏最多只能有一个
        + QStatusBar * stBar = statusBar();
        + setStatusBar(stBar);
            * 添加标签控件
                + QLabel * label = new QLabel("左侧提示的信息",this);
                + QLabel * label1 = new QLabel("右侧提示的信息",this);
                + stBar->addWidget(label);
                + stBar->addPermanentWidget(label1);

### 铆接部件 QDockWidget
    * 铆接部件可以有多个
        + QDockWidget * dockWidget = new QDockWidget("浮动",this);
        + addDockWidget(Qt::BottomDockWidgetArea,dockWidget); 放置位置下面 如果没有中心部件默认占满
            + 只允许上下
                    * dockWidget->setAllowedAreas(Qt::TopDockWidgetArea | Qt::BottomDockWidgetArea);  

### 中心部件
    * 中心内容也只能有一个
        + 文本窗口 QTextEdit
            + QTextEdit * edit = new QTextEdit(this);
            + setCentralWidget(edit); //设置中心部件

### 资源文件
1. 将图片文件文件夹拷贝到项目下
2. 右键项目->添加新文件->Qt->Qt recourse File
3. res 生成 res.qrc
4. 右键res.qrc->open in editor 编辑资源
5. 添加前缀 添加文件
6. 使用 ": + 前缀名 + 文件名"

### 小总结
    + 只能有一个的是set 可以允许多个是add

### 对话框
+ 模态对话框 不可以对其他窗口进行操作
    * QDialog dlg(this);
    * dlg.exec();
    * 消息对话框
        + 错误对话框 QMessageBox::critical(this,"critical","错误");
        + 信息对话框 information
    QMessageBox::information(this,"information","Open Success!",QMessageBox::Ok,QMessageBox::Ok);
        + 提问对话框 question
        QMessageBox::question(this,"Question","是否打开？",QMessageBox::Open|QMessageBox::Close,QMessageBox::Open);
        + 警告对话框warning
        QMessageBox::warning(this,"Warning","是否打开？",QMessageBox::Open|QMessageBox::Close,QMessageBox::Open);
        + 颜色对话框
            + QColor color = QColorDialog::getColor(QColor(255,0,0));qDebug()<<color.red()<<color.blue()<<color.green();
        + 文件对话框 最后一个是过滤，返回值是选取的路径
            + QString str = QFileDialog::getOpenFileName(this,"打开文件","./","(*.cpp)");
        + 字体对话框
            + bool flag;
            + QFont font = QFontDialog::getFont(&flag,QFont("华文彩云",12));
            + setFont(font);//设置字体
+ 非模态对话框 可以对其他窗口进行操作
    * QDialog *dlg2 = new QDialog(this); //为了确保不释放,开在堆上
    * dlg2->show();
    * dlg2->setAttribute(Qt::WA_DeleteOnClose);//55号 用于按关闭键自动释放[QWidge的对象树是在关闭总的窗口才会全部释放]

### 列表控件 listWidget
+ QListWidgetItem * item = new QListWidgetItem("锄禾日当午");
+ ui->listWidget->addItem(item); //添加进去
+ item->setTextAlignment(Qt::AlignCenter); //居中

### ui窗口自布局
1. Spacers 弹簧 Widget div盒子
2. Group Box 分组[适用于Radio Button]
3. 主窗口设置垂直布局后可以在sizePolicy->垂直策略->Fixed来使组件高度合适
4. 如果找不到某个组件的信号或者槽，找基类

### 自定义组件
1. add new -> 设计师类
2. 使用自定义组件
    * 查看基类[如widget] 从界面库中拖出来一个widget组件,然后点击提升为,写入类名
        + [设置全局后可以直接在右键中显示]
        3 自定义组件只有同基类才能被提升

### QT事件 QEvent 
* 鼠标事件
    + 事件是虚函数,可以进行重载  
    //鼠标进入事件  
    virtual void enterEvent(QEvent *event);  
    //鼠标离开事件  
    virtual void leaveEvent(QEvent *event);  
    //鼠标按下  
    virtual void mouseReleaseEvent(QMouseEvent *ev);  
    //鼠标释放  
    virtual void mousePressEvent(QMouseEvent *ev);  
    //鼠标移动  
    virtual void mouseMoveEvent(QMouseEvent *ev);  

* 定时器 QTimeEvent
    + 利用事件实现定时器
        + startTimer(1000); 启动定时器，单位毫秒,返回一个唯一定时器id
        + void timerEvent(QTimerEvent * ev)
            * 定时器函数,可以通过ev->timerId()== id1来判断当前是哪个id进来的
    + 定时器类QTimer
        +   
        //通过定时器类  
        QTimer * timer = new QTimer(this);  
        //启动定时器 每隔500秒发一个信号  
        timer->start(500);  
        //连接信号  
        connect(timer,&QTimer::timeout,中括号小括号{  
        static int num = 1;  
        ui->label_5->setText(QString::number(num++));  
        });  

* event事件分发器
    + bool event(QEvent * ev)
        * 返回值是bool类型，如果返回true，代表用户要处理这个事件,不向下分发事件了[类似于钩子]
    + 事件枚举QEvent
        * ev.type();
        * 拦截后使用子类的操作可以使用静态类型转换
            + QMouseEvent *ev = static_cast<QMouseEvent *>(QEvent中行参);
    + 但是尽量别拦截

* 事件过滤器
    + 在app到事件分发器前还能做个过滤
    + 使用方式
        + 给控件安装时间过滤器
            + installEventFilter(this);
        + 重写eventfilter事件

### 绘图 QPainter 
+ 绘图事件 void paintEvent(QPaintEvent *)
+ 画家类 QPainter(构图的设备)
    + 拿起笔 .setPen(笔)
    + 拿起刷子 .setBrush(刷子)
+ 画笔类 QPen(笔的颜色)
+ 画刷类 QBrush(笔的颜色)
+ 高级操作
    + 效率降低的抗锯齿
        + painter.setRenderHint()
    + 改变画家位置
        + painter.save();保存当前位置
        + painter.restore(); 还原到保存的位置
        + painter.translate(); 移动画家
    + 画家绘制图片drawPixmap

### 绘图设备
+ QPixmap 专门对图像显示做了优化
+ QBitmap 色深限定为1
+ QImage 专门为图像的像素级访问做了优化
+ QPicture 可以记录和重视画家的QPainter的各类命令
    + 自定义绘图操作 


### 文件读写 QFile
+ file.open(打开方式) QtODevice::readOnly
+ 全部读取 file.readAll() 按行读 file.readLine() 判断文件末尾atend()
+ QFile默认支持的是utf-8 指定格式 QTextCodec
    + QTextCodec *codec = QTextCodec::codecForName("gbk");
    + ui->textEdit->setText(codec->toUnicode(array)); 
+ 关闭文件对象 file.close();

### 文件信息 QFileInfo
+ QFileInfo info(path);
+ 后缀名 info.suffix()
+ 创建日期 info.birthTime().toString("yyyy/MM/dd hh:mm:ss");
+ 修改日期 info.lastModified().toString("yyyy/MM/dd hh:mm:ss");

### Qss 前端人狂喜
+ #myButton 这里的id实际上就是objectName指定的值
+ 伪状态
    + :active 当小部件驻留在活动窗口中时，将设置此状态
    + :checked	该控件被选中时候的状态
    + :hover	鼠标在控件上方
    + :pressed	该控件被按下时的状态
    + :disabled	该控件禁用时的状态
    + :first	该控件是第一个（列表中）
    + :focus	该控件有输入焦点时

### 动画 QPropertyAnimation
//winLabel 你要对那个组件使用动画  geometry几何结构  
QPropertyAnimation * an = new QPropertyAnimation(winLabel,"geometry");  
//动画时间  
an->setDuration(1000);  
//动画开始
an->setStartValue(QRect(winLabel->x(),winLabel->y(),winLabel->width(),winLabel->height()));  
//动画结束  
an->setEndValue(QRect(winLabel->x(),winLabel->y() + 300,winLabel->width(),winLabel->height()));  
//动画方式  
an->setEasingCurve(QEasingCurve::OutBounce);  
an->start();  

### 背景音乐 QSound
* qmake: QT += multimedia
* QSound * startSound = new QSound(":/res/TapButtonSound.wav",this); 载入音效
* startSound->play(); 播放
* startSound->setLoops(-1); -1循环次数无限

### 打包发布
* debug->release
* 运行 运行失败添加环境变量D:\QT\5.12.3\mingw73_64\lib
* 把 Goldreverse.exe 单独丢到一个文件夹下
* cmd中路径后windeployqt .\Goldreverse.exe 运行
* 此时已经可以使用了
* 深入打包[hm nis edit][https://www.bilibili.com/video/BV1g4411H78N?p=63&spm_id_from=pageDriver]
* HM NIS Edit 和 NSIS

### 案例:翻金币
+ 收获
    1. 删除资源文件后需要删除debug文件,不然会报错
    3. 界面的切换可以使用信号和槽 即其它界面emit发送一个信号,主界面接收
        + 当然也可以选择记录父类指针,但是必须要在构造函数中多传个参数，而不是使用默认的parent
    4. 在按钮上方有其他组件，可以使用label->setAttribute(Qt::WA_TransparentForMouseEvents);让其可以点到按钮[51号属性]
    5. 界面翻转金币 本质上是个按钮 
        + 人点击后 
        + 金币触发翻转
        + 定时器每隔30ms发送一次信号给金币
        + 金币触发图片重新放置,到最大值或者最小值的时候关闭定时器
        + 金币中有坐标i 和 j 以及一个flag 来确定该金币在页面中的位置
    6. 锁定窗口 m_chooseScence->setGeometry(this->geometry()); 每次进入或者退出都锁定他的位置
+ 延时器
    QTimer::singleShot(毫秒,拉姆达表达式);













[https://www.bilibili.com/video/BV1g4411H78N?p=63&spm_id_from=pageDriver]: 
