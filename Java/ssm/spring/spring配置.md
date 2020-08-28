# 配置

## 别名alias





## 配置

![image-20200809152301067](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200159-272106.png)



## import

![image-20200809152458914](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200202-974052.png)

## 命名空间注入

### p

![image-20200809162139475](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200204-822610.png)

### c

![image-20200809163004970](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201618-353450.png)

![image-20200809163046438](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/200207-810859.png)

## scope作用域

+ 单例模式 （默认机制）
+ 原型模式 ![image-20200809165632466](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200809165632466.png)
+ ![image-20200809165655633](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201547-498386.png)





## Set注入

![image-20200809165844427](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201534-923544.png)

### 普通的值注入

![image-20200809165901603](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201553-163973.png)

### Bean注入 ref 注入 链接的是一个对象

![image-20200809165913462](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201557-426780.png)

### 数组注入

![image-20200809170010446](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201600-973313.png)

### list注入

![image-20200809170032653](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201603-531087.png)

### map

![image-20200809170105814](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201612-605292.png)



### set

![image-20200809170120216](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201605-6991.png)



### null空值注入

![image-20200809170152466](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201609-829359.png)

![image-20200809170205782](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200809170205782.png)

### properties

![image-20200809170254522](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201607-609084.png)



## Bean的自动装配

### byName

![image-20200809171950705](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193208-622974.png)

### byType

![image-20200809172134367](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193211-783527.png)

![image-20200809172208869](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201641-194986.png)

### 小结

![image-20200809172307420](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201645-477448.png)

### Spring 由构造函数自动装配

这种模式与 *byType* 非常相似，但它应用于构造器参数。Spring 容器看作 beans，在 XML 配置文件中 beans 的 *autowire* 属性设置为 *constructor*。然后，它尝试把它的构造函数的参数与配置文件中 beans 名称中的一个进行匹配和连线。如果找到匹配项，它会注入这些 bean，否则，它会抛出异常。

例如，在配置文件中，如果一个 bean 定义设置为通过*构造函数*自动装配，而且它有一个带有 *SpellChecker* 类型的参数之一的构造函数，那么 Spring 就会查找定义名为 *SpellChecker* 的 bean，并用它来设置构造函数的参数。你仍然可以使用 <constructor-arg> 标签连接其余属性。下面的例子将说明这个概念。

让我们在恰当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序：

| 步骤 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| 1    | 创建一个名称为 *SpringExample* 的项目，并且在已创建的项目的 **src** 文件夹中创建一个包 *com.tutorialspoint*。 |
| 2    | 使用 *Add External JARs* 选项，添加所需的 Spring 库，在 *Spring Hello World Example* 章节中已说明。 |
| 3    | 在 *com.tutorialspoint* 包中创建 Java 类 *TextEditor*，*SpellChecker* 和 *MainApp*。 |
| 4    | 在 **src** 文件夹中创建 Beans 的配置文件 *Beans.xml*。       |
| 5    | 最后一步是创建所有 Java 文件和 Bean 配置文件的内容，并运行该应用程序，正如下面解释的一样。 |

这里是 **TextEditor.java** 文件的内容：

```java
package com.tutorialspoint;
public class TextEditor {
   private SpellChecker spellChecker;
   private String name;
   public TextEditor( SpellChecker spellChecker, String name ) {
      this.spellChecker = spellChecker;
      this.name = name;
   }
   public SpellChecker getSpellChecker() {
      return spellChecker;
   }
   public String getName() {
      return name;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```

下面是另一个依赖类文件 **SpellChecker.java** 的内容：

```java
package com.tutorialspoint;
public class SpellChecker {
   public SpellChecker(){
      System.out.println("Inside SpellChecker constructor." );
   }
   public void checkSpelling()
   {
      System.out.println("Inside checkSpelling." );
   }  
}
```

下面是 **MainApp.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      TextEditor te = (TextEditor) context.getBean("textEditor");
      te.spellCheck();
   }
}
```

下面是在正常情况下的配置文件 **Beans.xml** 文件：

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- Definition for textEditor bean -->
   <bean id="textEditor" class="com.tutorialspoint.TextEditor">
      <constructor-arg  ref="spellChecker" />
      <constructor-arg  value="Generic Text Editor"/>
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
   </bean>

</beans>
```

但是，如果你要使用自动装配 “by constructor”，那么你的 XML 配置文件将成为如下：

```java
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- Definition for textEditor bean -->
   <bean id="textEditor" class="com.tutorialspoint.TextEditor" 
      autowire="constructor">
      <constructor-arg value="Generic Text Editor"/>
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id="SpellChecker" class="com.tutorialspoint.SpellChecker">
   </bean>

</beans>
```

一旦你完成了创建源代码和 bean 的配置文件，我们就可以运行该应用程序。如果你的应用程序一切都正常，它将打印下面的消息：

```
Inside SpellChecker constructor.
Inside checkSpelling.
```



## 使用注解实现自动装配

### @Required 注释

**@Required** 注释应用于 bean 属性的 setter 方法，它表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。



### @Autowired注释

通过ByName的方式实现

**@Autowired** 注释对在哪里和如何完成自动连接提供了更多的细微的控制。

@Autowired 注释可以在 setter 方法中被用于自动连接 bean，就像 @Autowired 注释，容器，一个属性或者任意命名的可能带有多个参数的方法。

**Setter 方法中的 @Autowired**

你可以在 XML 文件中的 setter 方法中使用 **@Autowired** 注释来除去 元素。当 Spring遇到一个在 setter 方法中使用的 @Autowired 注释，它会在方法中视图执行 **byType** 自动连接。



### @Qualifier 注释

可能会有这样一种情况，当你创建多个具有相同类型的 bean 时，并且想要用一个属性只为它们其中的一个进行装配，在这种情况下，你可以使用 **@Qualifier** 注释和 **@Autowired** 注释通过指定哪一个真正的 bean 将会被装配来消除混乱。下面显示的是使用 @Qualifier 注释的一个示例。

这里是 **Student.java** 文件的内容：

```java
package com.tutorialspoint;
public class Student {
   private Integer age;
   private String name;
   public void setAge(Integer age) {
      this.age = age;
   }   
   public Integer getAge() {
      return age;
   }
   public void setName(String name) {
      this.name = name;
   }  
   public String getName() {
      return name;
   }
}
```

这里是 **Profile.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
public class Profile {
   @Autowired
   @Qualifier("student1")
   private Student student;
   public Profile(){
      System.out.println("Inside Profile constructor." );
   }
   public void printAge() {
      System.out.println("Age : " + student.getAge() );
   }
   public void printName() {
      System.out.println("Name : " + student.getName() );
   }
}
```



考虑下面配置文件 **Beans.xml** 的示例：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">

   <context:annotation-config/>

   <!-- Definition for profile bean -->
   <bean id="profile" class="com.tutorialspoint.Profile">
   </bean>

   <!-- Definition for student1 bean -->
   <bean id="student1" class="com.tutorialspoint.Student">
      <property name="name"  value="Zara" />
      <property name="age"  value="11"/>
   </bean>

   <!-- Definition for student2 bean -->
   <bean id="student2" class="com.tutorialspoint.Student">
      <property name="name"  value="Nuha" />
      <property name="age"  value="2"/>
   </bean>

</beans>
```





下面是 **MainApp.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      Profile profile = (Profile) context.getBean("profile");
      profile.printAge();
      profile.printName();
   }
}
```

一旦你在源文件和 bean 配置文件中完成了上面两处改变，让我们运行一下应用程序。如果你的应用程序一切都正常的话，这将会输出以下消息：

```java
Inside Profile constructor.
Age : 11
Name : Zara
```

### @Resource注解

自动装配 Java的本来的注解 

如果找不到 那么就使用ByType

### 小结

![image-20200809205925378](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193201-121455.png)



## 使用注解开发

1. bean

   

2. 属性怎么注入

3. 衍生的注解

4. 自动装配

5. 作用域





![image-20200810131530225](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201927-928338.png)

![image-20200810131622373](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/21/201658-753325.png)



## 使用Java的方式配置Spring

![image-20200810133443494](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193157-146970.png)





## 代理模式

[代理模式](https://zh.wikipedia.org/wiki/代理模式)是一种设计模式，提供了对目标对象额外的访问方式，即通过代理对象访问目标对象，**这样可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。**



简言之，代理模式就是设置一个中间代理来控制访问原目标对象，以达到增强原对象的功能和简化访问方式。



![image-20200810153501421](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193153-381890.png)



示例：

![image-20200810135216356](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193151-250037.png)

### 静态代理



**这种代理方式需要代理对象和目标对象实现一样的接口。**





优点：可以在不修改目标对象的前提下扩展目标对象的功能。

缺点：

1. 冗余。由于代理对象要实现与目标对象一致的接口，会产生过多的代理类。
2. 不易维护。一旦接口增加方法，目标对象与代理对象都要进行修改。



> 举例：保存用户功能的静态代理实现

- 接口类： IUserDao

```java
package com.proxy;

public interface IUserDao {
    public void save();
}
```



- 目标对象：UserDao

```java
package com.proxy;

public class UserDao implements IUserDao{

    @Override
    public void save() {
        System.out.println("保存数据");
    }
}
```

- 静态代理对象：UserDapProxy **需要实现IUserDao接口！\*

```java
package com.proxy;

public class UserDaoProxy implements IUserDao{

    private IUserDao target;
    public UserDaoProxy(IUserDao target) {
        this.target = target;
    }
    
    @Override
    public void save() {
        System.out.println("开启事务");//扩展了额外功能
        target.save();
        System.out.println("提交事务");
    }
}
```



- 测试类：TestProxy

```java
package com.proxy;

import org.junit.Test;

public class StaticUserProxy {
    @Test
    public void testStaticProxy(){
        //目标对象
        IUserDao target = new UserDao();
        //代理对象
        UserDaoProxy proxy = new UserDaoProxy(target);
        proxy.save();
    }
}
```



- 输出结果

```
开启事务
保存数据
提交事务
```



### 动态代理

**使用反射实现**

*动态代理利用了[JDK API](http://tool.oschina.net/uploads/apidocs/jdk-zh/)，动态地在内存中构建代理对象，从而实现对目标对象的代理功能。动态代理又被称为JDK代理或接口代理。*

**静态代理和动态代理的区别**：

- 静态代理在编译时就已经实现，编译完成后代理类是一个实际的class文件
- 动态代理是在运行时动态生成的，即编译完成后没有实际的class文件，而是在运行时动态生成类字节码，并加载到JVM中

JDK中生成代理对象主要涉及的类有

- [java.lang.reflect Proxy](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/reflect/Proxy.html)，主要方法为

```java
static Object    newProxyInstance(ClassLoader loader,  //指定当前目标对象使用类加载器

 Class<?>[] interfaces,    //目标对象实现的接口的类型
 InvocationHandler h      //事件处理器
) 
//返回一个指定接口的代理类实例，该接口可以将方法调用指派到指定的调用处理程序。
    
```

**实例化的动态代理**

![image-20200810164203684](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193141-679000.png)



- [java.lang.reflect InvocationHandler](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/reflect/InvocationHandler.html)，主要方法为

```java
 Object    invoke(Object proxy, Method method, Object[] args) 
// 在代理实例上处理方法调用并返回结果。
```



> 举例：保存用户功能的动态代理实现

- 接口类： IUserDao

```java
package com.proxy;

public interface IUserDao {
    public void save();
}
```

- 目标对象：UserDao

```java
package com.proxy;

public class UserDao implements IUserDao{

    @Override
    public void save() {
        System.out.println("保存数据");
    }
}
```

* 动态代理对象 UserProxyFactory
  



```java
package com.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyFactory {

    private Object target;// 维护一个目标对象

    public ProxyFactory(Object target) {
        this.target = target;
    }

    // 为目标对象生成代理对象
    public Object getProxyInstance() {
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),
                new InvocationHandler() {

                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("开启事务");

                        // 执行目标对象方法
                        Object returnValue = method.invoke(target, args);

                        System.out.println("提交事务");
                        return null;
                    }
                });
    }
}
```

**等价于：**

![image-20200810165329944](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193137-62666.png)



- 测试类：TestProxy

```java
package com.proxy;

import org.junit.Test;

public class TestProxy {

    @Test
    public void testDynamicProxy (){
        IUserDao target = new UserDao();
        System.out.println(target.getClass());  //输出目标对象信息
        IUserDao proxy = (IUserDao) new ProxyFactory(target).getProxyInstance();
        System.out.println(proxy.getClass());  //输出代理对象信息
        proxy.save();  //执行代理方法
    }
}
```

**测试类**

![image-20200810165437456](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193059-882703.png)











- 输出结果

```
class com.proxy.UserDao
class com.sun.proxy.$Proxy4
开启事务
保存数据
提交事务
```

### cglib代理

> cglib is a powerful, high performance and quality Code Generation Library. It can extend JAVA classes and implement interfaces at runtime.

[cglib](https://github.com/cglib/cglib) (Code Generation Library )是一个第三方代码生成类库，运行时在内存中动态生成一个子类对象从而实现对目标对象功能的扩展。

**cglib特点**

- JDK的动态代理有一个限制，就是使用动态代理的对象必须实现一个或多个接口。
  如果想代理没有实现接口的类，就可以使用CGLIB实现。
- CGLIB是一个强大的高性能的代码生成包，它可以在运行期扩展Java类与实现Java接口。
  它广泛的被许多AOP的框架使用，例如Spring AOP和dynaop，为他们提供方法的interception（拦截）。
- CGLIB包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。
  不鼓励直接使用ASM，因为它需要你对JVM内部结构包括class文件的格式和指令集都很熟悉。

cglib与动态代理最大的**区别**就是

- 使用动态代理的对象必须实现一个或多个接口
- 使用cglib代理的对象则无需实现接口，达到代理类无侵入。

使用cglib需要引入[cglib的jar包](https://repo1.maven.org/maven2/cglib/cglib/3.2.5/cglib-3.2.5.jar)，如果你已经有spring-core的jar包，则无需引入，因为spring中包含了cglib。

- cglib的Maven坐标

```xml
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.2.5</version>
</dependency>
```

> 举例：保存用户功能的动态代理实现

- 目标对象：UserDao

```java
package com.cglib;

public class UserDao{

    public void save() {
        System.out.println("保存数据");
    }
}
```

- 代理对象：ProxyFactory

```java
package com.cglib;

import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class ProxyFactory implements MethodInterceptor{

    private Object target;//维护一个目标对象
    public ProxyFactory(Object target) {
        this.target = target;
    }
    
    //为目标对象生成代理对象
    public Object getProxyInstance() {
        //工具类
        Enhancer en = new Enhancer();
        //设置父类
        en.setSuperclass(target.getClass());
        //设置回调函数
        en.setCallback(this);
        //创建子类对象代理
        return en.create();
    }

    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("开启事务");
        // 执行目标对象的方法
        Object returnValue = method.invoke(target, args);
        System.out.println("关闭事务");
        return null;
    }
}
```

- 测试类：TestProxy

```java
package com.cglib;

import org.junit.Test;

public class TestProxy {

    @Test
    public void testCglibProxy(){
        //目标对象
        UserDao target = new UserDao();
        System.out.println(target.getClass());
        //代理对象
        UserDao proxy = (UserDao) new ProxyFactory(target).getProxyInstance();
        System.out.println(proxy.getClass());
        //执行代理对象方法
        proxy.save();
    }
}
```

- 输出结果

```
class com.cglib.UserDao
class com.cglib.UserDao$$EnhancerByCGLIB$$552188b6
开启事务
保存数据
关闭事务
```









## AOP 面向切面编程

![Aop的实现机制](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193052-93282.png)



## spring整合mybatis

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>spring-stfy</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>spring--04-mybatis</artifactId>


    <dependencies>
<!--        <dependency>-->
<!--            <groupId>junit</groupId>-->
<!--            <artifactId>junit</artifactId>-->
<!--            <version>4.12</version>-->
<!--        </dependency>-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.17</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>

<!--        <dependency>-->
<!--            <groupId>org.springframework</groupId>-->
<!--            <artifactId>spring-webmvc</artifactId>-->
<!--            <version>5.2.0.RELEASE</version>-->
<!--        </dependency>-->

<!--spring操作数库的话-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.7.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.5</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.3</version>
        </dependency>





    </dependencies>


</project>
```

### mybatis

![image-20200811135630507](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193111-413001.png)

### 整合步骤

![image-20200811153556565](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193112-658305.png)

## 自动装配

![image-20200820193124217](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/20/193125-938730.png)