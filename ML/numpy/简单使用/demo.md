## 数组中数据的读取

```python
import numpy as np
'''
明白二维数组中的取法，就是[x, y]中，x是指的行，y是指的列
和列表中的[x][y]是不同的
'''

file_path = './demo.csv'
# 打开csv文件, unpack就是转置 和 transpose一样
t1 = np.loadtxt(file_path, delimiter=',', dtype='int', unpack=True)
print(t1)
print("*" * 100)
t2 = np.loadtxt(file_path, delimiter=',', dtype='int', unpack=False)
print(t2)

t3 = t2
print(t3.transpose())
print(t3.T)
# 将两个轴交换了一下 所以就是转置
print(t3.swapaxes(1,0))

# 取连续的行操作
print(t2[2:])

# 取不连续的行
print(t2[[1,2,3]])

# 取列操作(取单个多列)
print(t2[ : , 0])
print(t2[:][2])

# 取连续的多列
print(t2[:, 1:])

# 取不连续的多列
print(t2[:, [0,2]])

# 取第几行第几列的值
a = t2[2,3]
print(type(a))

# 取多行多列
# 取第三行到第五行，第二列到第四列的值
print(t2[2:5, 1:4])

# 取单个点
print(t2[0][0]) # 返回的是一个元素
print(t2[[0], [0]]) # 返回的还是一个数组，数组中只有一个元素

# 取多个不相邻的点
print(t2[[0,1], [0,2]]) # 返回的是一个数组，数组中有多个元素


```



## 数组的结构（shape和reshape）

```python

import numpy as np

# shape
array1 = np.array([[1,2,3],[4,5,6]])
print(array1.shape)

array2 = np.array([[2,4],[6,8],[0,12]])
print(np.shape(array2))

# reshape 就是改变原来数组的结构，第一个参数是行，第二个参数是列
array = np.array([[2,4],[6,8],[0,12]])
print(array.reshape((1, 6)))
new_array = array.reshape((1,6))
print(new_array)
```



## 简单的操作

```python
import numpy as np

# 传入列表，得到numpy的array
# 三种过的numpy 数组的方法

# 1.传入列表
li = [1,2,3,4]
t1 = np.array(li)

print(t1)
print(type(t1))
# 2.传入range对象
t2 = np.array(range(1, 5))
print(t2)
print(type(t2))

# 3.直接arrange
t3 = np.arange(1,5)
print(t3)
print(type(t3))
print(t3.dtype)

# 查看数据类型
t4 = np.array(range(1,4), dtype="f2")
print(t4.dtype)

t5 = np.array(range(1,4), dtype=bool)
print(t5)

# 矩阵乘法 @ d.dot


# 数组指定对应的轴进行操作, 轴就是对应的列
a = np.array([[ 0,  1,  2,  3],
              [ 4,  5,  6,  7],
              [ 8,  9, 10, 11]])
print(a.sum(axis=1))

'''
通函数
NumPy提供熟悉的数学函数，例如sin，cos和exp。在NumPy中，这些被称为“通函数”（ufunc）。在NumPy中，这些函数在数组上按元素进行运算，产生一个数组作为输出。
1. exp将元素放在e的几次幂
2. cos sin
3. sqrt跟下
'''
print(np.sqrt(a))
```





## bool索引，以及裁剪

```python
# 布尔索引
t2[t2 < 2] = 100
print(t2[t2 < 2]) # 返回的是一个一维数组

# 三元操作符
a = 3 if 3 > 2 else 2
print(a)

t5 = np.where(t2<3, 100, 300) # 返回一个值
print(t5)


# clip裁剪
print(t2)
t6 = t2.clip(3,10) # 小于3的替换为3，大于10的替换为10, 在 这两个数中间的值不变
print(t6)
# print(t2[1,1])  # 返回的是一个元素
```

## NaN

```python
import numpy as np

'''
NaN是浮点数的一个占位符
inf则是表示无穷大，-inf表示无穷小

np.NaN = np.NaN # return False

'''



# 从文件中读取数据
file_path = './demo.csv'

t = np.loadtxt(file_path, delimiter=',', dtype='int', unpack=False)
t = t.astype(float)

# NaN是一个浮点类型
t[1, 0] = np.NaN
print(t)
```