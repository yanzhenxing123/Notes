### 一、什么是中间件

中间件顾名思义，是**介于request与response处理之间的一道处理过程**，相对比较轻量级，并且在全局上改变django的输入与输出。因为改变的是全局，所以需要谨慎实用，用不好会影响到性能

django中间官网定义：

```
Middleware is a framework of hooks into Django’s request/response processing. 
It’s a light, low-level “plugin” system for globally altering Django’s input or output.
```

**中间件位于web服务端与url路由层之间**

### 二、中间件有什么用

如果你想修改请求，例如被传送到view中的**HttpRequest**对象。 或者你想修改view返回的**HttpResponse**对象，这些都可以通过中间件来实现。

可能你还想在view执行之前做一些操作，这种情况就可以用 middleware来实现。

Django默认的中间件：（在django项目的settings模块中，有一个 MIDDLEWARE_CLASSES 变量，其中每一个元素就是一个中间件，如下图）

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
　　# 'app01.mymid.MyMid1',
　　# 'app01.mymid.MyMid2',
]
```

请求进来是自上而下，通过反射找到类，用for循环来执行，可以自定义中间件，但是也要写在MIDDLEWARE中，可以在app01下创建一个mymid.py文件来写我们自定义的中间件

每一个中间件都有具体的功能

### 三、自定义中间件

中间件可以定义五个方法，分别是：（主要的是process_request和process_response）

```python
1、process_request(self,request)

2、process_view(self, request, callback, callback_args, callback_kwargs)

3、process_template_response(self,request,response)

4、process_exception(self, request, exception)

5、process_response(self, request, response)
```

以上方法的返回值可以是None或一个HttpResponse对象，如果是None，则继续按照django定义的规则向后继续执行，如果是HttpResponse对象，则直接将该对象返回给用户。

详细用法见下方：

#### 1、process_request和process_response

当用户发起请求的时候会依次经过所有的的中间件，这个时候的请求时process_request,最后到达views的函数中，views函数处理后，在依次穿过中间件，这个时候是process_response,最后返回给请求者。

![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917194635690-498179443.png)

上述截图中的中间件都是django中的，我们也可以自己定义一个中间件，我们可以自己写一个类，**但是必须继承MiddlewareMixin**

**第一步：导入**

```python
from django.utils.deprecation import MiddlewareMixin
```

**第二步：自定义中间件** 

```python
from django.utils.deprecation import MiddlewareMixin#
from django.shortcuts import HttpResponse
#
class Md1(MiddlewareMixin):
#
    def process_request(self,request):
        print("Md1请求")
 #
    def process_response(self,request,response):
        print("Md1返回")
        return response
#
class Md2(MiddlewareMixin):
#
    def process_request(self,request):
        print("Md2请求")
        #return HttpResponse("Md2中断")
    def process_response(self,request,response):#
        print("Md2返回")
        return response
```

**第三步：在view中定义一个视图函数（index）**

```
def index(request):

    print("view函数...")
    return HttpResponse("OK")
```

**第四步：在settings.py的MIDDLEWARE里注册自己定义的中间件**

![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917195653848-947929594.png)

运行结果：

```
Md1请求#
Md2请求#
view函数...#
Md2返回#
Md1返回#
```

**注意：**如果当请求到达请求2的时候直接不符合条件返回，即return HttpResponse("Md2中断")，程序将把请求直接发给中间件2返回，然后依次返回到请求者，结果如下：

返回Md2中断的页面，后台打印如下：

```
Md1请求#
Md2请求#
Md2返回#
Md1返回#
```

**流程图如下：**

**![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917195902130-1621272051.png)**

由此总结一下：

1. 中间件的process_request方法是在执行视图函数之前执行的。
2. 当配置多个中间件时，会按照MIDDLEWARE中的注册顺序，也就是列表的索引值，从前到后依次执行的。
3. 不同中间件之间传递的request都是同一个对象

多个中间件中的process_response方法是按照MIDDLEWARE中的注册顺序**倒序**执行的，也就是说第一个中间件的process_request方法首先执行，而它的process_response方法最后执行，最后一个中间件的process_request方法最后一个执行，它的process_response方法是最先执行。

#### 2、process_view

process_view(self, request, view_func, view_args, view_kwargs)

该方法有四个参数

request是HttpRequest对象。

view_func是Django即将使用的视图函数。 （它是实际的函数对象，而不是函数的名称作为字符串。）

view_args是将传递给视图的位置参数的列表（无名分组分过来的值）.

view_kwargs是将传递给视图的关键字参数的字典（有名分组分过来的值）。 view_args和view_kwargs都不包含第一个视图参数（request）。

Django会在调用视图函数之前调用process_view方法。

它应该返回None或一个HttpResponse对象。 如果返回None，Django将继续处理这个请求，执行任何其他中间件的process_view方法，然后在执行相应的视图。 如果它返回一个HttpResponse对象，Django不会调用适当的视图函数。 它将执行中间件的process_response方法并将应用到该HttpResponse并返回结果。

```
process_view(self, request, callback, callback_args, callback_kwargs)
```

```
from django.utils.deprecation import MiddlewareMixin
from django.shortcuts import HttpResponse

class Md1(MiddlewareMixin):

    def process_request(self,request):
        print("Md1请求")
        #return HttpResponse("Md1中断")
    def process_response(self,request,response):
        print("Md1返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("Md1view")

class Md2(MiddlewareMixin):

    def process_request(self,request):
        print("Md2请求")
        return HttpResponse("Md2中断")
    def process_response(self,request,response):
        print("Md2返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("Md2view")
```

运行结果：

```
Md1请求#
Md2请求#
Md1view#
Md2view#
view函数...#
Md2返回#
Md1返回#
```

下图进行分析上面的过程：

![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917200211954-1959113588.png)

当最后一个中间的process_request到达路由关系映射之后，返回到中间件1的process_view，然后依次往下，到达views函数，最后通过process_response依次返回到达用户。

process_view可以用来调用视图函数：

```
class Md1(MiddlewareMixin):

    def process_request(self,request):
        print("Md1请求")
        #return HttpResponse("Md1中断")
    def process_response(self,request,response):
        print("Md1返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):

        # return HttpResponse("hello")

        response=callback(request,*callback_args,**callback_kwargs)
        return response
```

运行结果：

```
Md1请求#
Md2请求#
view函数...#
Md2返回#
Md1返回#
```

注意：process_view如果有返回值，会越过其他的process_view以及视图函数，但是所有的process_response都还会执行。

#### 3、process_exception

process_exception(self, request, exception)

该方法两个参数:

一个HttpRequest对象

一个exception是视图函数异常产生的Exception对象。

这个方法只有在视图函数中出现异常了才执行，它返回的值可以是一个None也可以是一个HttpResponse对象。如果是HttpResponse对象，Django将调用模板和中间件中的process_response方法，并返回给浏览器，否则将默认处理异常。如果返回一个None，则交给下一个中间件的process_exception方法来处理异常。它的执行顺序也是按照中间件注册顺序的倒序执行。

```
process_exception(self, request, exception)
```

实例如下：

```
class Md1(MiddlewareMixin):

    def process_request(self,request):
        print("Md1请求")
        #return HttpResponse("Md1中断")
    def process_response(self,request,response):
        print("Md1返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):

        # return HttpResponse("hello")

        # response=callback(request,*callback_args,**callback_kwargs)
        # return response
        print("md1 process_view...")

    def process_exception(self,request,exception):
        print("md1 process_exception...")



class Md2(MiddlewareMixin):

    def process_request(self,request):
        print("Md2请求")
        # return HttpResponse("Md2中断")
    def process_response(self,request,response):
        print("Md2返回")
        return response
    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("md2 process_view...")

    def process_exception(self,request,exception):#只有报错了才会执行exception
        print("md1 process_exception...")javascript:void(0);)
```

```
Md1请求
Md2请求
md1 process_view...
md2 process_view...
view函数...

Md2返回
Md1返回
```

流程图如下：

当views出现错误时：

![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917201528597-447133849.png)

 将md2的process_exception修改如下：

```
  def process_exception(self,request,exception):

        print("md2 process_exception...")
        return HttpResponse("error")
```

运行结果：

```
Md1请求#
Md2请求#
md1 process_view...#
md2 process_view...#
view函数...#
md2 process_exception...#
Md2返回#
Md1返回#
```

#### 4、process_template_response(self,request,response)

该方法对视图函数返回值有要求，必须是一个含有render方法类的对象，才会执行此方法

```
class Test:
    def __init__(self,status,msg):
        self.status=status#
        self.msg=msg#
    def render(self):#
        import json#
        dic={'status':self.status,'msg':self.msg}#

        return HttpResponse(json.dumps(dic))
def index(response):#
    return Test(True,'测试')
```

### 四 中间件应用场景

**1、做IP访问频率限制**

某些IP访问服务器的频率过高，进行拦截，比如限制每分钟不能超过20次。

**2、URL访问过滤**

如果用户访问的是login视图（放过）

如果访问其他视图，需要检测是不是有session认证，已经有了放行，没有返回login，这样就省得在多个视图函数上写装饰器了！

 

作为延伸扩展内容，有余力的同学可以尝试着读一下以下两个自带的中间件：

```
'django.contrib.sessions.middleware.SessionMiddleware',
'django.contrib.auth.middleware.AuthenticationMiddleware',
```

### 五 CSRF_TOKEN跨站请求伪造

**csrf介绍：**

#### **1、什么是csrf**

CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。CSRF通过伪装来自受信任用户的请求来利用受信任的网站，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性

可以这样来理解：
    ***攻击者盗用了你的身份，以你的名义发送恶意请求，对服务器来说这个请求是完全合法的***，但是却完成了攻击者所期望的一个操作，比如以你的名义发送邮件、发消息，盗取你的账号，添加系统管理员，甚至于购买商品、虚拟货币转账等。 如下：其中Web A为存在CSRF漏洞的网站，Web B为攻击者构建的恶意网站，User C为Web A网站的合法用户

#### **2、csrf攻击原理**

**如下图：**

**![img](https://img2018.cnblogs.com/blog/1364097/201809/1364097-20180917213129524-982812929.png)**

从上图可以看出，要完成一次CSRF攻击，受害者必须依次完成两个步骤：
　　1.登录受信任网站A，并在本地生成Cookie。
　　2.在不登出A的情况下，访问危险网站B。

看到这里，你也许会说：“如果我不满足以上两个条件中的一个，我就不会受到CSRF的攻击”。是的，确实如此，但你不能保证以下情况不会发生：
　　1.你不能保证你登录了一个网站后，不再打开一个tab页面并访问另外的网站。
　　2.你不能保证你关闭浏览器了后，你本地的Cookie立刻过期，你上次的会话已经结束。（事实上，关闭浏览器不能结束一个会话，但大多数人都会错误的认为关闭浏览器就等于退出登录/结束会话了......）
　　3.上图中所谓的攻击网站，可能是一个存在其他漏洞的可信任的经常被人访问的网站。

#### **3、csrf攻击防范**

目前防御 CSRF 攻击主要有三种策略：验证 HTTP Referer 字段；在请求地址中添加 token 并验证；在 HTTP 头中自定义属性并验证



   （1）验证 HTTP Referer 字段
    根据 HTTP 协议，在 HTTP 头中有一个字段叫 Referer，它记录了该 HTTP 请求的来源地址。在通常情况下，访问一个安全受限页面的请求来自于同一个网站，比如需要访问 http://bank.example/withdraw?account=bob&amount=1000000&for=Mallory，用户必须先登陆 bank.example，然后通过点击页面上的按钮来触发转账事件。这时，该转帐请求的 Referer 值就会是转账按钮所在的页面的 URL，通常是以 bank.example 域名开头的地址。而如果黑客要对银行网站实施 CSRF 攻击，他只能在他自己的网站构造请求，当用户通过黑客的网站发送请求到银行时，该请求的 Referer 是指向黑客自己的网站。因此，要防御 CSRF 攻击，银行网站只需要对于每一个转账请求验证其 Referer 值，如果是以 bank.example 开头的域名，则说明该请求是来自银行网站自己的请求，是合法的。如果 Referer 是其他网站的话，则有可能是黑客的 CSRF 攻击，拒绝该请求。

​    这种方法的显而易见的好处就是简单易行，网站的普通开发人员不需要操心 CSRF 的漏洞，只需要在最后给所有安全敏感的请求统一增加一个拦截器来检查 Referer 的值就可以。特别是对于当前现有的系统，不需要改变当前系统的任何已有代码和逻辑，没有风险，非常便捷。

​    然而，这种方法并非万无一失。Referer 的值是由浏览器提供的，虽然 HTTP 协议上有明确的要求，但是每个浏览器对于 Referer 的具体实现可能有差别，并不能保证浏览器自身没有安全漏洞。使用验证 Referer 值的方法，就是把安全性都依赖于第三方（即浏览器）来保障，从理论上来讲，这样并不安全。事实上，对于某些浏览器，比如 IE6 或 FF2，目前已经有一些方法可以篡改 Referer 值。如果 bank.example 网站支持 IE6 浏览器，黑客完全可以把用户浏览器的 Referer 值设为以 bank.example 域名开头的地址，这样就可以通过验证，从而进行 CSRF 攻击。

即便是使用最新的浏览器，黑客无法篡改 Referer 值，这种方法仍然有问题。因为 Referer 值会记录下用户的访问来源，有些用户认为这样会侵犯到他们自己的隐私权，特别是有些组织担心 Referer 值会把组织内网中的某些信息泄露到外网中。因此，用户自己可以设置浏览器使其在发送请求时不再提供 Referer。当他们正常访问银行网站时，网站会因为请求没有 Referer 值而认为是 CSRF 攻击，拒绝合法用户的访问。

​    （2）在请求地址中添加 token 并验证
​     CSRF 攻击之所以能够成功，是因为黑客可以完全伪造用户的请求，该请求中所有的用户验证信息都是存在于 cookie 中，因此黑客可以在不知道这些验证信息的情况下直接利用用户自己的 cookie 来通过安全验证。要抵御 CSRF，关键在于在请求中放入黑客所不能伪造的信息，并且该信息不存在于 cookie 之中。可以在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求。

​    这种方法要比检查 Referer 要安全一些，token 可以在用户登陆后产生并放于 session 之中，然后在每次请求时把 token 从 session 中拿出，与请求中的 token 进行比对，但这种方法的难点在于如何把 token 以参数的形式加入请求。对于 GET 请求，token 将附在请求地址之后，这样 URL 就变成 http://url?csrftoken=tokenvalue。 而对于 POST 请求来说，要在 form 的最后加上 <input type=”hidden” name=”csrftoken” value=”tokenvalue”/>，这样就把 token 以参数的形式加入请求了。但是，在一个网站中，可以接受请求的地方非常多，要对于每一个请求都加上 token 是很麻烦的，并且很容易漏掉，通常使用的方法就是在每次页面加载时，使用 javascript 遍历整个 dom 树，对于 dom 中所有的 a 和 form 标签后加入 token。这样可以解决大部分的请求，但是对于在页面加载之后动态生成的 html 代码，这种方法就没有作用，还需要程序员在编码时手动添加 token。

​     该方法还有一个缺点是难以保证 token 本身的安全。特别是在一些论坛之类支持用户自己发表内容的网站，黑客可以在上面发布自己个人网站的地址。由于系统也会在这个地址后面加上 token，黑客可以在自己的网站上得到这个 token，并马上就可以发动 CSRF 攻击。为了避免这一点，系统可以在添加 token 的时候增加一个判断，如果这个链接是链到自己本站的，就在后面添加 token，如果是通向外网则不加。不过，即使这个 csrftoken 不以参数的形式附加在请求之中，黑客的网站也同样可以通过 Referer 来得到这个 token 值以发动 CSRF 攻击。这也是一些用户喜欢手动关闭浏览器 Referer 功能的原因。

   （3）在 HTTP 头中自定义属性并验证
    这种方法也是使用 token 并进行验证，和上一种方法不同的是，这里并不是把 token 以参数的形式置于 HTTP 请求之中，而是把它放到 HTTP 头中自定义的属性里。通过 XMLHttpRequest 这个类，可以一次性给所有该类请求加上 csrftoken 这个 HTTP 头属性，并把 token 值放入其中。这样解决了上种方法在请求中加入 token 的不便，同时，通过 XMLHttpRequest 请求的地址不会被记录到浏览器的地址栏，也不用担心 token 会透过 Referer 泄露到其他网站中去。

#### 在form表单中应用：

```
<form action="" method="post">
    {% csrf_token %}
    <p>用户名：<input type="text" name="name"></p>
    <p>密码：<input type="text" name="password"></p>
    <p><input type="submit"></p>
</form>
```

#### 在Ajax中应用：

#### 放在data里：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="/static/jquery-3.3.1.js"></script>
    <title>Title</title>
</head>
<body>
<form action="" method="post">
    {% csrf_token %}
    <p>用户名：<input type="text" name="name"></p>
    <p>密码：<input type="text" name="password" id="pwd"></p>
    <p><input type="submit"></p>
</form>
<button class="btn">点我</button>
</body>
<script>
    $(".btn").click(function () {
        $.ajax({
            url: '',
            type: 'post',
            data: {
                'name': $('[name="name"]').val(),
                'password': $("#pwd").val(),
                'csrfmiddlewaretoken': $('[name="csrfmiddlewaretoken"]').val()
            },
            success: function (data) {
                console.log(data)
            }

        })
    })
</script>
</html>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

放在cookie里：

获取cookie：document.cookie

是一个字符串，可以自己用js切割，也可以用jquery的插件

获取cookie：$.cookie('csrftoken')

设置cookie：$.cookie('key','value')

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) 放在cookie里

#### 其它操作

全站禁用：注释掉中间件 'django.middleware.csrf.CsrfViewMiddleware',

局部禁用：用装饰器（在FBV中使用）

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
from django.views.decorators.csrf import csrf_exempt,csrf_protect
# 不再检测，局部禁用（前提是全站使用）
# @csrf_exempt
# 检测，局部使用（前提是全站禁用）
# @csrf_protect
def csrf_token(request):#
    if request.method=='POST':
        print(request.POST)

        return HttpResponse('ok')
    return render(request,'csrf_token.html')#
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在CBV中使用：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# CBV中使用
from django.views import View
from django.views.decorators.csrf import csrf_exempt,csrf_protect
from django.utils.decorators import method_decorator
# CBV的csrf装饰器，只能加载类上（django的bug）
# 给get方法使用csrf_token检测
@method_decorator(csrf_exempt,name='get')，#不能直接放在函数上，可以放在分发函数dispatch上不需要指定名字，是什么请求就会分发到指定的函数上
# 给post加
@method_decorator(csrf_exempt,name='post')#
# 给所有方法加
@method_decorator(csrf_exempt,name='get')#
class Foo(View):
    def get(self,request):#
        pass
    def post(self,request):#
        pass
```