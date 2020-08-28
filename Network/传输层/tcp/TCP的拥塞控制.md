# 拥塞控制

原因：网络上的堵塞，对资源的需求总和大于可用资源，eg：带宽比较小的情况下



![image-20200614144515153](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200614144515153.png)

理想只是理想，实际的情况只能是适得其反

如果没有拥塞控制的话，可能会出现死锁的情况，

所以拥塞控制就是来避免出现这一种情况。

### 拥塞控制的实现

![image-20200614145020025](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200614145020025.png)

#### 快重传 快恢复

