# innodb记录存储结构

## 行格式

`show variables like 'innodb_page_size';` 

页的大小为16k

![image-20211108150010713](https://i.loli.net/2021/11/08/g3iQV9Pw56mIBDG.png)

1. compact
2. redundant

3. dynamic

4. compressed

### compact

![](https://images2015.cnblogs.com/blog/990532/201701/990532-20170116112748427-708866698.png)

compact使得一页中的数据更加紧凑

### redundant

![](https://images2015.cnblogs.com/blog/990532/201701/990532-20170116121716521-2114512946.png)

行溢出，两者都会用20个字节指向溢出页

### Dynamic

溢出时，真实数据列只存储指针，其他和compact基本相同

### compressed

会采用压缩算法对溢出页进行压缩，其他与Dynamic相同。

