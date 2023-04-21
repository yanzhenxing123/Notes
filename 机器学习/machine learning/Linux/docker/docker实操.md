## 运行nginx

+ 搜索镜像 docker search
+ 下载 pull
+ 运行测试

![image-20200826163455220](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/26/163455-883952.png)

**端口暴露的概念**

![image-20200826163535087](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/26/163535-832168.png)

访问：http://39.98.126.80:3344即可访问

## tomcat







## es +kibana

+ `docker stats 容器id`



通过宿主机 进行操作

![image-20200826192502916](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/26/192503-360050.png)

## 可视化

图形界面管理工具

`docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer`

```bash
root@root:/# docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
d1e017099d17: Pull complete 
717377b83d5c: Pull complete 
Digest: sha256:f8c2b0a9ca640edf508a8a0830cf1963a1e0d2fd9936a64104b3f658e120b868
Status: Downloaded newer image for portainer/portainer:latest
4e75638680243493210261782db122537ced461d87332728043ddaf729d86de3
root@root:/# docker ps
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS              PORTS                    NAMES
4e7563868024        portainer/portainer   "/portainer"        23 seconds ago      Up 21 seconds       0.0.0.0:8088->9000/tcp   gallant_williamson
6a19bc412ad6        tomcat                "catalina.sh run"   45 minutes ago      Up 45 minutes       0.0.0.0:3355->8080/tcp   tomcat01
root@root:/# 

```

