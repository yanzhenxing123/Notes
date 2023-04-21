# 主成分分析 PCA

## basis

**高维和低维指的是特征数量而不是矩阵的维度**

比如下面这个X数据：

```python
X.shape = (m, n)
X.shape[0] = m # 代表的意思就是这个数据集中含有m个数据
X.shape[1] = n # 代表的意思就是这个数据集中的每一个数据就有n个特征值

"""
所以应该有对应的y, 它的shape应该为(m, ), 代表的就是对应的目标值
""" 
```

所以下面的公式的意思就很好理解了：
$$
X·W_k^T = X_k
$$
*代表的意思就是*：将X的特征值减少到k个

即 **降维处理**



![image-20200722143156611](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114923-685042.png)





### PCA类

import numpy as np


class PCA:

```python
def __init__(self, n_components):
    """初始化PCA"""
    assert n_components >= 1, "n_components must be valid"
    self.n_components = n_components
    self.components_ = None

def fit(self, X, eta=0.01, n_iters=1e4):
    """获得数据集X的前n个主成分"""
    assert self.n_components <= X.shape[1], \
        "n_components must not be greater than the feature number of X"

    # 将数据的平均值设置为0
    def demean(X):
        return X - np.mean(X, axis=0)
    
    # 损失函数
    def f(w, X):
        return np.sum((X.dot(w) ** 2)) / len(X)

    # 损失函数的梯度
    def df(w, X):
        return X.T.dot(X.dot(w)) * 2. / len(X)

    # w的方向，及将w单位化
    def direction(w):
        return w / np.linalg.norm(w)

    # 第一个特征分量w
    def first_component(X, initial_w, eta=0.01, n_iters=1e4, epsilon=1e-8):

        w = direction(initial_w)
        cur_iter = 0

        while cur_iter < n_iters:
            gradient = df(w, X)
            last_w = w
            w = w + eta * gradient
            w = direction(w)
            if (abs(f(w, X) - f(last_w, X)) < epsilon):
                break

            cur_iter += 1

        return w

    X_pca = demean(X)
    self.components_ = np.empty(shape=(self.n_components, X.shape[1]))
    for i in range(self.n_components):
        initial_w = np.random.random(X_pca.shape[1])
        w = first_component(X_pca, initial_w, eta, n_iters)
        self.components_[i,:] = w

        # 将X进行向量化
        X_pca = X_pca - X_pca.dot(w).reshape(-1, 1) * w
    return self

def transform(self, X):
    """将给定的X，映射到各个主成分分量中"""
    assert X.shape[1] == self.components_.shape[1]

    return X.dot(self.components_.T)

# 或多或少会缺失一定的数据
def inverse_transform(self, X):
    """将给定的X，反向映射回原来的特征空间"""
    assert X.shape[1] == self.components_.shape[0]

    return X.dot(self.components_)

def __repr__(self):
    return "PCA(n_components=%d)" % self.n_components
```


## 梯度上升法解决主成分分析

![image-20200722102419972](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/114929-76416.png)

![image-20200722102447523](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200722102447523.png)



## MINST

```
import numpy as np
```

```
from sklearn.datasets import fetch_mldata
```

```
mnist = fetch_mldata("MNIST original", data_home='./datasets')
```

```
mnist
```

```
{'DESCR': 'mldata.org dataset: mnist-original',
 'COL_NAMES': ['label', 'data'],
 'target': array([0., 0., 0., ..., 9., 9., 9.]),
 'data': array([[0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        ...,
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0],
        [0, 0, 0, ..., 0, 0, 0]], dtype=uint8)}
```

In [27]:

```
X, y = mnist['data'], mnist['target']
```

In [29]:

```
X_train = np.array(X[:60000], dtype=float)
```

In [37]:

```
y_train = np.array(y[:60000], dtype=float)
```

```
X_test = np.array(X[60000:], dtype=float)
```

```
y_test = np.array(y[60000:], dtype=float)
```

In [33]:

```
X_train.shape
```

Out[33]:

```
(60000, 784)
```

### 使用kNN

In [34]:

```
from sklearn.neighbors import KNeighborsClassifier
```

In [35]:

```
knn_clf = KNeighborsClassifier()
```

In [36]:

```
%time knn_clf.fit(X_train, y_train)
Wall time: 25.3 s
```

Out[36]:

```
KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                     metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                     weights='uniform')
```

In [38]:

1

```
%time knn_clf.score(X_test, y_test)
Wall time: 10min 36s
```

Out[38]:

```
0.9688
```

### PCA降维操作

In [42]:

1

```
from sklearn.decomposition import PCA
```

In [44]:

1

```
pca = PCA(0.9)
```

2

```
pca.fit(X_train)
```

3

```
X_train_reduction = pca.transform(X_train)
```

In [56]:

1

```
X_train_reduction.shape
```

Out[56]:

```
(60000, 87)
```

In [57]:

1

```
knn_clf = KNeighborsClassifier()
```

2

```
%time knn_clf.fit(X_train_reduction, y_train)
Wall time: 528 ms
```

Out[57]:

```
KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                     metric_params=None, n_jobs=None, n_neighbors=5, p=2,
                     weights='uniform')
```

In [58]:

1

```
X_test_reduction = pca.transform(X_test)
```

In [62]:

1

```
%time knn_clf.score(X_test_reduction, y_test) # 可以发现不仅时间减少了 而且训练的准确度页增加了 可见 pca具有一定的降噪功能
Wall time: 1min 25s
```

Out[62]:

```
0.9728
```



## 人脸识别 特征脸

