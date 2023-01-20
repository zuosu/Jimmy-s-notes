## `print`
* `format`的使用
```python
print("删除dummy后的d1：{0}，使用pop方法的返回值：{1}".format(d1,return_value))
```
`.format`要在字符串之后，`print`括号内使用，而且格式的索引`{0},{1}`必须从0开始，否则**会报元组索引超出范围的错误**
## 判断数据类型
isinstance() 函数来判断一个对象是否是一个已知的类型，类似 type()。
> isinstance() 与 type() 区别：
> -   type() 不会认为子类是一种父类类型，不考虑继承关系。
> -   isinstance() 会认为子类是一种父类类型，考虑继承关系。
>  **如果要判断两个类型是否相同推荐使用 isinstance()。**
* 举例
```python
#!/usr/bin/env python
a = 1
b = [1,2,3,4]
c = (1,2,3,4)
d = {'a':1,'b':2,'c':3}
e = "abc"
if isinstance(a,int):
    print("a is int")
else:
    print("a is not int")
print("---"*10)
if isinstance(b,list):
    print("b is list")
else:
    print("b is not list")
print("---"*10)
 
if isinstance(c,tuple):
    print("c is tuple")
else:
    print("c is not tuple")
print("---"*10)
 
if isinstance(d,dict):
    print("d is dict")
else:
    print("d is not dict")
print("---"*10)
 
if isinstance(e,str):
    print("d is str")
else:
    print("d gis not str")
```
* 结果
```python
a is int
------------------------------
b is list
------------------------------
c is tuple
------------------------------
d is dict
------------------------------
d is str
```
* `type()`  与 `isinstance()` 区别：
```python
class A:
    pass
 
class B(A):
    pass
 
isinstance(A(), A)    # returns True
type(A()) == A        # returns True
isinstance(B(), A)    # returns True 这里是考虑继承的哈
type(B()) == A        # returns False
```

* 以下展示了使用 isinstance 函数的实例：
```python
>>>a = 2
>>> isinstance (a,int)
True
>>> isinstance (a,str)
False
>>> isinstance (a,(str,int,list))    # 是元组中的一个返回 True
True
```
## TypeError: 'int' object is not subscriptable的几种常见情况及解决办法
此报错一般是在整数上加了下标
[(31条消息) python报错：TypeError: 'int' object is not subscriptable的几种常见情况及解决办法_Devinxtw的博客-CSDN博客](https://blog.csdn.net/qq_40688707/article/details/88862338)

##  `enumerate()`  函数
* 普通的for循环
```python
>>> i = 0  
>>> seq = ['one', 'two', 'three']  
>>> for element in seq:  
...     print i, seq[i]  
...     i += 1  
...  
0 one  
1 two  
2 three
```
*  for 循环使用 enumerate
```python
>>> seq = ['one', 'two', 'three']  
>>> for i, element in enumerate(seq):  
...     print i, element  
...  
0 one  
1 two  
2 three
```

## `defaultdict` 方法
![](https://files.mdnice.com/user/25190/7f2377e8-0a5e-41a1-8995-83ec1d66635e.png)

[Python中的defaultdict方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/345741967)

## TypeError: unhashable(不可哈希的) type: ‘dict‘原因
**python不支持dict的key为list或dict类型，因为list和dict类型是unhashable（不可哈希）的**
[(31条消息) TypeError: unhashable type: ‘dict‘原因_好事要发生的博客-CSDN博客_typeerror: unhashable type: 'dict](https://blog.csdn.net/weixin_43505418/article/details/115439723)

[(31条消息) TypeError: unhashable type: 'dict'_HuaCode的博客-CSDN博客](https://blog.csdn.net/HuaCode/article/details/79855609)

## 嵌套元组怎么索引？
```python
tuple = (('Python', 'Tuple', 'Lists'), ('Python', 'Tuple', 'Lists', 'Includehelp', 'best'))
tuple[0][0]# 访问的是第0个元组的第0个元素
```