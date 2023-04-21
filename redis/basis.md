# Redis Basis

## 五大基本数据类型

### string

+ 设置对象

![image-20200830174727130](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/174727-293838.png)

### list

### set

![image-20200830173122332](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/173123-611992.png)

+ scard myset # 获取集合中的成员数量
+ srem myser hello # 移除
+ srandmember myset  # 随机抽选出一个元素
+ srandmember myset 2 # 随机抽选出指定的元素



![image-20200830173513637](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/173514-714526.png)

+ 将一个指定value移植到另一个集合中

![image-20200830173634426](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/173635-669369.png)

+ 微博 B站 共同关注（并集）

![image-20200830173835809](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/173836-102138.png)

微博，A用户将所有关注的人放在一个set集合中！将它的粉丝也放在一个集合中

共同关注，共同爱好，二度好友

### hash（哈希）

Map集合，key-map

+ 创建 删除

![image-20200830174254223](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/174255-447472.png)

+ 查找

![image-20200830174409767](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/174410-30681.png)

![image-20200830174439731](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/174440-626756.png)

![image-20200830174546334](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/174546-308571.png)

**哈希应用**：

1. 用户信息的保存

### zset（有序集合）

在set的基础上增加了一个值 还是字符串作为键 数字作为值

eg：`set k1 v1 zset k1 score1 v1`

+ 添加值

![image-20200830175041347](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/175041-124547.png)

+ 排序

**升序**

![image-20200830175359261](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/175359-958601.png)

**降序**

zrevrange
![image-20200830175705734](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/175707-422017.png)

+ 移除

![image-20200830175536270](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/175536-206007.png)

+ 获取区间中的数量



![image-20200830175755845](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/175756-108452.png)

## 三种特殊数据类型

### geospatial（地理位置）

朋友的定位，附近的人，打车距离计算

> geoadd

![image-20200830180701503](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/180701-487239.png)

> geopos

![image-20200830180746088](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/180746-924230.png)

> geodist

![image-20200830180858054](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/180858-330571.png)

> georedius 以给定的经纬度为中心，找出某一半径的元素

![image-20200830181054176](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/181054-639076.png)

![image-20200830181255947](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/181256-577734.png)

![image-20200830181433821](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/181434-506208.png)

![image-20200830181515852](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/181516-870800.png)

![image-20200830181558187](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/30/181601-490444.png)

### Hyperlog

> 什么是基数?

![image-20200901154452960](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/154453-781140.png)

> 简介：基数统计的算法



![image-20200901154834244](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/154835-39246.png)

> 测试

![image-20200901155541566](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/155545-920777.png)

### Bitmap

> 位储存

统计用户信息

![image-20200901162526611](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/162528-160355.png)

![image-20200901162544183](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/162544-568921.png)

## 事务

Mysql：ACID

![image-20200901163232093](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163232-257937.png)

Redis事务本质：一组命令的集合

![image-20200901163041114](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163042-998904.png)

> 执行事务

```shell
--队列 set set set 执行--

127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) OK
4) "v2"
127.0.0.1:6379>                                                                                                                        

```

![image-20200901163414063](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163414-67184.png)

> 放弃事务

![image-20200901163531589](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163532-979291.png)

> 编译型异常

![image-20200901163622496](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163637-996604.png)

![image-20200901163745525](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163746-615611.png)



> 运行时异常

![image-20200901163636950](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163648-77062.png)

![image-20200901163833034](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/163833-101997.png)

> 监控！

**悲观锁：**

+ 很悲观，认为什么时候都会出问题，无论做什么都会加锁



**乐观锁：**

+ 很乐观，认为什么时候都不会出问题，所以不会上锁！更新数据的时候去判断一下，在此期间是否有人修改过这个数据
+ 获取version
+ 更新的时候 比较version



> Redis侧监视测试

正常执行成功！

```bash
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
                           
```

![image-20200901165205677](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/165206-147267.png)

## Jedis

![image-20200901165321992](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/165323-675698.png)

> 测试

1. 导入对应的依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>redis-01-Jredis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.3.0</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.73</version>
        </dependency>


    </dependencies>


</project>
```

2. 连接测试
   + 连接数据库
   + 操作命令
   + 断开连接

```java
package top.yanzx;

import redis.clients.jedis.Jedis;

/**
 * @Author: yanzx
 * @Date: 2020/9/1 17:02
 * @Description:
 */
public class TestPing {
    public static void main(String[] args) {
        // 1. new Jedis对象
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        String ping = jedis.ping();

        System.out.println(ping);  // PONG

    }
}
```

## 常用的API

### 基础

![image-20200901170611563](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170612-78667.png)

### string

![image-20200901170642425](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170643-584342.png)

### List

![image-20200901170657474](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170658-103710.png)

### Set

![image-20200901170735354](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170739-966528.png)

![image-20200901170752696](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170849-886203.png)

### Hash

![image-20200901170858461](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/170859-455978.png)



### 事务

```java
package top.yanzx;

import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

/**
 * @Author: yanzx
 * @Date: 2020/9/1 17:10
 * @Description:
 */
public class TestTX {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("127.0.0.1", 6379);

        // 创建一个Json对象
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello", "world");
        jsonObject.put("name", "yanzx");
        String jsonString = jsonObject.toJSONString();

        // 开启事务

        Transaction multi = jedis.multi();


        try {
            multi.set("user1", jsonString);
            multi.set("user2", jsonString);
            multi.exec();
        } catch (Exception e) {
            multi.discard();
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            jedis.close();
        }

    }
}
```

## SpringBoot整合

![image-20200901180306597](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/180409-180380.png)

> 源码分析

![image-20200901180423093](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/180424-588485.png)

> 整合测试

1. 导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

2. 配置连接

```properties
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

3. 测试

![image-20200901181052939](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/181054-428864.png)

<img src="https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202009/01/181117-717263.png" alt="image-20200901181116691" style="zoom:200%;" />

```java
package top.yanzx;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class Redis02SpringbootApplicationTests {

	@Autowired
	private RedisTemplate redisTemplate;

	@Test
	void contextLoads() {
		// 操作字符串

		redisTemplate.opsForValue().set("mykey", "yanzx");
		System.out.println(redisTemplate.opsForValue().get("mykey"));
	}

}
```

### 自定义序列化

*自定义序列化代码(固定的)*

```java
package top.yanzx.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;

/**
 * @Author: yanzx
 * @Date: 2020/9/1 18:15
 * @Description:
 */
@Configuration
public class RedisConfig {
    // 编写我们自己的配置类
    @Bean
    public RedisTemplate<String, Object> redisTemplat(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory);

        // 使用Jackson2JsonRedisSerialize 替换默认序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        // 设置value的序列化规则和 key的序列化规则
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();

        return redisTemplate;
    }


}

```

*测试类*

```java
package top.yanzx;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Redis02SpringbootApplication {

   public static void main(String[] args) {
      SpringApplication.run(Redis02SpringbootApplication.class, args);
   }

}
```



