---
layout: post
title: Python科学计算库Numpy
tags: [Python, 数据]
author: Gump Wang
---

## 关于 Python 版本

~> 2.0 与 ~> 3.0 都可以实现想要的功能

## Anaconda

## Numpy 基础

* 打印帮助文档

``` python
print (help(numpy))
```

* 读取文件

``` python
genfromtxt("aaa.txt", delimiter=",", dtype=str, skip_header=1)
```

* array

将普通 list 转换成 ndarray (几维数据就用几个中括号)

```
# 一维
vector = numpy.array([1, 2, 3, 4])
# 二维矩阵
matrix = numpy.array([[1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]])
```

* shape

获取 ndarray 结构 (方便调试)

如 (4,) or (2, 3) # 对于矩阵 (行, 列)

* 读取数组元素

``` python
# 一维
vector[0]
# 二维矩阵
matrix[1, 3] # 行, 列
# 一维切片
vector[0:3]
# 二维矩阵某一列
matrix[:, 2]
# 二维矩阵多列
matrix[:, 0:2]
```

* dtype

numpy 中元素类型必须是一致的，如果类型不一致，会自动转换类型，如 int -> float, int -> string, float -> string

``` python
vector.dtype
```

* 判断

与每一个元素进行比较，bool 类型可以当作索引进行取值

``` python
vector == 2
# => array([False, True, False, False], dtype=bool) 

# 获取匹配元素
equal_to_two = (vector == 2)
print equal_to_two
print(vector([equal_to_two]))
# => array([False, True, False, False], dtype=bool)
# => [2]

# 获取二维矩阵中第二列元素等于 3 的行数据
second_column_2 = (matrix[:, 1] == 3)
print second_column_2
print(matrix[second_column_2, :])
# => [False, True, False, False]
# => [[2, 3, 4, 5]]
```

## 矩阵基础

* 组合条件 & | 
* 元素类型转换

``` python
vector = numpy.array(["1", "2", "3"])
print (vector.dtype)
vector = vector.astype(float)
print (vector.dtype)
print (vector)

# => <U1
# => float64
# => [1. 2. 3.]
```

* 求极值

``` python
vector.min()
```

* 求和

``` python
matrix = numpy.array([[5, 10, 15], [20, 25, 30], [35, 40, 45]])
# 按行求和
matrix.sum(axis=1)
# => array([ 30,  75, 120])

# 按列求和
matrix.sum(axis=0)
# => array([60, 75, 90])

```

## 常用函数

* arange, reshape

``` python
# 构建向量
numpy.arange(15)
# => array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])

# 构建矩阵
a = numpy.arange(15).reshape(3, 5)
# => array([[ 0,  1,  2,  3,  4],
# =>        [ 5,  6,  7,  8,  9],
# =>        [10, 11, 12, 13, 14]])

# 维度
a.ndim
# => 2

# 类型名字
a.dtype.name
# => 'int64'

# 元素个数
a.size
# => 15
```

* 初始化

``` python
# 初始值都为 0
numpy.zeros((3, 4)) # 参数为元组
# => array([[0., 0., 0., 0.],
# =>        [0., 0., 0., 0.],
# =>        [0., 0., 0., 0.]])

# 初始值都为 1
numpy.ones((2, 3, 4), dtype=numpy.int32)
# => array([[[1, 1, 1, 1],
# =>         [1, 1, 1, 1],
# =>         [1, 1, 1, 1]],
# =>
# =>        [[1, 1, 1, 1],
# =>         [1, 1, 1, 1],
# =>         [1, 1, 1, 1]]], dtype=int32)

# 根据起始值、结束值、步长初始化
numpy.arange(10, 30, 5)
# => array([10, 15, 20, 25])

# 随机初始化
numpy.random.random((2, 3)) # random 默认范围为 -1 ~ 1
# => array([[0.03098052, 0.31944657, 0.7471743 ],
# =>        [0.15375132, 0.18301579, 0.16133002]])

# 根据范围和数量初始化
numpy.linspace(0, 2*pi, 20)
# => array([0.        , 0.10526316, 0.21052632, 0.31578947, 0.42105263,
# =>        0.52631579, 0.63157895, 0.73684211, 0.84210526, 0.94736842,
# =>        1.05263158, 1.15789474, 1.26315789, 1.36842105, 1.47368421,
# =>        1.57894737, 1.68421053, 1.78947368, 1.89473684, 2.        ])
```

* 运算

``` python
a = numpy.array([20, 30, 40, 50])
# => array([20, 30, 40, 50])
b = numpy.arange(4)
# => array([0, 1, 2, 3])

c = a - b
# => array([20, 29, 38, 47])

c = c - 1
# => array([19, 28, 37, 46])

# 平方
b**2 
# => array([0, 1, 4, 9])

b < 35
# => array([ True,  True,  True,  True])

# 乘法
A = numpy.array([[1, 1],
		   		 [0, 1]])
B = numpy.array([[2, 0],
		   		 [3, 4]])

A * B # 对应位置相乘
# => array([[2, 0],
# =>        [0, 4]])

A.dot(B) # 矩阵乘法 or numpy.dot(A, B)
# => array([[5, 4],
# =>        [3, 4]])
```

## 矩阵常用操作

* 变换

``` python
a = numpy.floor(10*numpy.random.random((3, 4)))
# => array([[9., 1., 2., 2.],
# =>        [5., 5., 6., 8.],
# =>        [8., 3., 5., 2.]])

# 将矩阵转成向量，与 reshape 相反
a.revel()
# => array([9., 1., 2., 2., 5., 5., 6., 8., 8., 3., 5., 2.])

# 用 shape 指定行列
a.shape = (6, 2) # 对应 reshape(6, 2)
# => array([[9., 1.],
# =>        [2., 2.],
# =>        [5., 5.],
# =>        [6., 8.],
# =>        [8., 3.],
# =>        [5., 2.]])

# 注意: 对于二维矩阵如果指定 reshape 行数，列数为 -1 程序会自动计算列数，如 a.reshape(3, -1)

# 转制
a.T
```

* 拼接

``` python
a = numpy.floor(10*numpy.random.random((2, 2)))
# => array([[3., 8.],
# =>        [7., 0.]])
b = numpy.floor(10*numpy.random.random((2, 2)))
# => array([[3., 0.],
# =>        [0., 5.]])

# 横向拼接
numpy.hstack((a, b))
# => array([[3., 8., 3., 0.],
# =>        [7., 0., 0., 5.]])
# 纵向拼接
numpy.vstack((a, b))
# => array([[3., 8.],
# =>        [7., 0.],
# =>        [3., 0.],
# =>        [0., 5.]])
```

* 切分

``` python
a = numpy.floor(10*numpy.random.random((2, 12)))
# => array([[4., 7., 5., 8., 3., 5., 7., 6., 1., 9., 7., 3.],
# =>       [5., 0., 7., 9., 0., 2., 1., 0., 0., 7., 3., 9.]])

# 横向切分
numpy.hsplit(a, 3)
# => [array([[4., 7., 5., 8.],
# =>        [5., 0., 7., 9.]]), array([[3., 5., 7., 6.],
# =>        [0., 2., 1., 0.]]), array([[1., 9., 7., 3.],
# =>        [0., 7., 3., 9.]])]

# 指定位置横向切分
numpy.hsplit(a, (3, 4)) # 第 3 个后切一刀，第 4 个后切一刀
# => [array([[4., 7., 5.],
# =>        [5., 0., 7.]]), array([[8.],
# =>        [9.]]), array([[3., 5., 7., 6., 1., 9., 7., 3.],
# =>        [0., 2., 1., 0., 0., 7., 3., 9.]])]

# 纵向切分
numpy.vsplit(a, 1)
# => [array([[4., 7., 5., 8., 3., 5., 7., 6., 1., 9., 7., 3.],
# =>        [5., 0., 7., 9., 0., 2., 1., 0., 0., 7., 3., 9.]])]
```

## 不同复制操作对比

复制有 3 种方法

``` python
# 等号赋值，引用与元素都不复制
a = numpy.arange(12)
# => array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
b = a
# => True
b.shape = (3, 4)
a.shape
# => (3, 4)
id(a)
# => 4388924432
id(b)
# => 4388924432

# 浅拷贝，引用复制，元素值不复制
c = a.view()
c is a
# => False
c.shape = (2, 6)
a.shape
# => (3, 4) a 的 shape 值不变
c[0, 4] = 1234
a # a 的元素值改变
# => array([[   0,    1,    2,    3],
# =>        [1234,    5,    6,    7],
# =>        [   8,    9,   10,   11]])

# 深拷贝, 引用与元素值都复制
d = a.copy()
```

* 获取最大值索引

``` python
data = numpy.sin(numpy.arange(20)).reshape(5, 4)
# => array([[ 0.        ,  0.84147098,  0.90929743,  0.14112001],
# =>        [-0.7568025 , -0.95892427, -0.2794155 ,  0.6569866 ],
# =>        [ 0.98935825,  0.41211849, -0.54402111, -0.99999021],
# =>        [-0.53657292,  0.42016704,  0.99060736,  0.65028784],
# =>        [-0.28790332, -0.96139749, -0.75098725,  0.14987721]])

# 每列元素最大值索引(行)
ind = data.argmax(axis=0)
# => array([2, 0, 3, 1])

# 根据索引获取最大元素
data_max = data[ind, range(data.shape[1])]
# => array([0.98935825, 0.84147098, 0.99060736, 0.6569866 ])
```

* 扩展

``` python
a = numpy.arange(0, 40, 10)
# => array([ 0, 10, 20, 30])

# 扩大到 行元素数乘 3，列元素数乘 5
b = numpy.tile(a, (3, 5))
# => array([[ 0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30, 0, 10, 20, 30],
# =>        [ 0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30, 0, 10, 20, 30],
# =>        [ 0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30,  0, 10, 20, 30, 0, 10, 20, 30]])
```

* 排序

``` python
a = numpy.array([[4, 3, 5], [1, 2, 1]])

# 行元素按从小到大排序
b = numpy.sort(a, axis=1)
# => array([[3, 4, 5],
# =>        [1, 1, 2]])

# 索引从小到大排序
a = numpy.array([4, 3, 1, 2])
j = numpy.argsort(a)
# => array([2, 3, 1, 0])
a[j]
# => array([1, 2, 3, 4])
```