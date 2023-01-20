# 数据挖掘的五大流程
1. 获取数据
2. 数据预处理
**数据预处理：**是从数据中检测，纠正或删除损坏，不准确或不适用于模型的记录的过程  
**可能面对的问题有：** 数据类型不同，比如有的是文字，有的是数字，有的含时间序列，有的连续，有的间断。  也可能，数据的质量不行，有噪声，有异常，有缺失，数据出错，量纲不一，有重复，数据是偏态，数据量太  大或太小  
**数据预处理的目的：** 让数据适应模型，匹配模型的需求
3. 特征工程
**特征工程是：** 将原始数据转换为更能代表预测模型的潜在问题的特征的过程，可以通过挑选最相关的特征，提取  
特征以及创造特征来实现。其中创造特征又经常以降维算法的方式实现。  
**可能面对的问题有：** 特征之间有相关性，特征和标签无关，特征太多或太小，或者干脆就无法表现出应有的数  
据现象或无法展示数据的真实面貌  
**特征工程的目的：** 1) 降低计算成本2) 提升模型上限

4. 建模，测试模型并预测出结果
5. 上线，验证模型结果

# sklearn中的数据预处理和特征工程

* 模块preprocessing：几乎包含数据预处理的所有内容  
*  模块Impute：填补缺失值专用  
* 模块feature_selection：包含特征选择的各种方法的实践
* 模块decomposition：包含降维算法

# 2 数据预处理preprocessing&impute

## 2.1 数据无量纲化

在机器学习算法实践中，我们往往有着将不同规格的数据转换到同一规格，或不同分布的数据转换到某个特定分布  
的需求，这种需求统称为将数据**“无量纲化”** 。
1. 譬如梯度和矩阵为核心的算法中，如逻辑回归，支持向量机，神经网络，无量纲化可以加快求解速度；
2. 距离类模型，譬如K近邻，K-Means聚类中，无量纲化可以帮我们提升模型精度，避免某一个取值范围特别大的特征对距离计算造成影响；
3. 一个特例是决策树和树的集成算法们，对决策树我们不需要无量纲化，决策树可以把任意数据都处理得很好；

数据的无量纲化可以是线性的，也可以是非线性的。线性的无量纲化包括**中心化**（Zero-centered或者Meansubtraction）处理和**缩放处理**（Scale）。**中心化的本质**是让所有记录减去一个固定值，即让样本数据平移到某个位置。**缩放的本质**是通过除以一个固定值，将数据固定在某个范围之中，取对数也算是一种缩放处理。

### preprocessing.MinMaxScaler
当数据$(x)$按照最小值中心化后，再按极差（最大值 - 最小值）缩放，数据移动了最小值个单位，并且会被收敛到  [0,1]之间，而这个过程，就叫做数据归一化(Normalization，又称Min-Max Scaling)。

>注意，Normalization是归  
一化，不是正则化，真正的正则化是regularization，不是数据预处理的一种手段。

归一化之后的数据服从正态分  
布，公式如下:
$$
x^* = \frac{x-min(x)}{max(x)-min(x)}
$$

在sklearn当中，我们使用preprocessing.MinMaxScaler来实现这个功能。MinMaxScaler有一个重要参数，  
feature_range，控制我们希望把数据压缩到的范围，默认是[0,1]。

```python
from sklearn.preprocessing import MinMaxScaler
# 构造数据
data = [[-1,2],[-0.5,6],[0,10],[1,18]]# data为四行两列的数据

scaler = MinMaxScaler() # 实例化
scaler = scaler.fit(data) # fit在这里本质是生成min(x)和max(x)
result = scaler.transform(data) # 通过接口导出结果
print(result)
```

输出的结果如下，可见最终将数据都归到了[0,1]

![](https://files.mdnice.com/user/25190/884c06b6-db31-4e60-8d09-b3854fdfdd86.png)

实例化MinMaxScaler之后可以**一步到位实现数据的转化**
```python
scaler = MinMaxScaler() # 实例化
result_ = scaler.fit_transform(data)    # 训练和导出的结果一步达成
```

即在实际应用中只需要这两行代码就可以完成数据的无量纲化。

当忘记之前的数据时可以使用下面的方式**对处理之后的数据进行还原**
```python
scaler.inverse_transform(result)    # 将归一化后的结果逆转
```

使用MinMaxScaler的参数feature_range实现将数据**归一化到[0,1]以外的范围中**

```python
data = [[-1,2],[-0.5,6],[0,10],[1,18]]
scaler = MinMaxScaler(feature_range=[5,10]) # 实例化
result = scaler.fit_transform(data)
print(result)
```

![](https://files.mdnice.com/user/25190/47f12826-2d02-459c-9400-6c0cbdea183a.png)

> 使用numpy来实现归一化
> ```python
>  import numpy as np
># 归一化
x = np.array([[-1,2],[-0.5,6],[0,10],[1,18]])
x_nor = (x-x.min(axis=0))/(x.max(axis=0)-x.min(axis=0))
print(x_nor)
># 逆转归一化
x_returned = x_nor * (x.max(axis=0)-x.min(axis=0))+x.min(axis=0)
print(x_returned)
> ```

