# java中的几种集合

## 栈 Stack

先进后出

![image-20200713184754298](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185232-478706.png)



## 队列

先进先出

![image-20200713184816351](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185237-364017.png)

## 数组

**特点**：

*查询快* ：数组的地址是连续的，可以通过数组的首地址找到数组，通过数组的索引快速找到某一个元素

*增删慢* ：数组的长度是不变的，增加或则删除那么就必须新创建一个数组，拷贝后删除原来的数组。



## 链表

**特点**：

*查询慢* ：链表中的地址不是连续的，每次查询都必须从头开始

*增删快*：增加或删除一个元素对链表的整体没有什么影响

数组的每一个元素称之为一个节点。

![image-20200713185756010](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185240-46487.png)



## 树

### 二叉树

分支不超过两个

![image-20200713190230282](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185245-839419.png)

#### 排序树/查找树

在二叉树的基础上，元素是有大小顺序的

即左子树小 右子树大

![image-20200713190245285](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185249-321188.png)



#### 平衡树

左孩子和右孩子的数量一样

![image-20200713190447394](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185247-766116.png)

#### 不平衡树

左孩子和右孩子的数量不一样

![image-20200713190511454](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185258-34668.png)

## 红黑树

是一种平衡二叉树

**特点**：趋近于平衡树，查询的速度非常快，查询叶子节点的最大次数和最小次数不超过2倍

![image-20200713190957980](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185253-765666.png)

+ 性质1. 节点是红色或黑色。

- 性质2. 根节点是黑色。

+ 性质3. 每个叶节点（NIL节点，空节点）是黑色的。

- 性质4. 每个红色节点的两个子节点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)

- 性质5. 从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。



## 哈希表结构

**哈希表**：

jdk1.8之前 哈希表=数组+链表

jdk1.8之后 哈希表=数组+红黑树（查询速度快）

![image-20200713210046917](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/185300-41455.png)