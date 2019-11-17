---
layout: post
title: Python数据分析处理库Pandas
tags: [Python, 数据]
author: Gump Wang
---

Pandas 基于 Numpy, 做数据处理使用。

## 数据读取

```python
import pandas
food_info = pandas.read_csv("food_info.csv")
type(food_info) 
# => <class 'pandas.core.frame.DataFrame'> (pandas 核心结构，类似矩阵)

food_info.dtypes
# => NDB_No               int64
# => Shrt_Desc           object
# => Water_(g)          float64
# => Energ_Kcal           int64
# => Protein_(g)        float64
# => ...
# => dtype: object

# pandas 中，类型如下：
# object - string value
# int - integer value
# float - float value
$ datetime - time value
# bool - boolean value

# 显示头部数据，默认前 5 个
food_info.head()
# 显示头 3 条数据
food_info.head(3)
# 显示尾数据
food_info.tail()

# 显示列名
food_info.columns
# => Index(['NDB_No', 'Shrt_Desc', 'Water_(g)', 'Energ_Kcal', 'Protein_(g)',
# =>        ...
# =>        'Cholestrl_(mg)'],
# =>       dtype='object')

# 显示维度
food_info.shape
# => (8618, 36)
```

## 索引与计算

```python
# 按行取数据
# 获取第 0 行数据
food_info.loc[0]
# 获取第 3-6 行数据
food_info.loc[3:6]
# 获取第 2、5、10 行数据
food_info.loc[[2, 5, 10]]

# 按列取数据
# 列名取值
food_info["NDB_No"]
# 取其中某几列
food_info[["Zinc_(mg)", "Copper_(mg)"]]
# 获取单位为 "(g)" 的列
# 1. 将列名专为 list
col_names = food_info.columns.tolist()
gram_columns = []
for c in col_names:
	if c.endswith("(g)"):
		gram_columns.append(c)
gram_df = food_info[gram_columns]

# 加减乘除操作
# 对每个值进行除 1000
div_1000 = food_info["Iron_(mg)"] / 1000
# 对应相乘
water_energy = food_info["Water_(g)"] * food_info["Energ_Kcal"]

# 添加一列
iron_grams = food_info["Iron_(mg)"] / 1000
food_info["Iron_(g)"] = iron_grams 

# 极值
max_calories = food_info["Energ_Kcal"].max()

# 制定列排序，默认从小到大
food_info.sort_values("Sodium_(mg)", inplace=True)
```

## 数据预处理实例

```python
# 泰坦尼克生还实例
titanic_survival = pd.read_csv("itanic_train.csv")

age = titanic_survival["Age"]
age_is_null = pd.isnull(age)
# => 0 False
# => 1 False
# => 2 True
# => ...

# age 为 null 当作索引取值
age_null_true = age[age_is_null]
# => 2 NaN
# => ...

# 缺失值长度
age_null_count = len(age_null_true)

# 计算平均年龄
mean_age = sum(titanic_survival["Age"]) / len(titanic_survival["Age"])
# => nan (因为有缺失值)

good_ages = titanic_survival["Age"][age_is_null == False]
correct_mean_age = sum(good_ages) / len(good_ages)
# => 29.669

# pandas 均值函数
correct_mean_age = titanic_survival["Age"].mean()

# 每个船舱等级船票均价
# 1. 遍历计算
passenger_classes = [1, 2, 3]
fares_by_class = {}
for this_class in passenger_classes:
	pclass_rows = titanic_survival[titanic_survival["Pclass"] == this_class]
	pclass_fares = pclass_rows["Fare"]
	fare_for_class = pclass_fares.mean()
	fares_by_class[this_class] = fare_for_class
fares_by_class
# => [1: 84.154, 2: 20.662, 3: 13.675]

# 使用 pivot_table 计算
# 计算每种船票等级平均获救人数
passenger_survival = titanic_survival.pivot_table(index="Pclass", values="Survived", aggfunc=numpy.mean)
# => Pclass
# => 1 0.629630
# => 2 0.472826
# => 3 0.242363
# => Name: Survived, dtype: float64

# 每个等级平均年龄 aggfunc 默认为平均 numpy.mean
passenger_age = titanic_survival.pivot_table(index="Pclass", values="Age")
# => Pcalss
# => 1 38.233441
# => 2 29.877630
# => 3 25.140620
# => Name: Age, dtype: float64

# 不同登船地点中 船票总值 与 生还总数 的关系
port_stats = titanic_survival.pivot_table(index="Embarked", values=["Fare", "Survived"], aggfunc=numpy.sum)

# => Embarded       Fare       Survived
# => C 			10072.2962		93
# => Q			1022.2543		30
# => S 			17439.3988		217

# 丢掉缺失值
# 丢掉指定列缺失值样本 (axis 维度, 可指定 axis=1 or axis=columns)(!!! 待完善)
drop_na_columns = titanic_survival.dropna(axis=1)
# 删除 Age 与 Sex 这两列中包含 Nan 的行
new_titanic_survival = titanic_survival.dropna(axis=0, subset=["Age", "Sex"])

# 排序
titanic_survival.sort_values
```

## 自定义函数

apply

## Series 结构

* Series: Collection of values
* DataFrame: Collection of Series objects

Series 为 一行 或 一列





