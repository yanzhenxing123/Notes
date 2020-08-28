# 基础

## springMVC步骤

![](https:////upload-images.jianshu.io/upload_images/7896890-65ef874ad7da59a2.png?imageMogr2/auto-orient/strip|imageView2/2/w/784/format/webp)

![image-20200813183640900](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200058-629049.png)





![image-20200813193918096](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200104-275194.png)

![image-20200813193945994](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200107-637363.png)

**总的来说就是DispatcherServlet就是一个调度器**





## restful 风格

就是我写的博客的风格 将所有的数据都是网站 不加问号

简洁高效安全



`http://localhost:8080/user/edit/123`

这就是restful风格



# 前端提交

### @RequestParam

![image-20200814153759474](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200110-939394.png)

指定只收`username`其他的都会报错



### 前端接受的是一个对象

`User对象`

```java
package top.yanzx.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @Author: yanzx
 * @Date: 2020/8/14 14:39
 * @Description:
 */


@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private int age;

}
```

![image-20200814154100303](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200121-246640.png)

用这个直接接受就可以了，不匹配就是null



### ModelMap

继承了`ModelLinkedMap`

![image-20200814154426571](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200128-757167.png)

### 总结和区别

![image-20200814154449069](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200131-370528.png)

## 乱码

**过滤器解决乱码**

***过滤器***

```java
package top.yanzx.filter;

import javax.servlet.*;
import java.io.IOException;

/**
 * @Author: yanzx
 * @Date: 2020/8/14 15:58
 * @Description:
 */
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");

        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {

    }
}
```

**注册**

在``web.xml`进行注册：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">



    <!--    配置dispatcherServlet SpringMVC的核心：请求分发器，前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!--        绑定SpringMvc的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>

        <!--        启动级别-->
        <!--        服务器一经启动 就启动了-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--    接受所有请求-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--        所有的请求 /*和/的区别 ： /*会匹配.jsp页面 会无限的循环-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>


<!--    过滤器 处理的是request 和 response-->

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>top.yanzx.filter.EncodingFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>encoding</filter-name>
<!--        过滤所有的请求-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

* 但是现在还是没有解决我们所遇到的问题 乱码 因为 我们自己编写的这一个filter只能解决get请求，对于post请求的话，需要用人家写好的



```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">



    <!--    配置dispatcherServlet SpringMVC的核心：请求分发器，前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!--        绑定SpringMvc的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>

        <!--        启动级别-->
        <!--        服务器一经启动 就启动了-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--    接受所有请求-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--        所有的请求 /*和/的区别 ： /*会匹配.jsp页面 会无限的循环-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>


<!--    过滤器 处理的是request 和 response-->

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>encoding</filter-name>
<!--        过滤所有的请求 注意是/*-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```





## json

```java
package top.yanzx.controller;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import top.yanzx.pojo.User;

/**
 * @Author: yanzx
 * @Date: 2020/8/14 19:42
 * @Description:
 */

@Controller
// @RestController // 不走视图解析 只用json的时候可以用这一个
public class UserController {


    @RequestMapping("/j1")
    @ResponseBody // 不走视图解析器 直接返回一个字符串
    public String json1() throws JsonProcessingException {

        ObjectMapper mapper = new ObjectMapper();

        // create an obj
        String name = "闫振兴", sex = "male";
        int age = 19;
        User user = new User(name, age, sex);

        // new Data() // 这是一个时间戳  
       
        String str = mapper.writeValueAsString(user);

        return str;
    }
    
    
    // 处理时间戳
    @RequestMapping("/j2")
    @ResponseBody // 不走视图解析器 直接返回一个字符串
    public String json1() throws JsonProcessingException {
		Date date = new Date();
        
    }
}
```

**返回json的步骤**：

+ 导包
+ 解决乱码问题
+ RestController 或者 @ResponseBody
+ 注意时间戳  utils





### fastjson

