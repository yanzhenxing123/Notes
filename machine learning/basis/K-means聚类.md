

## 前言

聚类（clusting）属于非监督学习（unsupervised learning），无类别标记（class label）。

> k均值聚类算法（k-means clustering algorithm）是一种迭代求解的聚类分析算法，其步骤是随机选取K个对象作为初始的聚类中心，然后计算每个对象与各个种子聚类中心之间的距离，把每个对象分配给距离它最近的聚类中心。

## 一、Kmeans介绍

算法接受参数k，然后将事先输入的n个数据划分为k个聚类以便使得所获得的聚类满足同一聚类中的对象相似度高，而不同聚类中的相似度低。以空间中k个中心进行聚类，对最靠近他们的对象归类，通过迭代的方法，逐次更新聚类中心的值，直至得到最好的聚类结果。

给定样本集 ![[公式]](https://www.zhihu.com/equation?tex=D%3D%5C%7B+x_%7B1%7D%2C+x_%7B2%7D%2C+%5Cdots+x_%7Bn%7D+%5C%7D) ，“k均值”（kmeans）算法对聚类所得簇划分 ![[公式]](https://www.zhihu.com/equation?tex=C+%3D+%5C%7B++C_%7B1%7D%2C+C_%7B2%7D%2C+%5Cdots%2C++C_%7Bk%7D+%5C%7D) 最小化平方误差：

![[公式]](https://www.zhihu.com/equation?tex=E+%3D+%5Csum_%7Bi%3D1%7D%5E%7Bk%7D+%5Csum_%7Bx+%5Cin+C_%7Bi%7D%7D+%5Cparallel+x+-+%5Cmu_%7Bi%7D+%5Cparallel+_%7B2%7D%5E2)

其中， ![[公式]](https://www.zhihu.com/equation?tex=%5Cmu_%7Bi%7D+%3D+%5Cfrac%7B1%7D%7B%5Cmid+C_%7Bi%7D+%5Cmid%7D+%5Csum_%7Bx+%5Cin+C_%7Bi%7D%7Dx) 是簇 ![[公式]](https://www.zhihu.com/equation?tex=C_%7Bi%7D) 的均值向量。E在一定程度上刻画了簇内样本围绕簇均值向量的紧密程度，E越小则簇内样本相似度越高。

- 算法描述：

（1）适当选择c个类的初始中心；

（2）在k次迭代中，对任意一个样本，求其到c各中心的距离，将该样本归到距离更短的中心所在的类；

（3）利用均值等方法更新该类的中心值；

（4）对于所有的c个聚类中心，如果利用（2）（3）的迭代法更新后，值保持不变，则迭代结束，否则继续迭代。

## 二、相似度的度量

**1、有序距离属性**

(1)欧式距离： ![[公式]](https://www.zhihu.com/equation?tex=d_%7B12%7D+%3D+%5Csqrt%7B%28x_%7B1%7D-x_%7B2%7D%29%5E2+%2B+%28y_%7B1%7D-y_%7B2%7D%29%5E2%7D)

(2)曼哈顿距离 ![[公式]](https://www.zhihu.com/equation?tex=d_%7B12%7D+%3D+%5Cmid+x_1-x_%7B2%7D+%5Cmid+%2B+%5Cmid+y_1+-+y_2+%5Cmid)

(3)切比雪夫距离 ![[公式]](https://www.zhihu.com/equation?tex=d_%7B12%7D+%3D+max+%5C%7B+%5Cmid+x_1-x_%7B2%7D+%5Cmid+%2C+%5Cmid+y_1+-+y_2+%5Cmid+%5C%7D)

(4)闵可夫斯基距离： ![[公式]](https://www.zhihu.com/equation?tex=d+%3D+%28%5Csum_%7Bu%3D1%7D%5En+%5Cmid+x%5E%7B%28i%29%7D+-+x%5E%7B%28j%29%7D+%5Cmid+%5Ep%29%5E%7B%5Cfrac%7B1%7D%7Bp%7D%7D)

(5)余弦距离： ![[公式]](https://www.zhihu.com/equation?tex=cos+%5Ctheta+%3D+%5Cfrac%7Bx_%7B1%7Dx_%7B2%7D+%2B+y_%7B1%7Dy_%7B2%7D%7D+%7B%5Csqrt+%7Bx_%7B1%7D%5E%7B2%7D+%2B+y_%7B1%7D%5E2%7D+%5Csqrt+%7Bx_%7B2%7D%5E%7B2%7D+%2B+y_%7B2%7D%5E2+%7D%7D)

(6)Jaccard相似系数： ![[公式]](https://www.zhihu.com/equation?tex=J%28A%2CB%29+%3D+%5Cfrac%7B%5Cmid+A+%5Ccap+B+%5Cmid%7D%7B%5Cmid+A+%5Ccup+B+%5Cmid%7D)

(7)相似系数： ![[公式]](https://www.zhihu.com/equation?tex=%5Crho_%7BXY%7D+%3D+%5Cfrac%7BCov%28X%2CY%29%7D%7B%5Csqrt%7BD%28X%29%7D+%5Csqrt+%7BD%28Y%29%7D%7D+%3D+%5Cfrac%7B%28X-E%28X%29%29%28Y-E%28Y%29%29%7D%7B%5Csqrt%7BD%28X%29%7D+%5Csqrt+%7BD%28Y%29%7D%7D)

**2、无序距离属性**

**VDM（value Difference Metric）：**

![[公式]](https://www.zhihu.com/equation?tex=VDM_%7Bp%7D%28a%2Cb%29+%3D+%5Csum_%7Bi%3D1%7D%5Ek+%5Cmid+%5Cfrac%7Bm_%7Bu%2Ca%2Ci%7D%7D%7Bm_%7Bu%2Ca%7D%7D+-+%5Cfrac%7Bm_%7Bu%2Cb%2Ci%7D%7D%7Bm_%7Bu%2Cb%7D%7D+%5Cmid+%5Ep)

其中， ![[公式]](https://www.zhihu.com/equation?tex=m_%7Bu%2Ca%7D+) 表示在属性 ![[公式]](https://www.zhihu.com/equation?tex=u) 上取值为 ![[公式]](https://www.zhihu.com/equation?tex=a) 的样本数， ![[公式]](https://www.zhihu.com/equation?tex=m_%7Bu%2Ca%2Ci%7D) 表示在第 ![[公式]](https://www.zhihu.com/equation?tex=i) 个样本簇中再属性 ![[公式]](https://www.zhihu.com/equation?tex=u) 上取值为 ![[公式]](https://www.zhihu.com/equation?tex=a) 的样本数， ![[公式]](https://www.zhihu.com/equation?tex=k) 为样本簇数，则属性 ![[公式]](https://www.zhihu.com/equation?tex=u) 在两个离散值 ![[公式]](https://www.zhihu.com/equation?tex=a%EF%BC%8Cb) 之间的VDM距离如上式。

**3、混合属性**

将闵可夫斯基距离和VDM结合即可处理混合属性，假设有 ![[公式]](https://www.zhihu.com/equation?tex=n_%7Bc%7D) 个有序属性、 ![[公式]](https://www.zhihu.com/equation?tex=n-n_%7Bc%7D) 个无序属性，则：

![[公式]](https://www.zhihu.com/equation?tex=MinkovDM_%7Bp%7D%28x_%7Bi%7D%2Cx_%7Bj%7D%29%3D%28%5Csum_%7Bu%3D1%7D%5E%7Bn_c%7D+%5Cmid+x_%7Biu%7D+-+x_%7Bju%7D+%5Cmid+%5Ep+%2B+%5Csum_%7Bu%3Dn_c%2B1%7D%5E%7Bn%7D+VDM_%7Bp%7D%28x_%7Biu%7D%2C+x_%7Bju%7D%29%29%5E%7B%5Cfrac%7B1%7D%7Bp%7D%7D)

## 三、Kmeans的计算过程

![img](https://pic2.zhimg.com/80/v2-cbac9fd43448af575b97a9f2f5dc324d_720w.png)

现在有4组数据，每组数据有2个维度，对其进行聚类分为2类，将其可视化一下。 ![[公式]](https://www.zhihu.com/equation?tex=A%3D%281%2C1%29%2CB%3D%282%2C1%29%2CC%3D%284%2C3%29%2CD%3D%285%2C4%29)

![img](https://pic3.zhimg.com/80/v2-0b2558d13b662cd610c9d570c707474a_720w.jpg)

假设选取两个星的位置为初始中心 ![[公式]](https://www.zhihu.com/equation?tex=c_1%3D%281%2C1%29%2Cc_2%3D%282%2C1%29) ，计算每个点到初始中心的距离，使用欧式距离得到4个点分别距离两个初始中心的距离，归于最近的类：

![img](https://pic1.zhimg.com/80/v2-f08382d95db7652bfedfb9ae77fc467c_720w.jpg)

通过比较，将其进行归类。并使用平均法更新中心位置。

![img](https://pic2.zhimg.com/80/v2-891961a34a9bf606a1668840d3712889_720w.png)

由于归于group1的只有一个点，一次更新后的中心位置 ![[公式]](https://www.zhihu.com/equation?tex=c_1%3D%281%2C1%29) ，而 ![[公式]](https://www.zhihu.com/equation?tex=c_%7B2%7D+%3D+%28%5Cfrac%7B11%7D%7B3%7D%2C+%5Cfrac%7B8%7D%7B3%7D%29)

![img](https://pic4.zhimg.com/80/v2-c078de3b55027d76c44497feca141b4b_720w.png)

![img](https://pic4.zhimg.com/80/v2-37ee58003a132be1eae2a2c8e6dc27b3_720w.jpg)

再次计算每个点与更新后的位置中心的距离

![img](https://pic2.zhimg.com/80/v2-0d92f0fd2a6196dca654fe27b7b3fa11_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-9759ac417a5b3083d2350a5dbac8bfe1_720w.png)

![img](https://pic1.zhimg.com/80/v2-457006536f3c23a83601e18353250df4_720w.png)

继续迭代下去，

![img](https://pic3.zhimg.com/80/v2-f244bd21254ab2a604f36db0e80fa716_720w.jpg)

![img](https://pic1.zhimg.com/80/v2-ab5c3ef5ef3ed5d5f82e1ab5fb7aba88_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-22f5f0195b8ee8e5a4ada108a0f8ace6_720w.png)

此时，与上一次的类别标记无变化，即可停止。

## 四、Kmeans的编程实现

```python
import numpy as np

def kmeans(X, k, maxIt):
    
    numPoints, numDim = X.shape
    # 给数据标签留个位置
    dataSet = np.zeros((numPoints, numDim + 1))
    dataSet[:, :-1] = X
    
    # 随机初始化位置中心
    centroids = dataSet[np.random.randint(numPoints, size = k), :]
    centroids = dataSet[0:2, :]
    # 随机给初始位置中心赋予类别标记
    centroids[:, -1] = range(1, k +1)
    
    iterations = 0
    oldCentroids = None
    
    # Run the main k-means algorithm
    while not shouldStop(oldCentroids, centroids, iterations, maxIt):
        print("iteration: \n", iterations)
        print("dataSet: \n", dataSet)
        print("centroids: \n", centroids)
        # Save old centroids for convergence test. Book keeping.
        oldCentroids = np.copy(centroids)
        iterations += 1
        
        # Assign labels to each datapoint based on centroids
        updateLabels(dataSet, centroids)
        
        # Assign centroids based on datapoint labels
        centroids = getCentroids(dataSet, k)
        
    # We can get the labels too by calling getLabels(dataSet, centroids)
    return dataSet

def shouldStop(oldCentroids, centroids, iterations, maxIt):
    if iterations > maxIt:
        return True
    return np.array_equal(oldCentroids, centroids)  

def updateLabels(dataSet, centroids):
    # For each element in the dataset, chose the closest centroid. 
    # Make that centroid the element's label.
    numPoints, numDim = dataSet.shape
    for i in range(0, numPoints):
        dataSet[i, -1] = getLabelFromClosestCentroid(dataSet[i, :-1], centroids)
        
def getLabelFromClosestCentroid(dataSetRow, centroids):
    label = centroids[0, -1];
    minDist = np.linalg.norm(dataSetRow - centroids[0, :-1])
    for i in range(1 , centroids.shape[0]):
        dist = np.linalg.norm(dataSetRow - centroids[i, :-1])
        if dist < minDist:
            minDist = dist
            label = centroids[i, -1]
    print("minDist:", minDist)
    return label

def getCentroids(dataSet, k):
    # Each centroid is the geometric mean of the points that
    # have that centroid's label. Important: If a centroid is empty (no points have
    # that centroid's label) you should randomly re-initialize it.
    result = np.zeros((k, dataSet.shape[1]))
    for i in range(1, k + 1):
        oneCluster = dataSet[dataSet[:, -1] == i, :-1]
        result[i - 1, :-1] = np.mean(oneCluster, axis = 0)
        result[i - 1, -1] = i
    
    return result

x1 = np.array([1, 1])
x2 = np.array([2, 1])
x3 = np.array([4, 3])
x4 = np.array([5, 4])
testX = np.vstack((x1, x2, x3, x4))

result = kmeans(testX, 2, 10)
print("final result:")
print(result)
# final result:
# [[1. 1. 1.]
#  [2. 1. 1.]
#  [4. 3. 2.]
#  [5. 4. 2.]]
```



我是尾巴

2020年1月26日，新型冠状病毒肺炎全国确诊2070例，疑似病例2648例，治愈49，死亡56，中国加油~

本次推荐：

[黄海广：Python代码写得丑怎么办？推荐几个神器拯救你zhuanlan.zhihu.com![图标](https://zhstatic.zhihu.com/assets/zhihu/editor/zhihu-card-default.svg)](https://zhuanlan.zhihu.com/p/59763076)

继续加油~

编辑于 04-07

[聚类算法](https://www.zhihu.com/topic/19627785)

[机器学习](https://www.zhihu.com/topic/19559450)

[聚类](https://www.zhihu.com/topic/19590190)

赞同 2添加评论

分享

喜欢收藏申请转载



### 推荐阅读

- ![K-means和谱聚类的比较](https://pic4.zhimg.com/v2-a649cfb84acf70afa0b58e7522bab612_250x0.jpg?source=172ae18b)

- # K-means和谱聚类的比较

- Biowood

- # k均值聚类、模糊的c均值聚类算法

- K均值聚类（K-means）：硬聚类算法，隶属度取0或1，类内误差平方和最小化。 模糊的c均值聚类(FCM)：模糊聚类算法，隶属度取[0,1]，类内加权误差平方和最小化。 1.K-means聚类算法 先随机选…

- 枫叶

- ![常用聚类算法](https://pic4.zhimg.com/v2-99863816ea8d29383c5ff28a8b7a667e_250x0.jpg?source=172ae18b)

- 