### url传递参数



#### url映射：

1.为什么回去urls.py中去寻找映射？

是因为在“settings.py"文件中配置了”ROOT_URLCONF“为”urls.py“，所有的django回去”urls.py“中去寻找。

2.”urls.py“中我们的所有映射都应该放在”urlpatterns”这个变量中。

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    # http://127:0.0.1:8000
    path('', index),
    # http://127:0.0.1:8000/book
    path('book/', book),
    # http://127:0.0.1:8000/movie
    path('movie/', movie)

]
```

3.所有的映射不是随便写的，而是使用’path‘函数或者是're_path'函数进行包装的。



#### url中传递参数：

1.采用在url中使用变量的方式：在url中以<xxx>作为函数的传递， 在视图函数中也要进行编写，需要定义形参并进行返回，视图函数中的参数要和path中的变量名称要一样。

2.采用查询字符串的方式：在url中，不需要单独的匹配查询字符串的部分，只需要在视图函数中使用

`request.GET.get("参数名称")的方式进行获取，示例代码如下：`

views.py:

```python
def book_detail(request, book_id, category_id):
    # 可以从数据库中根据book_id获取这个图书的信息
    text = "你获取的图书id是：%s, 图书分类是：%s" % (book_id, category_id)
    return HttpResponse(text)

def author_detail(request):
    # get 相当于就是获取字典
    author_id = request.GET.get("id")
    text = "作者的id是：%s" % author_id
    return HttpResponse(text)

```

urls.py:

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    # http://127:0.0.1:8000
    path('', index),
    # http://127:0.0.1:8000/book
    path('book/', views.book),
    path('book/detail/<book_id>/<category_id>/', views.book_detail),

    path('book/author/', views.author_detail)

]
```

get是一个类似于字典的类型。



#### url模块化

如果项目变得越来越大，那么url会变得越来越多，因此可以进行如下操作：

一般我们会在app中新建一个urls.py文件用来储存所有和这个app相关的子url。

注意：

1.应该使用`include`函数包含子”urls.py",并且这个“urls.py"的路径是相对于项目的路径。

示例代码：

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('book/', include("book.urls"))
]
```

2.在”app“的”urls.py"中，所有的url‘匹配也要放在一个叫做“urlpatterns”的变量中，否则找不到。

3.拼接时的斜杠问题。





