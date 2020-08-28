# spring Boot配置

`application.yaml`

![image-20200819195317098](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/195317-838586.png)

但是对 空格的要求十分严格



## 注入

可以使用`@value`注解

![](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/19/200101-581488.png)

# Spring Boot 使用YAML配置

　　YAML是JSON的一个超集，可以非常方便地将外部配置以层次结构形式存储起来。当项目的类路径中有SnakeYAML库（spring-boot-starter中已经被包含）时，SpringApplication类将自动支持YAML作为properties的替代。
　　如果将项目中的application.properties文件修改为YAML文件（尾缀为.yml或yaml）的形式，则其配置信息如文件3-8所示。

　　文件3-8　application.yml

```yaml
server:
  port: 8081
#DB Configuration
spring:
  datasource:
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3307/microservice
    username: root
    password: 123456
#logging
logging:
  level:
    com.xc.springboot: debug
```

　　从上述配置文件中可以看出，yml文件是一个树状结构的配置，它与properties文件相比，有很大的不同，在编写时需要注意以下几点。
　　（1）在properties文件中是以“.”进行分割的，在yml中是用“:”进行分割的。
　　（2）yml的数据格式和json的格式很像，都是K-V格式，并且通过“:”进行赋值。
　　（3）每个k的冒号后面一定都要加一个空格，例如driver-class-name后面的“:”之后，需要有一个空格，否则文件会报错。



## 配置的优先级

![image-20200820134723360](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/134723-501112.png)

## springBoot多环境配置的激活

### properties

![image-20200820134914034](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/134916-282299.png)

### yaml

一个文件可以进行多个配置

![image-20200820135028965](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/135029-191735.png)

## 配置文件可以写什么





## 静态资源

### webjars

以`webjars`的方式引进jquery，

`resouces`> `static`>`public`资源顺序



### 总结

![image-20200820144346188](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/144346-164619.png)

**自定义：**

`spring.mvc.static-path-pattern=hello/,classpath:/yanzx/`



## 资源跳转

`templates`目录下的所有页面，只能通过controller进行跳转，需要模板引擎的支持。





