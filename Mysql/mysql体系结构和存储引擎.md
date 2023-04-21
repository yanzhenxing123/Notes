

## 存储引擎之间的比较

```sql
show engines\G;
```

![da](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/09/202007-450900.png)

使用不同存储的存储引擎存储相同的数据，所得到的表的大小是不一样的，

```sql
mysql> create table demo01 engine = myisam
    -> as select * from weibo;
```

