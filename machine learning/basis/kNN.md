# k-近邻算法

## 代码实现



**函数实现**

```python
"""
@Author: yanzx
@Date: 2020/7/18 15:06
@Description: kNN
"""

import numpy as np
from math import sqrt
from collections import Counter
from sklearn.neighbors import KNeighborsClassifier

'''
k: 指定k的数量
X_train：x的训练数据
y_train：y的训练数据
x：要预测的数据
'''

def kNN_classify(k, X_train, y_train, x):
    assert 1 <= k <= X_train.shape[0], "k must be valid"
    distances = [sqrt(np.sum((x_train - x) ** 2)) for x_train in X_train]  # 返回一个列表，值对应着之间的距离
    nearest = np.argsort(distances) # 从小到大排序，返回索引
    topK_y = [y_train[i] for i in nearest[:k]]
    # 数量
    votes = Counter(topK_y)
    return votes.most_common(1)[0][0]
```

**类实现**

```python
"""
@Author: yanzx
@Date: 2020/7/18 15:51
@Description: 类实现
"""
from math import sqrt
import numpy as np
from collections import Counter
from matplotlib import pyplot as plt

class KNNClassfier:
    def __init__(self, k):
        """初始化"""
        assert k >= 0, "k must be valid"
        self.k = k
        self._X_train = None
        self._y_train = None

    def fit(self, X_train, y_train):
        assert X_train.shape[0] == y_train.shape[0], "熟练数据x和y和一维大小必须相等"
        assert self.k <= X_train.shape[0], "k一定要小于等于训练数据x的一位值"

        self._X_train = X_train
        self._y_train = y_train
        return self

    def predict(self, X_predict):
        '''预测向量X_predict是一个二维的np.array数组'''
        assert self._X_train is not None and self._y_train is not None, "must be fit before predict"
        assert X_predict.shape[1] == self._X_train.shape[1], "预测的列数和训练集中列的个数应该相同"
        y_predict = [self._predict(x) for x in X_predict]
        return np.array(y_predict)

    def _predict(self, x):
        distance = [sqrt(np.sum((x_train-x)**2)) for x_train in self._X_train]
        np_index = np.argsort(distance)
        y = [self._y_train[index] for index in np_index[:self.k]]
        votes = Counter(y)
        return votes.most_common(1)[0][0]

    def __repr__(self):
        return "KNN(k = %d)" % self.k

def main():
    x1 = np.random.randint(0, 100, size=(50, 2))

    plt.figure(figsize=(20, 10), dpi=80)

    x2 = np.random.randint(100, 200, size=(50, 2))

    x_train_data = np.vstack([x1, x2])
    y1 = np.ones(50, dtype='int64')
    y2 = np.zeros(50, dtype='int64')
    y_train_data = np.hstack([y1, y2])
    plt.scatter(x_train_data[y_train_data == 0, 0], x_train_data[y_train_data == 0, 1])
    plt.scatter(x_train_data[y_train_data == 1, 0], x_train_data[y_train_data == 1, 1])
    # plt.show()

    kNNClassfier = KNNClassfier(6)
    kNNClassfier.fit(x_train_data, y_train_data)
    x_predict = np.array([[50, 60]])

    res = kNNClassfier.predict(x_predict)
    print(res)

if __name__ == '__main__':
    main()




```



## 分类准确度

`分类准确的程度`

[![Jupyter Notebook](http://localhost:8888/static/base/images/logo.png?v=641991992878ee24c6f3826e81054a0f)](http://localhost:8888/tree?token=9f39aca389bf7a2563de92ba93b5cdab9fb3daff7723e3ef)

kNN2(未保存改变)![Current Kernel Logo](http://localhost:8888/kernelspecs/python3/logo-64x64.png)Logout

Python 3 

可信的

- [File](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Edit](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [View](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Insert](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Cell](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Kernel](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Widgets](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)
- [Help](http://localhost:8888/notebooks/MachineLearning/basis/kNN2.ipynb#)

运行

In [1]:

1

```
import numpy as np
```

In [2]:

1

```
import matplotlib
```

In [3]:

1

```
from matplotlib import pyplot as plt
```

2

```
from sklearn import datasets
E:\Anaconda\lib\importlib\_bootstrap.py:219: RuntimeWarning: numpy.ufunc size changed, may indicate binary incompatibility. Expected 192 from C header, got 216 from PyObject
  return f(*args, **kwds)
```

In [20]:

1

```
digits = datasets.load_digits()
```

In [ ]:

1

```

```

In [21]:

1

```
x = digits.data
```

In [ ]:

1

```

```

In [22]:

1

```
y = digits.target
```

In [32]:

1

```
y.shape
```

Out[32]:

```
(1797,)
```

In [23]:

1

```
x.shape
```

Out[23]:

```
(1797, 64
```

In [33]:

1

```
from sklearn.model_selection import train_test_split
```

3

```
X_train, X_test, y_train, y_test = train_test_split(x, y, test_size = 0.2, random_state = 666)
```

In [34]:

1

```
from sklearn.neighbors import KNeighborsClassifier
```

In [35]:

1

```
knn_clf = KNeighborsClassifier(n_neighbors = 3)
```

In [36]:

1

```
knn_clf.fit(X_train, y_train)
```

Out[36]:

```
KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                     metric_params=None, n_jobs=None, n_neighbors=3, p=2,
                     weights='uniform')
```

In [37]:

1

```
y_predict = knn_clf.predict(X_test)
```

In [38]:

1

```
y_predict
```

Out[38]:

```
array([8, 1, 3, 4, 4, 0, 7, 0, 8, 0, 4, 6, 1, 1, 2, 0, 1, 6, 7, 3, 3, 6,
       3, 2, 3, 4, 0, 2, 0, 3, 0, 8, 7, 2, 3, 5, 1, 3, 1, 5, 8, 6, 2, 6,
       3, 1, 3, 0, 0, 4, 9, 9, 2, 8, 7, 0, 5, 4, 0, 9, 5, 5, 5, 7, 4, 2,
       8, 8, 7, 1, 4, 3, 0, 2, 7, 2, 1, 2, 4, 0, 9, 0, 6, 6, 2, 0, 0, 5,
       4, 4, 3, 1, 3, 8, 6, 4, 4, 7, 5, 6, 8, 4, 8, 4, 6, 9, 7, 7, 0, 8,
       8, 3, 9, 7, 1, 8, 4, 2, 7, 0, 0, 4, 9, 6, 7, 3, 4, 6, 4, 8, 4, 7,
       2, 6, 9, 5, 8, 7, 2, 5, 5, 9, 7, 9, 3, 1, 9, 4, 4, 1, 5, 1, 6, 4,
       4, 8, 1, 6, 2, 5, 2, 1, 4, 4, 3, 9, 4, 0, 6, 0, 8, 3, 8, 7, 3, 0,
       3, 0, 5, 9, 2, 7, 1, 8, 1, 4, 3, 3, 7, 8, 2, 7, 2, 2, 8, 0, 5, 7,
       6, 7, 3, 4, 7, 1, 7, 0, 9, 2, 8, 9, 3, 8, 9, 1, 1, 1, 9, 8, 8, 0,
       3, 7, 3, 3, 4, 8, 2, 1, 8, 6, 0, 1, 7, 7, 5, 8, 3, 8, 7, 6, 8, 4,
       2, 6, 2, 3, 7, 4, 9, 3, 5, 0, 6, 3, 8, 3, 3, 1, 4, 5, 3, 2, 5, 6,
       9, 6, 9, 5, 5, 3, 6, 5, 9, 3, 7, 7, 0, 2, 4, 9, 9, 9, 2, 5, 6, 1,
       9, 6, 9, 7, 7, 4, 5, 0, 0, 5, 3, 8, 4, 4, 3, 2, 5, 3, 2, 2, 3, 0,
       9, 8, 2, 1, 4, 0, 6, 2, 8, 0, 6, 4, 9, 9, 8, 3, 9, 8, 6, 3, 2, 7,
       9, 4, 2, 7, 5, 1, 1, 6, 1, 0, 4, 9, 2, 9, 0, 3, 3, 0, 7, 4, 8, 5,
       9, 5, 9, 5, 0, 7, 9, 8])
```

In [39]:

```
from sklearn.metrics import accuracy_score
```



## 寻找最好超参数

**在kNN算法中就是找到最好的k**

用循环进行查找





## 考虑权重

解决问题：平票问题

uniform：默认的

distance：考虑距离的权重



## 曼哈顿距离

![image-20200720153954521](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114733-591137.png)

## 取对应的p

![image-20200720154128448](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114737-279286.png)

## 网格搜索（grid search）



![image-20200720155133327](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114833-208228.png)



## 数据归一化 

**目的**：就是将数据变得更加可靠，避免类似于单位的不统一而产生的距离大小问题

方法：

* min-max标准化（不适用于没有最值的数据） ：

![image-20200720164214064](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114815-562425.png)

* z-score 标准化(zero-mean normalization) 均值方差归一化

![image-20200720164309549](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114811-385921.png)

![image-20200720164810173](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114810-873491.png)



## 总结

![image-20200720171659606](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114817-424809.png)

![image-20200720172111435](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114820-408457.png)