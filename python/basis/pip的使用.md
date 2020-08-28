# pip的使用

## 国内源:

- 清华：[https://pypi.tuna.tsinghua.edu.cn/simple](https://links.jianshu.com/go?to=https%3A%2F%2Fpypi.tuna.tsinghua.edu.cn%2Fsimple)
- 阿里云：[http://mirrors.aliyun.com/pypi/simple/](https://links.jianshu.com/go?to=http%3A%2F%2Fmirrors.aliyun.com%2Fpypi%2Fsimple%2F)
- 中国科技大学 [https://pypi.mirrors.ustc.edu.cn/simple/](https://links.jianshu.com/go?to=https%3A%2F%2Fpypi.mirrors.ustc.edu.cn%2Fsimple%2F)
- 华中理工大学：[http://pypi.hustunique.com/](https://links.jianshu.com/go?to=http%3A%2F%2Fpypi.hustunique.com%2F)
- 山东理工大学：[http://pypi.sdutlinux.org/](https://links.jianshu.com/go?to=http%3A%2F%2Fpypi.sdutlinux.org%2F)
- 豆瓣：[http://pypi.douban.com/simple/](https://links.jianshu.com/go?to=http%3A%2F%2Fpypi.douban.com%2Fsimple%2F)

> 新版ubuntu要求使用https源，要注意。

## 临时使用

可以在使用pip的时候加参数`-i https://pypi.tuna.tsinghua.edu.cn/simple`
 例如：`pip install -i http://mirrors.aliyun.com/pypi/simple/ tensorflow`，这样就会从阿里云这边的镜像去安装tensorflow库。

## 永久使用

在Linux下, 修改 `~/.pip/pip.conf` (没有就创建一个文件夹及文件。文件夹要加“.”，表示是隐藏文件夹)

内容如下：



```csharp
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

windows下，直接在user目录中创建一个pip目录，再新建文件pip.ini。（例如：C:\Users\WQP\pip\pip.ini）内容同上。

![gratisography-wild-t-rex (1)](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/23/155029-880287.jpeg)