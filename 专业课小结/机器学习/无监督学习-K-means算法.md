# 无监督学习-K-means算法

## 1、 什么是无监督学习

- 一家广告平台需要根据相似的人口学特征和购买习惯将美国人口分成不同的小组，以便广告客户可以通过有关联的广告接触到他们的目标客户。
- Airbnb 需要将自己的房屋清单分组成不同的社区，以便用户能更轻松地查阅这些清单。
- 一个数据科学团队需要降低一个大型数据集的维度的数量，以便简化建模和降低文件大小。

我们可以怎样最有用地对其进行归纳和分组？我们可以怎样以一种压缩格式有效地表征数据？**这都是无监督学习的目标，之所以称之为无监督，是因为这是从无标签的数据开始学习的。**

## 2、 无监督学习包含算法

- 聚类
  - K-means(K均值聚类)
- 降维
  - PCA

## 3、 K-means原理

我们先来看一下一个K-means的聚类效果图

![image-20220404231339803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220404231339803.png)

### 3.1 K-means聚类步骤

- 1、随机设置K个特征空间内的点作为初始的聚类中心
- 2、对于其他每个点计算到K个中心的距离，未知的点选择最近的一个聚类中心点作为标记类别
- 3、接着对着标记的聚类中心之后，重新计算出每个聚类的新中心点（平均值）
- 4、如果计算得出的新中心点与原中心点一样，那么结束，否则重新进行第二步过程

## 4、K-meansAPI

- sklearn.cluster.KMeans(n_clusters=8,init=‘k-means++’)
  - k-means聚类
  - **n_clusters:开始的聚类中心数量** 比如 n_clusters=4
  - init:初始化方法，默认为'k-means ++’
  - labels_:默认标记的类型，可以和真实值比较（不是值比较）

## 5、 案例：k-means对Instacart Market用户聚类

### 5.1 分析

- 1、降维之后的数据
- 2、k-means聚类
- 3、聚类结果显示

### 5.2 代码

```python
# 取500个用户进行测试
# 如果b_i>>a_i:趋近于1效果越好， b_i<<a_i:趋近于-1，效果不好。轮廓系数的值是介于 [-1,1] ，越趋近于1代表内聚度和分离度都相对较优。
cust = data[:500]
km = KMeans(n_clusters=4)
km.fit(cust)
pre = km.predict(cust)
print(silhouette_score(cust, pre))
```

返回结果：

```python
0.466014214896049
```



#### 问题：如何去评估聚类的效果呢？

## 6、Kmeans性能评估指标

### 6.1 轮廓系数

![image-20220404231422871](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220404231422871.png)

注：对于每个点i 为已聚类数据中的样本 ，b_i 为i 到其它族群的所有样本的距离最小值，a_i 为i 到本身簇的距离平均值。最终计算出所有的样本点的轮廓系数平均值

### 6.2 轮廓系数值分析

![image-20220404231441630](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220404231441630.png)

- 分析过程（我们以一个蓝1点为例）
  - 1、计算出蓝1离本身族群所有点的距离的平均值a_i
  - 2、蓝1到其它两个族群的距离计算出平均值红平均，绿平均，取最小的那个距离作为b_i
  - 根据公式：极端值考虑：如果b_i >>a_i: 那么公式结果趋近于1；如果a_i>>>b_i: 那么公式结果趋近于-1

### 6.3 结论

**如果b_i>>a_i:趋近于1效果越好， b_i<<a_i:趋近于-1，效果不好。轮廓系数的值是介于 [-1,1] ，越趋近于1代表内聚度和分离度都相对较优。**

### 6.4 轮廓系数API

- sklearn.metrics.silhouette_score(X, labels)
  - 计算所有样本的平均轮廓系数
  - X：特征值
  - labels：被聚类标记的目标值

### 6.5 用户聚类结果评估

```python
silhouette_score(cust, pre)
```

## 7、K-means总结

- 特点分析：采用迭代式算法，直观易懂并且非常实用
- 缺点：容易收敛到局部最优解(多次聚类)

> 注意：聚类一般做在分类之前

---



### 案例：

```python
import pandas as pd
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA

# 1、获取数据集
# ·商品信息- products.csv：
# Fields：product_id, product_name, aisle_id, department_id
# ·订单与商品信息- order_products__prior.csv：
# Fields：order_id, product_id, add_to_cart_order, reordered
# ·用户的订单信息- orders.csv：
# Fields：order_id, user_id,eval_set, order_number,order_dow, order_hour_of_day, days_since_prior_order
# ·商品所属具体物品类别- aisles.csv：
# Fields：aisle_id, aisle
from sklearn.metrics import silhouette_score

products = pd.read_csv("../instacart/products.csv")
order_products = pd.read_csv("../instacart/order_products__prior.csv")
orders = pd.read_csv("../instacart/orders.csv")
aisles = pd.read_csv("../instacart/aisles.csv")

# 2、合并表，将user_id和aisle放在一张表上
# 1）合并orders和order_products on=order_id tab1:order_id, product_id, user_id
tab1 = pd.merge(orders, order_products, on=["order_id", "order_id"])
# 2）合并tab1和products on=product_id tab2:aisle_id
tab2 = pd.merge(tab1, products, on=["product_id", "product_id"])
# 3）合并tab2和aisles on=aisle_id tab3:user_id, aisle
tab3 = pd.merge(tab2, aisles, on=["aisle_id", "aisle_id"])

# 3、交叉表处理，把user_id和aisle进行分组
table = pd.crosstab(tab3["user_id"], tab3["aisle"])

# 4、主成分分析的方法进行降维
# 1）实例化一个转换器类PCA
transfer = PCA(n_components=0.95)
# 2）fit_transform
data = transfer.fit_transform(table)

print(data.shape)

# 取500个用户进行测试
# 如果b_i>>a_i:趋近于1效果越好， b_i<<a_i:趋近于-1，效果不好。轮廓系数的值是介于 [-1,1] ，越趋近于1代表内聚度和分离度都相对较优。
cust = data[:500]
km = KMeans(n_clusters=4)
km.fit(cust)
pre = km.predict(cust)
print(silhouette_score(cust, pre))
```



返回结果：

```python
(206209, 44)
0.466014214896049
```



### 几个问题：

#### 1、线性回归的参数求解的方法是什么?

答案: 正规方程和梯度下降

#### 2、什么是过拟合? 原因有哪些?

答案: 过拟合就是训练误差很小,但是测试误差很大

 原因有: 样本偏差, 模型过于复杂

#### 3、分类问题, 回归问题, 聚类问题的评估方法分别是什么?

答案: 分类问题的评估方法是准确率, 精确率和召回率

**回归问题的评估方法是均方差**

**聚类问题的评估方法是轮廓系数**