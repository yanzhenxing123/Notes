

## cookie与session的实现原理

 

![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160316150452615-1393868360.jpg)

　　

　　HTTP被设计为”无状态”，每次请求都处于相同的空间中。 在一次请求和下一次请求之间没有任何状态保持，我们无法根据请求的任何方面(IP地址，用户代理等)来识别来自同一人的连续请求。上图很明显的展示了Django的session与cookie的实现原理。服务器会生成两份相同的cookie字符串，一份保存在本地，一份发向请求的浏览器。浏览器将收到的cookie字符串保存下来，当下次再发请求时，会将信息与这段cookie一同发送到服务器，服务器得到这段cookie会与本地保存的那份判断是否相同，如果相同就表示用户已经登录成功，保存用户登录成功的状态。Django的session保存在数据库中的数据相当于一个大字典，key为cookie的字符串，value仍是一个字典，字典的key和value为用户设置的相关信息。这样就可以方便的存取session里面的信息。

 

**Cookies**

cookies 是浏览器为 Web 服务器存储的一小段信息。 每次浏览器从某个服务器请求页面时，它向服务器回送之前收到的cookies。它保存在浏览器下的某个文件夹下。

浏览器下的cookie：

![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160316153159303-1103847788.jpg)

 

 

**Session**　　

　　Django的Session机制会向请求的浏览器发送cookie字符串。同时也会保存到本地一份，用来验证浏览器登录是否为同一用户。它存在于服务器，Django默认会把session存入到数据库中。

　　Session依赖于Cookie，如果浏览器不能保存cooki那么session就失效了。因为它需要浏览器的cooki值去session里做对比。**session就是用来在服务器端保存用户的会话状态。**

 

 

### 操作session

**根据网友lvusyy的友情提示**，在操作session之前，你需要同步一下Django的数据库。我用的是Django自带的sqlite3.所以需要执行同步的命令：

![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160317171206318-2116964962.jpg)

 

![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160317171222209-1148155392.jpg)

还有一点，在django处理请求的过程中，需要经过中间件的过滤，涉及到**跨站请求伪造**时，django会把请求阻止过滤掉，所以我们要在setting.py中**禁用**跨站请求伪造的中间件，如果不禁用，记得好像会报一个403的错误黄页。关于跨站请求伪造，之后的章节我会详细说明其功能用处：

![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160317171827521-1785606039.jpg)

 

**Django中操作session：**

　　**获取session：request.session[key]   \**request.session.get(key)\****

　　**设置session：reqeust.session[key] = value**

　　**删除session：del request[key]**

 

　　request.session是每一个客户端相当于在上图中对应的value

 

一段简单的Django中实现session的代码，判断用户是否已经成功登录：

```python
def login(request):
    if request.method =='POST':
        username = request.POST.get('username')
        pwd = request.POST.get('pwd')
        if username =='lisi' and pwd == '12345':
            request.session['IS_LOGIN'] = True       设置session
            return redirect('/app01/home/')

    return render(request,'login.html')

def home(request):
    is_login = request.session.get('IS_LOGIN',False)   获取session里的值
    if is_login:
        return HttpResponse('order')
    else:
        return redirect('/app01/login/')
```

 

### 过期时间

cookie可以有过期时间，这样浏览器就知道什么时候可以删除cookie了。 如果cookie没有设置过期时间，当用户关闭浏览器的时候，cookie就自动过期了。 你可以改变 **SESSION_EXPIRE_AT_BROWSER_CLOSE** 的设置来控制session框架的这一行为。缺省情况下， SESSION_EXPIRE_AT_BROWSER_CLOSE 设置为 False ，这样，会话cookie可以在用户浏览器中保持有效达 **SESSION_COOKIE_AGE** 秒（缺省设置是两周，即1,209,600 秒）。 如果你不想用户每次打开浏览器都必须重新登陆的话，用这个参数来帮你。如果 SESSION_EXPIRE_AT_BROWSER_CLOSE 设置为 True ，当浏览器关闭时，Django会使cookie失效。

 

**SESSION_COOKIE_AGE：设置cookie在浏览器中存活的时间**

在settings.py中添加：

**![img](https://images2015.cnblogs.com/blog/773921/201603/773921-20160316161532881-2110905546.jpg)**

 

 

### 例子

**结合前端实现的cookie与session会话机制：**

```python
def login(request):
    if request.method == "POST":
        username = request.POST.get('username')
        pwd = request.POST.get('pwd')
        if username == 'alex' and pwd == '123':
            request.session['IS_LOGIN'] = True
            request.session['USRNAME'] = 'alex'
            return redirect('/app01/home/')
        elif username == 'eirc' and pwd == '123':
            request.session['IS_LOGIN'] = True
            request.session['USRNAME'] = 'eirc'
            return redirect('/app01/home/')

    return render(request, 'login.html')

def home(request):
    is_login = request.session.get('IS_LOGIN', False)
    if is_login:
        username = request.session.get('USRNAME', False)
        return render(request, 'home.html', {'username': username})
    else:
        return redirect("/app01/login/")
```