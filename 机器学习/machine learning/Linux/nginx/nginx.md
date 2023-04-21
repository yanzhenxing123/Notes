# 开发笔记----nginx

## 反向代理

代理模式：代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。通俗的来讲代理模式就是我们生活中常见的中介，spring中十分常见。

正向代理：eg：访问外网可以用vpn服务器，然后通过服务器进行访问，再把数据传回来。所以说，在进行正向代理的时候，用户需要进行操作的，比如要连接服务器。

反向代理：反向代理是代理的服务器，客户端也就是无感的。只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器IP地址。

nginx的server配置：

    server {
            listen       80;
            server_name  www.123.com;
    
            location / {
                proxy_pass http://127.0.0.1:8080;
                index  index.html index.htm index.jsp;
            }
        }
也就是监听server_name为www.123.com 然后代理的是 本地的8080端口。

也就是启动了一个tomcat然后，就不需要开启自己的公网端口8080，访问 www.123.com, 就可以了。

## 负载均衡

简单说一下，深的也不会。

Nginx可以配置代理多台服务器，当一台服务器宕机之后，仍能保持系统可用。

> 负载均衡的几种方式  不全

+ 轮询（默认） 

每个请求**按时间顺序逐一分配**到不同的后端服务器，如果后端服务器down掉，能自动剔除。

```
upstream backserver {
    server 192.168.0.14;
    server 192.168.0.15;
}
```

+ weight

也就是给每台服务器一个权重，因为后台 服务器 的性能可能不均匀，好的服务器就权重大一点。

```
upstream backserver {
    server 192.168.0.14 weight=3;
    server 192.168.0.15 weight=7;
}
```

+ ip_hash指令

在负载均衡系统中，假如用户在某台服务器上登录了，那么该用户第二次请求的时候，因为我们是负载均衡系统，每次请求都会重新定位到服务器集群中的某一个，那么已经登录某一个服务器的用户再重新定位到另一个服务器，其登录信息将会丢失。

可以采用ip_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。



```
upstream backserver {
    ip_hash; # 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
    server 192.168.0.14:88;
    server 192.168.0.15:80;
}
```

hash算法：pass



开发中，使用nginx要把服务器 的防火墙关了。今天忘关了，就不能用，找了好久。

```shell
systemctl stop firewalld 
```



## docker中用nginx

在docker中，各个容器是相互隔离的， 只有在相同的网络之中才能进行通讯。容器之间可以通过定义的端口进行通讯。

`docker-compose.yml`配置如下：

```shell
version: "3"

services:
  app:
    restart: always
    build: .
    command: bash -c "python3 manage.py collectstatic --no-input && python3 manage.py migrate && gunicorn --timeout=30 --workers=4 --bind :8000 django_app.wsgi:application"
    volumes:
      - .:/code
      - static-volume:/code/collected_static
    expose:
      - "8000"
    depends_on:
      - db
    networks:
      - web_network
      - db_network
  db:
    image: mysql:5.7
    volumes:
      - "./mysql:/var/lib/mysql"
    ports:
      - "3306:3306"
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=mypassword
      - MYSQL_DATABASE=django_app
    networks:
      - db_network
  nginx:
    restart: always
    image: nginx:latest
    ports:
      - "8000:8000"
    volumes:
      - static-volume:/code/collected_static
      - ./config/nginx:/etc/nginx/conf.d
    depends_on:
      - app
    networks:
      - web_network

networks:
  web_network:
    driver: bridge
  db_network:
    driver: bridge

volumes:
  static-volume:
```

大体思路： 

项目为app 数据库容器为db 然后 nginx  app作为桥梁 

- 定义了 3 个**容器**，分别是 `app` 、 `db` 和 `nginx` 。容器之间通过定义的端口进行通讯。
- 定义了 2 个**网络**，分别是 `web_network` 和 `db_network` 。只有处在相同网络的容器才能互相通讯。不同网络之间是隔离的，即便采用同样的端口，也无法通讯。
- 定义了 1 个**数据卷**， `static-volume` 。数据卷非常适合多个容器共享使用同一数据， `app` 和 `nginx` 都用到了它。
- `expose` 和 `ports` 都可以暴露容器的端口，区别是 expose 仅暴露给其他容器，而 ports 会暴露给其他容器和宿主机。

Docker 允许用户给每个容器定义其工作的网络，只有在相同的网络之中才能进行通讯。可以看到 `nginx` 容器处于 `web_network` 网络，而 `db` 容器处于 `db_network` 网络，因此它两是无法通讯的，实际上确实也不需要通讯。而 `app` 容器同时处于 `web_network` 和 `db_network` 网络，相当于是桥梁，连通了3个容器。

