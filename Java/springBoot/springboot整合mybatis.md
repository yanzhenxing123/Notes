# 整合mybatis

**步骤：**

1. 导入包
2. 配置文件
3. mybatis配置
4. 编写sql
5. service层调用dao层
6. controller层调用service层





`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>top.yanzx</groupId>
    <artifactId>springboot-05-mybatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-05-mybatis</name>
    <description>Demo project for Spring Boot</description>


    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--    lombok-->

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


</project>
```

配置文件`application.properties`

```properties
spring.datasource.username=root
spring.datasource.password=209243
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver


# 整合mybatis
mybatis.type-aliases-package=top.yanzx.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```



pojo中`User.java`

```java
package top.yanzx.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @Author: yanzx
 * @Date: 2020/8/21 20:29
 * @Description:
 */

@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
```



mapper中`UserMapper.java`

```java
package top.yanzx.mapper;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;
import top.yanzx.pojo.User;

import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/21 20:30
 * @Description:
 */
@Mapper // 代表这一个是mybatis的mapper类
@Repository // 被spring整合
public interface UserMapper {
    int age = 18;  // public static final

    List<User> queryUserList(); // public abstratct

    User queryUserById();

    int addUser(User user);

    int updateUser(User user);

    int deleteUser(User user);


}
```



resources下面的mybatis中的mapper下的UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="top.yanzx.mapper.UserMapper">
    <select id="queryUserList" resultType="User">
        select * from user
    </select>

    <select id="queryUserById" resultType="User">
        select * from user where id = #{id}
    </select>

    <insert id="addUser" parameterType="User">
        insert into user (id, name, pwd) value (#{id}, #{name}, #{pwd})
    </insert>

    <update id="updateUser" parameterType="User">
        update user set name=#{name}, pwd=#{pwd} where id = #{id}
    </update>

    <delete id="deleteUser" parameterType="int" >
        delete from user where id = #{id}
    </delete>
</mapper>
```

controller中的`UserController.java`

```java
package top.yanzx.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import top.yanzx.mapper.UserMapper;
import top.yanzx.pojo.User;

import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/22 14:52
 * @Description:
 */
@RestController
public class UserController {

    // 注入
    @Autowired
    private UserMapper userMapper;

    @GetMapping("/queryUserList")
    public List<User> queryUserList(){
        List<User> userList = userMapper.queryUserList();
        for (User user : userList) {
            System.out.println(user);
        }
        return userList;
    }

    @GetMapping("/addUser")
    public String addUser(){

        int i = userMapper.addUser(new User(6, "yanzxxxx", "xxxxxx"));
        System.out.println(i);
        return "ok";
    }



}
```

