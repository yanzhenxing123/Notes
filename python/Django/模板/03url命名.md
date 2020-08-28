### url命名



#### 为什么需要url命名？

因为url是经常变化的，但是他的名字不变，如果在代码中写死的话，修改会非常麻烦，所以取名字后只需将它的名字进行反转就行了，不需要写死url。



#### 如何给一个url指定名称？

在`path`函数传递一个name参数即可，示例代码如下：

```python
urlpatterns = [
    path("", views.index, name='index'),
    path("signin/", views.login, name="login"),
]

```



#### 应用命名空间：

在多个app之间，有可能产生同名的url。这时候为了避免反转url的时候产生混淆，可以使用应用的命名空间。

```
app_name = "front"
```

反转:

```python
login_url = reverse("front:login")
```



#### 应用（app）命名空间和实例命名空间：

一个app，可以创建多个实例。可以使用多个url映射同一个app。所以这就会产生一个问题。以后做反转的时候，如果不使用应用命名空间，那么就会发生混淆。解决方案：

只要在‘include’函数中传递一个namespace即可，代码如下：

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("front.urls")),

    # 同一个app下有两个实例
    path("cms1/", include("cms.urls", namespace="cms1")),
    path("cms2/", include("cms.urls", namespace="cms2")),
 
]

```

以后做反转的时候，就可以根据实力命名空间来指定具体的url。实例代码如下：

```python
def index(request):
    username = request.GET.get("username")
    if username:
        return HttpResponse("CMS首页")
    else:
        current_namespace = request.resolver_match.namespace
        return redirect(reverse("%s:login" % current_namespace))

def login(request):
    return HttpResponse("CMS登录页面")
```

