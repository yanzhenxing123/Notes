#### 创建项目

1.通过命令进行创建：

```
​```
django-admin startproject [项目名称]
​```
```

2.通过pycharm创建：

文件 新建 项目选择Django即可



#### 项目结构

1.“manage.py”:管理用的，一般用处都是 python manage.py  xxx

2.“urls.py": url 和视图函数映射的，以后来了一个请求，就会从这里找到匹配的函数

3.”wsig.py":专门用来做部署的，不需要来更改



#### django推荐的项目规范：

1.按照功能或者模块进行分层，分成一个个app，所有的和莫格模块相关的试图视图都写在对应的app的views.py中，并且模型和其他的也类似。

2.命令操作：cmd进入项目，输入：“python manage.py startapp xxx”。



#### DEBUG模式：

1.如果开启了debug模式，以后修改了Django项目中的代码，然后 按下“ctal+s“ ，那么Django就会自动重启项目，不需要手动重启。

2.开启以后，如果代码出现了bug，那么在浏览器和控制台会打印出错信息，方便找出错误

3.如果在真正的生产环境中，那么要关闭。

4.如果关闭了，必须要修改设置ALLOWED_HOSTS



#### ALLOWED_HOSTS：

这个变量是用来设置以后被人只能通过这个变量中的IP地址或者域名进行访问



#### 视图函数：

1.视图函数的第一个参数必须是request，这个参数不能少

2.视图函数的返回值必须是”django.http.response.HttpResponseBase“的子类的对象。





