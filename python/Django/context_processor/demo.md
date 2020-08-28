# Django的学习过程

#### 1.基本命令

python manage.py xxx

#### 2.基本目录

![image-20200510170033339](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200510170033339.png)

#### 3.settings配置文件

BASE_DIR 

INSTALLED_APPS 

MIDDLLEWARE....

#### 路由（urls.py)

**1. 什么是urls.py**

> urls.py本质上就是一个标准的python文件，这个python文件的作用就是在URL请求和处理该请求的视图函数之间建立一个对应关系，换句话说，它就是一个url请求映射表。

**2. urls.py文件位置**

> 除了在项目根目录下有一个urls.py之外，项目的每个应用下都会有一个urls.py配置文件。

**3. urls.py配置格式**

urlpatterns = patterns('视图前缀',

  url(r'^正则表达式1/$', '视图函数1', name="url标识1"),

  url(r'^正则表达式2/$', '视图函数2', name="url标识2"),

)

> patterns函数的第一个参数表示视图前缀，视图前缀可以为空，之后跟上若干个url函数，每个url函数表示一个请求映射关系。

> *注意：*
>
> *3.1 url函数的第二个参数，表示视图函数，它的名字不是随便取的，必须要在views.py中真实存在，项目的每个应用下都会有一个views.py文件。*
>
> *3.2 views.py文件中的视图函数，其第一个参数必须是HttpRequest对象。*
>
> *3.2 name的作用主要体现在一个视图函数对应多个url请求的场景中，name可以用来唯一标识一个url，所以它必须全局唯一。*

**4. urls.py如何工作**

> 前面说过，urls.py本质上就是一个请求映射表，它决定了哪个请求由哪个函数来处理，具体过程如下：
>
> 4.1 浏览器发送请求url
>
> 4.2 服务端根据请求的url，在项目的所有应用（包括根目录）的urls.py配置文件中进行查找，如果能匹配到该url，就会将该url交给其对应的视图函数进行处理。
>
> 4.3 负责处理该url的视图函数，会搜集一些业务数据，然后把这些数据，通过 return render(request, '模板文件', 数据); 渲染到前端页面展示给用户。

front app下：

urls.py

```python
from django.urls import path
from . import views

# 应用命名空间
# 应用命名空间的变量叫做app_name
app_name = 'front'

urlpatterns = [
    path("", views.index, name='index'),
    path("signin/", views.login, name="login"),
]
```

主目录下：

urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # 包含front app下的所有函数
    path("", include("front.urls")),
]
```

#### 视图层(view)

类视图

## 视图函数

**视图函数**，简称视图，属于`Django`的视图层，默认定义在`views.py`文件中，是用来处理`web`请求信息以及返回响应信息的函数，所以研究视图函数只需熟练掌握两个对象即可：请求对象`(HttpRequest)`和响应对象`(HttpResponse)`

## 请求对象`(HttpRequest)`

`django`将`http`协议请求报文中的请求行、首部信息、内容主体封装到了`HttpRequest`对象中（类似于我们自定义框架的`environ`参数）。 `django`会将`HttpRequest`对象当做参数传给视图函数的第一个参数`request`，在视图函数中，通过访问该对象的属性便可以提取`http`协议的请求数据

#### HttpRequest对象常用属性`part1`

1. `HttpRequest.method`
   获取请求使用的方法（值为纯大写的字符串格式）。例如："GET"、"POST"应该通过该属性的值来判断请求方法
2. `HttpRequest.GET`
   值为一个类似于字典的`QueryDict`对象，封装了`GET`请求的所有参数，可通过`HttpRequest.GET.get`(‘键’)获取相对应的值
3. `HttpRequest.POST`
   值为一个类似于字典的`QueryDict`对象，封装了`POST`请求所包含的表单数据，可通过`HttpRequest.POST.get`(‘键’)获取相对应的值



```python
from django.shortcuts import render, redirect, reverse
from django.views.generic import View
from .forms import SignupForm,SigninForm
from .models import User
# 上下文处理器 messages
from django.contrib import messages


def index(request):
    # 实现状态的登录
    # user_id = request.session.get("user_id")
    # context = {}
    # try:
    #     user = User.objects.get(pk=user_id)
    #     context['front_user'] = user
    # except:
    #     pass
    return render(request, 'index.html')

# 登录
class SigninView(View):
    # get请求
    def get(self, request):
        return render(request, "signin.html")

    # post请求
    def post(self, request):
        form = SigninForm(request.POST)
        # 表单验证 只验证符不符合长度格式
        if form.is_valid():
            username = form.cleaned_data.get("username")
            password = form.cleaned_data.get("password")
            user = User.objects.filter(username=username, password=password).first()
            if user:
                request.session['user_id'] = user.id
                return redirect(reverse("index"))
            else:
                print("用户名或者密码错误")
                # messages.add_message(request, messages.INFO, "用户名或者密码错误")
                messages.info(request, "用户名或者密码错误")  # 与上面那一种方式等价，这一个时上面的简写而已
                return redirect(reverse('signin'))
        else:
            # errors = form.errors.get_json_data()
            # print(errors)

            # 调用自己定义的get_error()方法 实现对错误信息的重新处理规划
            errors = form.get_error()
            for error in errors:
                messages.info(request, error)
            # 再重新加载这一个登录页面 这一个方式时get请求
            return redirect(reverse('signin'))

# 注册
class SignupView(View):
    def get(self, request):
        return render(request, "signup.html")

    def post(self, request):
        form = SignupForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect(reverse("index"))
        else:
            errors = form.errors.get_json_data()
            print(errors)
            return redirect(reverse("signup"))

```

#### 模板(template) 

1.模板由HTML代码和逻辑控制代码构成

2.同一个模板可以有多个上下文，可以通过模板对象来渲染多个上下文

3.Django模板解析工作，都是在后台通过正则表达式一起行调用来完成。a

**DTL**

```html
{% extends "base.html" %}

{% block content %}
    <p>书名：{{ book.1 }}</p>
    <p>作者：{{ book.2 }}</p>
    <form action="{% url 'delete_book' %}" method="post">
        <input type="hidden" name="book_id" value="{{ book.0 }}">
        <input type="submit" value="删除">
{#        <input type="submit" value="更改图书名字">#}
    </form>
{% endblock %}
```



```python
from django.template import loader, Context

def index(request):
    ......
    t = loader.get_template("index.html")  # 手动加载模板
    c = Context(
        'app':'app01',
        'user':request.user,
        'id_addr':request.META['REMOTE_ADDR']
        'message':'index view'
         )   # 构建上下文对象
    return t.render(c)  # 渲染模板

def home(request):
    ......
    t = loader.get_template("home.html")  # 手动加载模板
    c = Context(
        'app':'app01',
        'user':request.user,
        'id_addr':request.META['REMOTE_ADDR']
        'message':'home view'
         )   # 构建上下文对象
    return t.render(c)  # 渲染模板
```



#### 模型(model)

**ORM**：作用是在关系型数据库和对象之间作一个映射，这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句打交道，只要像平时操作对象一样操作它就可以了 。

- MVC框架中包括一个重要的部分，就是ORM，它实现了数据模型与数据库的解耦，即数据模型的设计不需要依赖于特定的数据库，通过简单的配置就可以轻松更换数据库
- ORM是“对象-关系-映射”的简称，主要任务是：
  - 根据对象的类型生成表结构
  - 将对象、列表的操作，转换为sql语句
  - 将sql查询到的结果转换为对象、列表
- Django中的模型包含存储数据的字段和约束，对应着数据库中唯一的表

![img](https://images2018.cnblogs.com/blog/1321568/201805/1321568-20180508154058140-1104078946.png)

#### cookie、session

#### 中间件

每一个请求都是先通过中间件中的 process_request 函数，这个函数返回 None 或者 HttpResponse 对象，如果返回前者，继续处理其它中间件，如果返回一个 HttpResponse，就处理中止，返回到网页上。

#### 上下文处理器

上下文处理器是可以返回一些数据，在全局模板中都可以使用。比如登录后的用户信息，在很多页面中都需要使用，那么我们可以放在上下文处理器中，就没有必要在每个视图函数中都返回这个对象。

#### form表单

数据过滤、清洗



