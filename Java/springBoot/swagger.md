# swagger

## 前后端分离

**前后端分离的时代**：

- 前端：前端控制层、视图层
  + 伪造后端数据，json，已经存在了
- 后端：后端控制层、服务层 、数据访问层
- 前后端如何交互？===> API
- 前后端相对独立 松耦合
- 前后端可以部署在不同的服务器上面

**产生的问题**：

+ 前后端集成联调，前端人员和后端人员无法做到“及时协商，尽早解决”， 最总导致问题集中爆发

**解决方案**：

+ 前后端分离：
  + postman
  + 后端提供接口，需要实时更新最新的消息及改动

## swagger

+ **世界上最流行的Api框架**

+ RestFul Api文档在线自动生成工具
+ 直接运行 可以在线测试



## 在项目中使用swagger需要springfox

+ swagger2
+ ui

## springboot集成Swagger

1. 新建一个springboot项目

2. 导入相关依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
      <dependency>
          <groupId>io.springfox</groupId>
          <artifactId>springfox-swagger2</artifactId>
          <version>2.9.2</version>
      </dependency>
   
   
   <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
   <dependency>
       <groupId>io.springfox</groupId>
       <artifactId>springfox-swagger-ui</artifactId>
       <version>2.9.2</version>
   </dependency>
   
   
   ```

3. 编写hello工程

   + 建一个controller包

   + 创建一个`HelloController`类：

     ```java
     package top.yanzx.swagger.controller;
     
     import org.springframework.web.bind.annotation.RequestMapping;
     import org.springframework.web.bind.annotation.RestController;
     
     /**
      * @Author: yanzx
      * @Date: 2020/8/24 13:21
      * @Description:
      */
     
     @RestController
     public class HelloController  {
         @RequestMapping(value = "/hello")
         public String hello(){
             return "hello";
         }
     }
     ```

4. 配置swagger==>Config

   注解：`@Configuration`就相当于`@Component`

   将一个类作为配置类 装到IOC容器中就行了

   ```java
   package top.yanzx.swagger.config;
   
   import org.springframework.context.annotation.Configuration;
   import springfox.documentation.swagger2.annotations.EnableSwagger2;
   
   /**
    * @Author: yanzx
    * @Date: 2020/8/24 13:26
    * @Description:
    */
   @Configuration
   @EnableSwagger2 // 开启swagger2
   public class SwaggerConfig {
   
   }
   ```

5. 测试运行：访问`http://localhost:8080/swagger-ui.html`

   ![image-20200824133322320](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/24/133324-450583.png)

## 配置swagger

**覆盖掉以前的配置信息**

```java
package top.yanzx.swagger.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

/**
 * @Author: yanzx
 * @Date: 2020/8/24 13:26
 * @Description:
 */
@Configuration
@EnableSwagger2 // 开启swagger2
public class SwaggerConfig {

    // 配置了Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }


    // 配置Swagger信息=apiInfo
    private ApiInfo apiInfo(){

        // 作者信息
        Contact contract = new Contact("闫振兴", "http://www.yanzx.top", "209243446@qq.com");

        return new ApiInfo(
                "闫振兴的swagger API文档",
                "这是描述",
                "1.0",
                "http://www.yanzx.top",
                contract,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList()
        );
    }
}


```

## Swagger配置扫描接口

```java
@Bean
public Docket docket(){
    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
        	.enable()true
            .select()
        	.enable(false)// 默认为true // RequestHandlerSelectors要扫描接口的方式
            // basePackage指定要扫描的包
            // any():扫描全部
            // none 都不扫描
            // 
            .apis(RequestHandlerSelectors.basePackage("top.yanzx.swagger.controller"))
            .build();
}
```

**问题：Swagger只在生产环境中使用**

+ 生产环境`application-dev.properties` 

+ 线上环境 `application-pro.properties`

+ 总的配置`application.properties`

  ```properties
  spring.profiles.active=pro
  ```

+ 通过代码判读现在是哪个环境

  ```java
  package top.yanzx.swagger.config;
  
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.Profile;
  import org.springframework.core.env.Environment;
  import org.springframework.core.env.Profiles;
  import springfox.documentation.builders.RequestHandlerSelectors;
  import springfox.documentation.service.ApiInfo;
  import springfox.documentation.service.Contact;
  import springfox.documentation.spi.DocumentationType;
  import springfox.documentation.spring.web.plugins.Docket;
  import springfox.documentation.swagger2.annotations.EnableSwagger2;
  
  import java.util.ArrayList;
  
  /**
   * @Author: yanzx
   * @Date: 2020/8/24 13:26
   * @Description:
   */
  @Configuration
  @EnableSwagger2 // 开启swagger2
  public class SwaggerConfig {
  
      // 配置了Swagger的Docket的bean实例
      @Bean
      public Docket docket(Environment environment){
  
          // 环境的判断
          Profiles profiles = Profiles.of("dev", "test");
          boolean flag = environment.acceptsProfiles(profiles);
          System.out.println(flag);
  
  
          return new Docket(DocumentationType.SWAGGER_2)
                  .apiInfo(apiInfo())
                  .enable(flag)           // 是否开启
                  .select()
                  // RequestHandlerSelectors要扫描接口的方式
                  // basePackage指定要扫描的包
                  // any():扫描全部
                  // none 都不扫描
  
                  .apis(RequestHandlerSelectors.basePackage("top.yanzx.swagger.controller"))
                  .build();
      }
  
  
      // 配置Swagger信息=apiInfo
      private ApiInfo apiInfo(){
  
          // 作者信息
          Contact contract = new Contact("闫振兴", "http://www.yanzx.top", "209243446@qq.com");
  
          return new ApiInfo(
                  "闫振兴的swagger API文档",
                  "这是描述",
                  "1.0",
                  "http://www.yanzx.top",
                  contract,
                  "Apache 2.0",
                  "http://www.apache.org/licenses/LICENSE-2.0",
                  new ArrayList()
          );
      }
  }
  ```



## 配置API文档的分组

`.groupName()`



## 总结

【注意点】：项目发布时 关闭swagger