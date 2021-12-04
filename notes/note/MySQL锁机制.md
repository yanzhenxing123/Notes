# MySQL锁机制

## lock和latch

lock对象事务

latch对象线程

![img](https://images2015.cnblogs.com/blog/754297/201601/754297-20160131225332443-857830570.jpg)

查看状态的命令

lock：`show engine innodb status;`

latch: `show engine innodb mutex`



## innodb存储引擎中的锁

行级锁：

+ 共享锁（S Lock）
+ 排他锁（X Lock）

**锁的兼容性**

| -    | X      | S      |
| ---- | ------ | ------ |
| X    | 不兼容 | 不兼容 |
| S    | 不兼容 | 兼容   |



数据库层次

![image-20210609104709324](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202106/09/104709-530682.png)

