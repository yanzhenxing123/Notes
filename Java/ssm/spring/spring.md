# Spring

## IOC

**Ioc—Inversion of Control，即“控制反转”，不是什么技术，而是一种设计思想。**在Java开发中，**Ioc意味着将你设计好的对象交给容器控制，而不是传统的在你的对象内部直接控制。** 理解好Ioc的关键是要明确“谁控制谁，控制什么，为何是反转（有反转就应该有正转了），哪些方面反转了”，那我们来分析一下：

　　●**谁控制谁，控制什么：**传统Java SE程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由Ioc容器来控制对 象的创建；**谁控制谁？当然是IoC 容器控制了对象；控制什么？那就是主要控制了外部资源获取（不只是对象包括比如文件等）。**

　　●**为何是反转，哪些方面反转了：**有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象；为何是反转？**因为由容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转；哪些方面反转了？依赖对象的获取被反转了。**

传统应用程序示意图， 主动去创建相关对象然后再组合起来：

![image-20210404001715821](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/04/001716-229562.png)

当有了IoC/DI的容器后，在客户端类中不再主动去创建这些对象了，如图所示:
IoC/DI容器后程序结构示意图

![image-20210404001851408](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/04/003329-587354.png)

### IOC能做什么

　　IoC 不是一种技术，只是一种思想，一个重要的面向对象编程的法则，它能指导我们如何设计出松耦合、更优良的程序。传统应用程序都是由我们在类内部主动创建依赖对象，从而导致类与类之间高耦合，难于测试；有了IoC容器后，把创建和查找依赖对象的控制权交给了容器，由容器进行注入组合对象，所以对象与对象之间是 松散耦合，这样也方便测试，利于功能复用，更重要的是使得程序的整个体系结构变得非常灵活。

　　其实**IoC对编程带来的最大改变不是从代码上，而是从思想上，发生了“主从换位”的变化。应用程序原本是老大，要获取什么资源都是主动出击，但是在IoC/DI思想中，应用程序就变成被动的了，被动的等待IoC容器来创建并注入它所需要的资源了。**

　　**IoC很好的体现了面向对象设计法则之一—— 好莱坞法则：“别找我们，我们找你”；即由IoC容器帮对象找相应的依赖对象并注入，而不是由对象主动去找。**



### IOC和DI

　　**DI—Dependency Injection，即“依赖注入”**：**组件之间依赖关系**由容器在运行期决定，形象的说，即**由容器动态的将某个依赖关系注入到组件之中**。**依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。**通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。

　　理解DI的关键是：“谁依赖谁，为什么需要依赖，谁注入谁，注入了什么”，那我们来深入分析一下：

　　●**谁依赖于谁：**当然是**应用程序依赖于IoC容器**；

　　●**为什么需要依赖**：**应用程序需要IoC容器来提供对象需要的外部资源**；

　　●**谁注入谁：**很明显是**IoC容器注入应用程序某个对象，应用程序依赖的对象**；

　　**●注入了什么：**就是**注入某个对象所需要的外部资源（包括对象、资源、常量数据）**。

　　**IoC和DI**由什么**关系**呢？其实它们**是同一个概念的不同角度描述**，由于控制反转概念比较含糊（可能只是理解为容器控制对象这一个层面，很难让人想到谁来维护对象关系），所以2004年大师级人物Martin Fowler又给出了一个新的名字：“依赖注入”，相对IoC 而言，**依赖注入**  **明确描述了“被注入对象依赖IoC** **容器配置依赖对象”。**

IoC的一个重点是在系统运行中，动态的向某个对象提供它所需要的其他对象。这一点是通过DI（Dependency Injection，依赖注入）来实现的**。比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，有了 spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。A需要依赖 Connection才能正常运行，而这个Connection是由spring注入到A中的，依赖注入的名字就这么来的。那么DI是如何实现的呢？ Java 1.3之后一个重要特征是反射（reflection），它允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性，spring就是通过反射来实现注入的。



### hello spring

![image-20210404002544076](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/04/003156-628530.png)

`User.java`

```java
package top.yanzx.pojo;

import javax.sound.midi.Soundbank;

/**
 * @Author: yanzx
 * @Date: 2020/8/9 14:54
 * @Description:
 */
public class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("name：" + name);
    }
}

```

`beans.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">


<!--    ioc创建对象的方式-->
    <bean id="user" class="top.yanzx.pojo.User">
<!--        无参构造-->
<!--        <property name="name" value="yanzx"/>-->

<!--        有参-->
<!--        1.index赋值-->
<!--        <constructor-arg index="0" value="yanzxing" />-->
<!--        2.不建议使用（通过类型进行创建）-->
<!--        <constructor-arg type="java.lang.String" value="严重性大"/>-->

<!--        3.最好的有参构造方式-->
        <constructor-arg name="name" value="yanzx"/>

    </bean>

<!--    bean的别名name为id-->
    <alias name="user" alias="kkkUser"/>



</beans>
```





`MyTest.java`

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import top.yanzx.pojo.User;

/**
 * @Author: yanzx
 * @Date: 2020/8/9 14:56
 * @Description:
 */
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        // 之前就使用无参构造构造了
        User user = (User) context.getBean("kuser");
        user.show();

    }
}

```

#### Bean的作用域

scope

![image-20210404003150155](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/04/003540-488027.png)

> 单例模式(了解) 

博客：https://blog.csdn.net/qq_25343557/article/details/79099608



#### 自动装配

- 自动装配是使用spring满足bean依赖的一种方法
- spring会在应用上下文中为某个bean寻找其依赖的bean。

Spring的自动装配需要从两个角度来实现，或者说是两个操作：

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean；
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI；

组件扫描和自动装配组合发挥巨大威力，使得显示的配置降低到最少。

**推荐不使用自动装配xml配置 , 而使用注解 **

> 测试环境搭建

1、新建一个项目

2、新建两个实体类，Cat  Dog  都有一个叫的方法

```java
package top.yanzx.pojo;

/**
 * @Author: yanzx
 * @Date: 2021/4/4 13:49
 * @Description:
 */
public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}


package top.yanzx.pojo;

/**
 * @Author: yanzx
 * @Date: 2021/4/4 13:49
 * @Description:
 */
public class Cat {
    public void shout() {
        System.out.println("miao~");
    }
}




```

3、新建一个用户类 User

```java
package top.yanzx.pojo;

import org.springframework.beans.factory.annotation.Autowired;

/**
 * @Author: yanzx
 * @Date: 2021/4/4 13:49
 * @Description:
 */
public class User {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;

    private String name = "yanzx";

    public Cat getCat() {
        return cat;
    }

    public Dog getDog() {
        return dog;
    }

    public String getName() {
        return name;
    }

}

```

4、编写Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/tool http://www.springframework.org/schema/tool/spring-tool.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

<!--    开启属性注解支持-->
    <context:annotation-config/>
    <bean id="dog" class="top.yanzx.pojo.Dog"/>
    <bean id="cat" class="top.yanzx.pojo.Cat"/>

<!--    1. 默认-->
<!--    <bean id="user" class="top.yanzx.pojo.User">-->
<!--        <property name="cat" ref="cat"/>-->
<!--        <property name="dog" ref="dog"/>-->
<!--        <property name="name" value="yanzx"/>-->
<!--    </bean>-->

<!--    2. byName-->
<!--        <bean id="user" class="top.yanzx.pojo.User"  autowire="byName">-->
<!--            <property name="name" value="yanzx"/>-->
<!--        </bean>-->

    <!--    3. byType-->
<!--            <bean id="user" class="top.yanzx.pojo.User"  autowire="byType">-->
<!--                <property name="name" value="yanzx"/>-->
<!--            </bean>-->

    <bean id="user" class="top.yanzx.pojo.User"/>




</beans>
```

5、测试

```java
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import top.yanzx.pojo.User;

/**
 * @Author: yanzx
 * @Date: 2021/4/4 13:51
 * @Description:
 */
public class MyTest {
    @Test
    public void testMethodAutowire() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        User user = (User) context.getBean("user");
        System.out.println(user.getName());
        user.getCat().shout();
        user.getDog().shout();
    }
}

```

结果正常输出，环境OK



##### byName

**autowire byName (按名称自动装配)**

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

采用自动装配将避免这些错误，并且使配置简单化。

测试：

1、修改bean配置，增加一个属性  autowire="byName"

```xml
<bean id="user" class="top.yanzx.pojo.User" autowire="byName">
   <property name="str" value="yanzx"/>
</bean>
```

2、再次测试，结果依旧成功输出！

3、我们将 cat 的bean id修改为 catXXX

4、再次测试， 执行时报空指针java.lang.NullPointerException。因为按byName规则找不对应set方法，真正的setCat就没执行，对象就没有初始化，所以调用时就会报空指针错误。

**小结：**

当一个bean节点带有 autowire byName的属性时。

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。

2. 去spring容器中寻找是否有此字符串名称id的对象。

3. 如果有，就取出注入；如果没有，就报空指针异常。

   

##### byType

**autowire byType (按类型自动装配)**

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一的异常。

```
NoUniqueBeanDefinitionExc
ption
```

测试：

1、将user的bean配置修改一下 ： autowire="byType"

2、测试，正常输出

3、在注册一个cat 的bean对象！

```xml
<bean id="dog" class="top.yanzx.pojo.Dog"/>
<bean id="cat" class="top.yanzx.pojo.Cat"/>
<bean id="cat2" class="top.yanzx.pojo.Cat"/>

<bean id="user" class="top.yanzx.pojo.User" autowire="byType">
   <property name="str" value="yanzx"/>
</bean>
```

4、测试，报错：NoUniqueBeanDefinitionException

5、删掉cat2，将cat的bean名称改掉！测试！因为是按类型装配，所以并不会报异常，也不影响最后的结果。甚至将id属性去掉，也不影响结果。



#### 使用注解

jdk1.5开始支持注解，spring2.5开始全面支持注解。

准备工作：利用注解的方式注入属性。

1、在spring配置文件中引入context文件头

```xml
xmlns:context="http://www.springframework.org/schema/context"

http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
```

2、开启属性注解支持！

```xml
<context:annotation-config/>
```



##### @Autowired

- @Autowired是按类型自动转配的，不支持id匹配。
- 需要导入 spring-aop的包！

测试：

1、将User类中的set方法去掉，使用@Autowired注解

```java
public class User {
   @Autowired
   private Cat cat;
   @Autowired
   private Dog dog;
   private String str;

   public Cat getCat() {
       return cat;
  }
   public Dog getDog() {
       return dog;
  }
   public String getStr() {
       return str;
  }
}
```

2、此时配置文件内容

```java
<context:annotation-config/>

<bean id="dog" class="top.yanzx.pojo.Dog"/>
<bean id="cat" class="top.yanzx.pojo.Cat"/>
<bean id="user" class="top.yanzx.pojo.User"/>
```

3、测试，成功输出结果！

@Autowired(required=false)  说明：false，对象可以为null；true，对象必须存对象，不能为null。

```java
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
private Cat cat;
```

##### @Qualifier

- @Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配
- @Qualifier不能单独使用。

测试实验步骤：

1、配置文件修改内容，保证类型存在对象。且名字不为类的默认名字！

```xml
<bean id="dog1" class="top.yanzx.pojo.Dog"/>
<bean id="dog2" class="top.yanzx.pojo.Dog"/>
<bean id="cat1" class="top.yanzx.pojo.Cat"/>
<bean id="cat2" class="top.yanzx.pojo.Cat"/>
```

2、没有加Qualifier测试，直接报错

3、在属性上添加Qualifier注解

```java
@Autowired
@Qualifier(value = "cat2")
private Cat cat;
@Autowired
@Qualifier(value = "dog2")
private Dog dog;
```

测试，成功输出！

##### @Resource

- @Resource如有指定的name属性，先按该属性进行byName方式查找装配；
- 其次再进行默认的byName方式进行装配；
- 如果以上都不成功，则按byType的方式自动装配。
- 都不成功，则报异常。

实体类：

```java
public class User {
   //如果允许对象为null，设置required = false,默认为true
   @Resource(name = "cat2")
   private Cat cat;
   @Resource
   private Dog dog;
   private String str;
}
```

beans.xml

```java
<bean id="dog" class="top.yanzx.pojo.Dog"/>
<bean id="cat1" class="top.yanzx.pojo.Cat"/>
<bean id="cat2" class="top.yanzx.pojo.Cat"/>

<bean id="user" class="top.yanzx.pojo.User"/>
```

测试：结果OK

配置文件2：beans.xml ， 删掉cat2

```xml
<bean id="dog" class="top.yanzx.pojo.Dog"/>
<bean id="cat1" class="top.yanzx.pojo.Cat"/>
```

实体类上只保留注解

```java
@Resource
private Cat cat;
@Resource
private Dog dog;
```

结果：OK

结论：先进行byName查找，失败；再进行byType查找，成功。





##### @Autowired与@Resource异同：

1、@Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。

2、@Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用

3、@Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。

它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byType，@Resource先byName。



#### 使用注解开发

实际开发中使用注解。

1、配置扫描哪些包下的注解

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    <context:component-scan base-package="top.yanzx.pojo"/>

</beans>
```

2、在指定包下编写类，增加注解

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
public class User {
   public String name = "yanzx";
}
```

3、测试

```java
@Test
public void test(){
   ApplicationContext applicationContext =
       new ClassPathXmlApplicationContext("beans.xml");
   User user = (User) applicationContext.getBean("user");
   System.out.println(user.name);
}
```



##### 属性注入

使用注解注入属性

1、可以不用提供set方法，直接在直接名上添加@value("值")

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
public class User {
   @Value("yanzx")
   // 相当于配置文件中 <property name="name" value="yanzx"/>
   public String name;
}
```

2、如果提供了set方法，在set方法上添加@value("值");

```java
@Component("user")
public class User {

   public String name;

   @Value("yanzx")
   public void setName(String name) {
       this.name = name;
  }
}
```



##### 衍生注解

我们这些注解，就是替代了在配置文件当中配置步骤而已！更加的方便快捷！

**@Component三个衍生注解**

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。

- @Controller：web层
- @Service：service层
- @Repository：dao层

写上这些注解，就相当于将这个类交给Spring管理装配了！



##### 自动装配注解



在Bean的自动装配已经讲过了，可以回顾！

##### 作用域

@scope

- singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

```java
@Controller("user")
@Scope("prototype")
public class User {
   @Value("yanzx")
   public String name;
}
```



##### xml与注解

**XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便
- 注解不是自己提供的类使用不了，开发简单方便

**xml与注解整合开发** ：推荐最佳实践

- xml管理Bean
- 注解完成属性注入
- 使用过程中， 可以不用扫描，扫描是为了类上的注解

```xml
<context:annotation-config/>  
```

作用：

- 进行注解驱动注册，从而使注解生效

- 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册

- 如果不扫描包，就需要手动配置bean

- 如果不加注解驱动，则注入的值为null！

  

##### 基于Java类进行配置

JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息，在 Spring4 的版本， JavaConfig 已正式成为 Spring4 的核心功能 。

测试：

1、编写一个实体类，Dog

```java
@Component  //将这个类标注为Spring的一个组件，放到容器中！
public class Dog {
   public String name = "dog";
}
```

2、新建一个config配置包，编写一个MyConfig配置类

```java
@Configuration  //代表这是一个配置类
public class MyConfig {

   @Bean //通过方法注册一个bean，这里的返回值就Bean的类型，方法名就是bean的id！
   public Dog dog(){
       return new Dog();
  }

}
```

3、测试

```java
@Test
public void test2(){
   ApplicationContext applicationContext =
           new AnnotationConfigApplicationContext(MyConfig.class);
   Dog dog = (Dog) applicationContext.getBean("dog");
   System.out.println(dog.name);
}
```

4、成功输出结果！

**导入其他配置如何做呢？**

1、我们再编写一个配置类！

```java
@Configuration  //代表这是一个配置类
public class MyConfig2 {
}
```

2、在之前的配置类中我们来选择导入这个配置类

```java
@Configuration
@Import(MyConfig2.class)  //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class MyConfig {

   @Bean
   public Dog dog(){
       return new Dog();
  }

}
```

关于这种Java类的配置方式，我们在之后的SpringBoot 和 SpringCloud中还会大量看到，我们需要知道这些注解的作用即可！

## AOP

https://blog.csdn.net/mu_wind/article/details/102758005

### 静态代理

### 动态代理

### 面向切面编程

## 作业解答

### springboot整合mybatis

http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

![image-20210404170118176](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/04/170743-942580.png)

### 算法



#### 第一题

```java
//二进制数转字符串
public class Task1 {
    public static void main(String[] args) {
        System.out.println(printBin(0.625));
    }

    //解题思路：十进制的小数转换为二进制小数，主要是利用小数部分乘2后，取整数部分，直至小数点后为0
    public static String printBin(double num) {
        StringBuilder ans = new StringBuilder("0.");
        while (num != 0) {
            num *= 2;
            if (num >= 1) { //乘2后num>=1,说明此时整数部分为1，取完该整数部分1后，num接着利用的还是其小数部分，所以要减掉整数部分（即1）
                ans.append("1");
                num -= 1;
            } else { //小于1说明整数部分为0，取该整数部分0
                ans.append("0");
            }
            if (ans.length() > 32) return "ERROR";
        }
        return ans.toString();
    }
}

```

#### 第二题

```java
/验证回文字符串
public class Task2 {
    public static void main(String[] args) {
        String s="abac";
        System.out.println(validPalindrome2(s));
    }

    //解法一：首先判断是否是回文字符串，如果不是，则删除一个字符后在进行判断
    public static boolean validPalindrome(String s) {
        if(isPalindrome(s)){
            return true;
        }else {
            for(int i=0;i<s.length();i++){
                String tmp = new StringBuilder(s).deleteCharAt(i).toString();
                if(isPalindrome(tmp)){
                    return true;
                }
            }
        }
        return false;
    }

    //*解法二：（因为解法一的效率不是很高效，所以推荐大家使用解法二）
    //双指针解法：从两侧向中间找到不等的字符，删除后判断是否回文
    public static boolean validPalindrome2(String s) {
        for(int i = 0, j = s.length()-1; i < j ; i++, j--){
            if(s.charAt(i) != s.charAt(j)){
                //分两种情况，删除右边或者删除左边
                return isPalindrome(new StringBuilder(s).deleteCharAt(i).toString()) || isPalindrome(new StringBuilder(s).deleteCharAt(j).toString());
            }
        }
        return true;
    }


    //此方法用于判断一个字符串是不是回文串
    public static boolean isPalindrome(String s){
        String reverse = new StringBuilder(s).reverse().toString();
        return reverse.equals(s);
    }
}

```

#### 第三题

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.HashSet;
import java.util.Set;

//最长单词
public class Task3 {
    public static void main(String[] args) {
        String[] words=
new String[]{"cat","banana","dog","nana","walk","walker","dogwalker"};
        System.out.println(longestWord(words));
    }

    //解题思路：先把字符串数组排序，字符串长的在前面，相同长度的字典序小的在前面，排好序后加入到set里判断是否包含，从第一个字符串开始判断，看是否由其它字符串组成
    public static String longestWord(String[] words) {
        //按照规则排序
        Arrays.sort(words, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if(o1.length() == o2.length())
                    return o1.compareTo(o2);
                else{
                    return Integer.compare(o2.length(),o1.length());
                }
            }
        });

        Set<String> set = new HashSet<>(Arrays.asList(words));
        for(String word : words){
            set.remove(word);
            if(find(set,word))
                return word;
        }
        return "";
    }

    //从第一个字符串开始判断，看是否由其它字符串组成
    public static boolean find(Set<String> set, String word){
        if(word.length() == 0)
            return true;
        for(int i = 0; i < word.length(); i++){
            if(set.contains(word.substring(0,i+1)) && find(set,word.substring(i+1)))
                return true;
        }
        return false;
    }
}

```

