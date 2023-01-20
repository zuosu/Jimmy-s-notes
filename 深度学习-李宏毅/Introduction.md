![](https://files.mdnice.com/user/25190/4d4a49e5-f62b-46e4-a085-46215fa67bb6.png)

# 机器学习基本概念
## 什么是机器学习？
找一个函数

## 不同类型的函数
![](https://files.mdnice.com/user/25190/2c004819-ec4d-43ae-9691-52f26d89b0d6.png)

1. regression输出是一个数值（scalar）；
2. Classification输出类别，如输入一封邮件判断是否为垃圾邮件
![](https://files.mdnice.com/user/25190/1571b723-9a61-4425-b021-e3cb9dc65352.png)

alphaGO也是一个classification，只不过输出类别比较多，是棋盘的位置。

## 怎么去找这个函数？
以预测Youtube明天点赞数为例
1. 确定一个带有参数的函数

![](https://files.mdnice.com/user/25190/b657b748-24e0-4186-95f7-cef6c76c177a.png)

2. 根据训练资料定义一个损失函数

![](https://files.mdnice.com/user/25190/585eba09-7ce6-4564-9aaf-91e6b6302010.png)

如果取$b=0.5k$（k是单位千）$w=1$，由1.2日的订阅量为$4.9k$，代入函数 $y=0.5k+x_1$得到$y=5.4k$，然而真实值为$7.5k$，这样可以得到预测值和真实值之间误差的绝对值为$2.1k$。

![](https://files.mdnice.com/user/25190/0799822a-01aa-4a0b-8974-d1df94d8e279.png)

将上述误差累积取均值作为L，L越大说明损失越严重。$e_n$有多种定义，上述的e定义为误差的绝对值，也可以定义为误差的平方。具体要选择什么样的e，视情况而定。如果$y$和$\hat{y}$均是概率分布，e取Cross-entropy。

尝试不同的参数$b、w$得到对应的loss，绘制出下图。
![](https://files.mdnice.com/user/25190/7b17ed54-e96d-4e62-b0ff-fcf8869ea032.png)

图中央的蓝色曲线为等高线，这条线上的损失相等，越向左下角L越大，越向右上角L越小。L最小的点大致在（1，250）椭圆的中心。

3. 参数优化

参数优化即找到最优参数$w、b$即求最小化L的问题。
$$
w^*,b^*=arg \ \  min_{w,b} L
$$

在此介绍Gradient Descent方法

![](https://files.mdnice.com/user/25190/cf751abc-aa6f-420d-849d-9ca581d3b084.png)

对L求取$w_0$处的偏导，若得到的值大于0则减小w，否则增大w。（当$L$在$w_0$处的偏导小于0，相当于L在$w_0$处的附近是关于w的减函数，让w增大可以使L减小）。

若是需要减小w，该减小多少呢（或者说减小的步长是多少呢）？
* 步长和此处斜率有关，步长大可以让步长大一点；相反斜率小，步长就小一点。
* 步长还和参数$\eta$（称为学习速率）有关

![](https://files.mdnice.com/user/25190/111cdfa4-0f74-4118-815d-28a0a8f6c8ff.png)

这些需要自己去设定的参数称为超参数**hyperparameters**。

确定步长之后，逐步更新w的值，直到求取的偏导为0，或迭代次数达到自己设定的值之后停止更新。

![](https://files.mdnice.com/user/25190/b58b57bd-ec03-4d42-89e0-de013c4b9fcb.png)

从上图可以看出Gradient Descent方法存在一种劣势，它找到的最优点可能是局部最优点而不是全局最优点。但在实际应用中这并不是其真正的痛点。

![](https://files.mdnice.com/user/25190/49aee4ae-8907-4b08-a7f3-daee6de973db.png)

![](https://files.mdnice.com/user/25190/3a0697da-3418-4047-b615-6d488061628c.png)


通过之前的数据我们得出了一个函数，接下来需要使用这个函数去预测未来的订阅量。得到的损失$L'$是0.58k。


![](https://files.mdnice.com/user/25190/18d0ae94-2afd-4ad2-afe6-8061a3057ff8.png)

![](https://files.mdnice.com/user/25190/09dced5e-a89f-45de-af88-599cdd398f0d.png)

这个函数是根据前一天的订阅量来预测的，当考虑用前7天、前28天、前56天预测效果会怎样？

![](https://files.mdnice.com/user/25190/c89cf603-c0f6-432b-9a03-96ea11a0af69.png)

从图中可以看出，考虑前面的天数越多，预测得到结果的损失就越小。这些函数都是线性的被称为线性模型。

# 深度学习基本概念
## 使用常量+折线（hard sigmoid）近似

上面讲述的线性模型存在Model Bias（模型误差）。如下图，线性模型只能通过改变斜率和截距来改变模型（蓝色线条），若真实情况如红色线条，通过线性模型显然不能很好的模拟。

![](https://files.mdnice.com/user/25190/80ee75a0-7eec-4d90-bed8-5b360e3aed6e.png)

使用常量+一组折线来模拟红线。

![](https://files.mdnice.com/user/25190/b3453bb0-8c67-4cc3-ab36-8b2dfef1b216.png)

进一步推广，所有的分段线性曲线都可以通过上面的方式来模拟。

![](https://files.mdnice.com/user/25190/31e47402-690a-42f5-aa46-be148e14cad5.png)

当曲线的转折点越多需要的蓝色折线（$\_ /^{-}$）也就越多。遇到平滑曲线时也可以通过分段曲线来近似（在弯曲部分转折点取多点就可以）。

![](https://files.mdnice.com/user/25190/5352f921-51ab-41a7-aa3b-ad65435189f6.png)

## 怎样得到折线？

![](https://files.mdnice.com/user/25190/e5407307-9fe6-4405-a50e-2636cc93eecf.png)

使用sigmoid函数
$$
y=c\frac{1}{1+e^{-(b+wx_1)}}\\
=c\ sigmoid(b+wx_1)
$$
当e的指数趋于无穷大时，y趋于c，当e的指数趋于负无穷大时，y趋于0，能够很好的近似折线。该折线也被称为Hard Sigmoid。

> sigmoid Function翻译为中文记为S函数，有点像高中生物学的种群数量的S曲线。
> 
> 之前了解到sigmoid函数是在神经网络里，它起到过滤的作用。

通过改变Sigmoid Function中的参数可以得到不同的近似折线。改变w可以改变斜率；改变b可以让其平移；改变c可以改变高度。

![](https://files.mdnice.com/user/25190/066b9072-ee00-4a0c-af8b-8ee2e50b72c6.png)

这样刚开始模拟红色曲线的问题就解决啦。

![](https://files.mdnice.com/user/25190/3d35b589-16ca-49cc-9c73-443b2d8d1450.png)

总结进而得到了一个更多参数的新模型。

![](https://files.mdnice.com/user/25190/0272b441-5263-408c-9c3a-13faa772aa90.png)

![](https://files.mdnice.com/user/25190/c5ab6c8c-a3f3-4a0b-88cd-4a7c46d21e57.png)

蓝色框可以进一步写为矩阵的形式

![](https://files.mdnice.com/user/25190/d873b642-b422-4d52-8931-3dddc0267cae.png)

![](https://files.mdnice.com/user/25190/c841f573-a0a3-4e21-981d-50920315b48c.png)

![](https://files.mdnice.com/user/25190/4a6f83b6-80cc-4bcf-8f53-443c69dce484.png)

按照上图中的虚线代入得矩阵得表示形式。

![](https://files.mdnice.com/user/25190/7e1a90c4-df82-4e7b-8031-09b526bc35ca.png)


将上面的各个参数拉直统一为$\theta$列向量。

![](https://files.mdnice.com/user/25190/081e4713-adc3-4f7a-800f-131888829092.png)

1. 回到机器学习，这样就确定了一个带参数的函数。

![](https://files.mdnice.com/user/25190/b64691df-9a35-4064-ab0e-efb91e375fab.png)

2. 计算Loss

![](https://files.mdnice.com/user/25190/3f691775-686b-4c84-b849-66be7aff7856.png)

3. 优化新模型的参数

和机器学习的优化参数方法一样使用梯度下降法。随机选初始值 -> 求取梯度 -> 按照步长（$\eta * g$）迭代 -> 直到不想迭代（或所求梯度为0，达到此条件很难）

![](https://files.mdnice.com/user/25190/dde5f31d-c835-4819-b72a-ccb0cd05f91e.png)

![](https://files.mdnice.com/user/25190/709f5f25-56a5-48dc-8890-c857a7787d40.png)

实际计算中Loss并不是使用所有训练数据计算得来的，而是将N个数据分成多批（batch）用一个batch来计算Loss。更新完所有batch叫做一个阶段（epoch）。

![](https://files.mdnice.com/user/25190/1acff2df-879b-4441-80c1-1c1a78dabca1.png)

举例说明epoch和batch之间的区别。
一共有10000个数据，每个batch中有10个数据，则共有1000个batch，每次通过一个batch计算Loss就要更新一次，那么一个epoch就要更新1000次。

![](https://files.mdnice.com/user/25190/1998cc38-c19a-404a-aef7-8c8f40bfaef7.png)

hard sigmoid function可以用两个ReLU函数进行代替。sigmoid函数和ReLU函数都是激活函数。

![](https://files.mdnice.com/user/25190/28f68c5d-5b44-4558-b040-b7853c4c7ca2.png)

![](https://files.mdnice.com/user/25190/85a1a2c9-bfd4-49c3-8af0-bc85ca3ef7e5.png)

使用线性和ReLU函数的实验结果：

![](https://files.mdnice.com/user/25190/d28d6113-e4c5-432b-8297-349a682747ea.png)

改进，把x多做几层线性模拟

![](https://files.mdnice.com/user/25190/c40260f3-667a-4b82-b938-0cad75752462.png)

多做几层之后的效果

![](https://files.mdnice.com/user/25190/1995fa54-d8bc-4552-a37b-b63906db4c2e.png)

![](https://files.mdnice.com/user/25190/f67cfddd-e105-4da1-8daf-feb25b9e9c6e.png)

给上述的模型取个名字，神经网络。

![](https://files.mdnice.com/user/25190/a8489c26-80c7-4ffd-aee5-4421b6fb7de1.png)

多个隐含层意为深度学习。

![](https://files.mdnice.com/user/25190/22576718-aa73-459a-97c5-30a17274bf5e.png)

用四层来预测2021年的数据效果反而变差了，我们称之为过拟合（overfitting）。

> 怎样改善过拟合？

___
使用正则化，在Loss Function中添加一项
$$
\lambda \sum (w_i)^2
$$
为了让loss尽可能的小，需要加的这一项尽可能小，也就需要$w_i$尽可能的小。当$w_i$小时可以让函数y变得更加平滑（存在干扰$\Delta x_i$时对应的输出变化会很小）。参数$\lambda$越大，说明越想考虑平滑。

![](https://files.mdnice.com/user/25190/64316e44-848b-4a5a-9516-2a677e25e271.png)

![](https://files.mdnice.com/user/25190/f454c41d-fa1c-49e3-9d20-5458189624c2.png)
___

![](https://files.mdnice.com/user/25190/f28e6d23-9a9d-4d82-912b-261bfe004940.png)

使用三层还是四层呢？

![](https://files.mdnice.com/user/25190/27661884-4576-4222-bc46-1d782c1a47e5.png)

总结：深度学习和机器学习一样都需要都有三个步骤
1. 确定一个带参数的函数
2. 确定损失函数
3. 优化参数

其中第一步确定一个带参数的函数相当于确定一个结构，如全前向连接。

![](https://files.mdnice.com/user/25190/94229c6e-4687-4b4b-a781-19018bdf2290.png)

![](https://files.mdnice.com/user/25190/31f972d7-1044-4c8c-b310-da3e7c2054c8.png)

![](https://files.mdnice.com/user/25190/388e8f94-2d12-4fc8-a6c2-cb18499c02af.png)

![](https://files.mdnice.com/user/25190/1a8bc0a5-52f2-4d80-9e66-8c6ec8631b7e.png)

![](https://files.mdnice.com/user/25190/3cc3a203-52aa-4f00-814f-8a15c98288b3.png)

举例：手写数字的识别，输入为256个x，输出为10个y，这10个y就是该数字是0-9的概率，取概率大的作为结果。

![](https://files.mdnice.com/user/25190/cd8da4d7-2c9c-47bd-a2ba-a730a60f65b3.png)