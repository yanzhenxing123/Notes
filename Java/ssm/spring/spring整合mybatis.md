**步骤：**

因为是增删改查，所以以查为例，进行整合。

数据库 为`mysql.mybatis`

1. 连接数据库，创建一个`User`类，进行接受数据库中的字段

```java
package top.yanzx.pojo;

/**
 * @Author: yanzx
 * @Date: 2020/8/11 14:09
 * @Description:
 */
public class User {
    private int id;
    private String name;
    private String pwd;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```

2. 如果是使用的`mybatis`的话，那么就需要用`SqlSessionFactoryBuilder`获取`SqlSessionFactory`， 再由`SqlSessionFactory`获取`SqlSession`，但是整合之后就比较简单，只需有一个`UserMapper`的实现类`UserMapperImpl`即可

   `UserMapperImpl`， 使用的是`SqlSessionTemplate`获取的`SqlSession`

```java
package top.yanzx.mapper;

import org.mybatis.spring.SqlSessionTemplate;
import top.yanzx.pojo.User;

import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/11 14:55
 * @Description:
 */
public class UserMapperImpl implements UserMapper{

    // 以前使用的sqlSession进行操作,现在都是使用SqlSessionTemplate
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession){
        this.sqlSession = sqlSession;
    }


    @Override
    public List<User> selectUser() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.selectUser();

    }
}
```

``UserMapper.xml``也相当于是一个实现类，但是真正的调用是上面的`UserMapperImpl.java`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<!--namespace 绑定一个对应的Dao/Mapper接口-->
<mapper namespace="top.yanzx.mapper.UserMapper">
    <!--    相当于实现了一个接口-->
    <select id="selectUser" resultType="top.yanzx.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```

3. 重点还是在配置这里

`mybatis.xml`已经快没有用了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<!--别名管理-->
    <typeAliases>
        <package name="top.yanzx.pojo"/>
    </typeAliases>
<!--    设置-->

<!--    <environments default="development">-->
<!--        <environment id="development">-->
<!--            &lt;!&ndash;            事务管理&ndash;&gt;-->
<!--            <transactionManager type="JDBC" />-->
<!--            &lt;!&ndash; 配置数据库连接信息 &ndash;&gt;-->
<!--            <dataSource type="POOLED">-->
<!--                <property name="driver" value="com.mysql.cj.jdbc.Driver" />-->
<!--                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>-->
<!--                <property name="username" value="root" />-->
<!--                <property name="password" value="209243" />-->
<!--            </dataSource>-->
<!--        </environment>-->
<!--    </environments>-->

<!--    <mappers>-->
<!--&lt;!&ndash;        <mapper class="top.yanzx.mapper.UserMapper"/>&ndash;&gt;-->

<!--&lt;!&ndash;        <mapper resource="top/yanzx/mapper/UserMapper.xml"/>&ndash;&gt;-->
<!--    </mappers>-->

</configuration>
```



`spring-dao.xml`这个就是用spring整合`mybatis`的核心配置文件，将东西用依赖注入进行实现。

这个文件就相当于是写死的，直接拿过来用就可以了



```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    DataSource 数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
        <property name="username" value="root" />
        <property name="password" value="209243" />
    </bean>

<!--    SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
<!--        绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>

        <property name="mapperLocations" value="classpath:top/yanzx/mapper/*.xml"/>

    </bean>

    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
<!--        没有set方法 所以构造方法初始化-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>


</beans>
```

`applicationContext.xml`是一个总的配置文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">
	
    <!--直接import进来即可-->
    <import resource="spring-dao.xml"/>

    
    <bean id="userMapper" class="top.yanzx.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>


</beans>
```

4. 进行测试

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.core.io.Resource;
import top.yanzx.mapper.UserMapper;
import top.yanzx.pojo.User;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/11 14:15
 * @Description:
 */
public class MyTest {
    @Test
    public void test() throws IOException {
        /*这个是mybatis没有整合时候的查找*/
//        String resources = "mybatis-config.xml";
//        InputStream in = Resources.getResourceAsStream(resources);
//        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(in);
//
//        SqlSession sqlSession = sessionFactory.openSession(true);
//
//        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//
//        List<User> userList = mapper.selectUser();
//
//        for (User user : userList) {
//            System.out.println(user);
//    }
        
       // spring的方式
        //public class MyTest {
//    public static void main(String[] args) {
//
//        // 获取spring的上下文对象，拿到容器
////        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
//
//        ApplicationContext context = new ClassPathXmlApplicationContext();
//        Hello hello = (Hello) context.getBean("hello");
//        System.out.println(hello.toString());
//
//    }
//
//}

        
// 使用spring的方法进行mybatis
        // 获取spring的上下文对象，拿到容器
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        // 拿到bean
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        for (User user : userMapper.selectUser()) {
            System.out.println(user);
        }
    }
}
```