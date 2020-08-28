## 泛型方法

```java
package cn.itcast.day02;

/**
 * 定义泛型方法
 */

public class GenericMethod {
    // 含有泛型的普通方法
    public<M> void method01(M m){
        System.out.println(m);
    }
    // 含有泛型的静态方法
    public static <S> void method02(S s){
        System.out.println(s);
    }
}
```

```java
package cn.itcast.day02;

public class GenericMethedTest {
    public static void main(String[] args) {
        GenericMethod gm = new GenericMethod();
        gm.method01("闫振兴");

        // 静态泛型方法
        GenericMethod.method02(true);
        GenericMethod.method02(1212);

    }
}
```



## 泛型类

```java
package cn.itcast.day02;

/**
 * 定义一个泛型类
 */
public class GenericClass<E> {
    private E name;

    public void setName(E name) {
        this.name = name;
    }

    public E getName(){
        return name;
    }
}
```

```java
package cn.itcast.day02;

public class GernericClassTest {
    public static void main(String[] args) {
        // 1.不使用泛型
        GenericClass gc = new GenericClass();
        gc.setName("yanzx");
        Object obj = gc.getName();
        System.out.println(obj);

        // 2.使用泛型
        GenericClass<String> gcString = new GenericClass<>();
        gcString.setName("yanzx  yanzx ");
        String name = gcString.getName();
        System.out.println(name);
    }
}
```



## 泛型接口

```java
package cn.itcast.day02;

public interface GenericInterface<T> {
    public T text();
}
```

```java
package cn.itcast.day02;

public class GenericInterfaceImpl implements GenericInterface<String> {
    @Override
    public String text() {
        System.out.println("aynzx ");
        return "yanzx";
    }

    public static void main(String[] args) {
        GenericInterfaceImpl demo = new GenericInterfaceImpl();
        String text = demo.text();
        System.out.println(text);
    }
}
```

## 泛型通配符

```java
package cn.itcast.day02;

import java.util.ArrayList;

/**
 * 泛型通配符 ArrayList <?>
 * 只能作为方法的参数使用
 */

public class TongPeiFu {
    public static void main(String[] args) {
        ArrayList<String> s = new ArrayList<>();
        s.add("yan");
        s.add("振兴");
        ArrayList<Integer> i = new ArrayList<>();
        i.add(1);
        i.add(2);
        i.add(3);
        show2(s);
        show2(i);
    }

    private static void show2(ArrayList<?> list) {
        for (Object obj: list){
            System.out.println(obj);
        }

    }
}
```







```java

// 参数
// 只能传递number的本身或子类
Collection<? extends Number >

    
// 只能传递本身或number的父类
Collection<? super Number >
```

