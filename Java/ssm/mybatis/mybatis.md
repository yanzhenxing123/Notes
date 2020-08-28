**maven配置**

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.2</version>
</dependency>
```



## 搭建环境

```sql
INSERT INTO `user` (`id`, `name`, `pwd`) VALUES
(1, '闫振兴', '123456'),
(2, 'tom', '123456'),
(3, '小明', '123456')
```



+ 创建一个maven项目
+ 删除src包



**配置文件**

**mybatis-config**



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
<!--            事务管理-->
            <transactionManager type="JDBC" />
            <!-- 配置数据库连接信息 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT"/>
                <property name="username" value="root" />
                <property name="password" value="209243" />
            </dataSource>
        </environment>
    </environments>

</configuration>
```



### 编写mybatis的工具类

```java
package tom.yanzx.utils;

import jdk.internal.loader.Resource;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

/**
 * @Author: yanzx
 * @Date: 2020/8/6 23:47
 * @Description:
 */
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        String resource = "mybatis-config.xml";
        try {
            // 使用第一步：获取sqlSessionFactory对象
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

//    从 SqlSessionFactory 中获取 SqlSession
    // sqlSession完全包含了面向数据库执行的sql命令
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

### 编写代码

+ 实体类
+ Dao接口
+ 接口实现类（由原来的Impl转换为mapper配置文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<!--namespace 绑定一个对应的Dao/Mapper接口-->
<mapper namespace="tom.yanzx.dao.UserDao">
<!--    相当于实现了一个接口-->
    <select id="getUserList" resultType="tom.yanzx.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```



### 测试

**注意点**

`org.apache.ibatis.binding.BindingException: Type interface tom.yanzx.dao.UserDao is not known to the MapperRegistry.`

**MapperRegistry**



2. 资源过滤问题

就是找不到resource中的.xml文件



maven约定大于配置

![image-20200807161635953](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/172949-510112.png)

### 步骤：

![image-20200807182200357](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/172955-378424.png)





## CRUD

### mapper.xml中的namespace

### select

+ id
+ resultType
+ paramtertype: 参数类型

**接口**

```java
package tom.yanzx.dao;

import tom.yanzx.pojo.User;

import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/7 15:45
 * @Description: 接口
 */
public interface UserMapper {
    List<User> getUserList();

    // id查询用户
    User getUserById(int id);

    // 插入
    int addUser(User user);


    // 修改用户
    int deleteUser(int id);
}
```







**xml文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<mapper namespace="tom.yanzx.dao.UserMapper">
    <select id="getUserList" resultType="tom.yanzx.pojo.User">
        select * from mybatis.user
    </select>

    <select id="getUserById" parameterType="int" resultType="tom.yanzx.pojo.User">
        select * from mybatis.user where id = #{id}
    </select>

<!--    insert 没有return-->
    <insert id="addUser" parameterType="tom.yanzx.pojo.User">
        insert into mybatis.user (id, name , pwd) values(#{id}, #{name}, #{pwd});
    </insert>


    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user where id = #{id};
    </delete>


</mapper>
```





**test类**

```java
package tom.yanzx.dao;

import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import tom.yanzx.pojo.User;
import tom.yanzx.utils.MybatisUtils;

import javax.sound.midi.Soundbank;
import java.util.List;

/**
 * @Author: yanzx
 * @Date: 2020/8/7 15:57
 * @Description:
 */
public class UserDaoTest {
    SqlSession sqlSession;
    @Test
    public void test(){
        try {
            // 1.获取SqlSession对象
            sqlSession = MybatisUtils.getSqlSession();

            // 执行sql
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            List<User> userList = userMapper.getUserList();

            for (User user: userList){
                System.out.println(user);

            }

        }catch (Exception e){
            e.printStackTrace();

        }finally {

            // 关闭SqlSession
            sqlSession.close();

        }


    }

    @Test
    public void getUserById(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        User user = mapper.getUserById(1);
        System.out.println(user);
    }

    @Test
    public void addUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int res = mapper.addUser(new User(4, "hhhh", "99999"));
        if (res > 0){
            System.out.println("succuss");

        }
        // 提交事务
        sqlSession.commit();
        sqlSession.close();
    }

    @Test
    public void deleteUser(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);

        int temp = mapper.deleteUser(3);

        System.out.println(temp);


//        sqlSession.commit();


    }

}
```





## 万能map

用map进行传参



## 模糊查询

拼接字符串 `%李%`

![image-20200807193948588](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/173023-135574.png)



