# java中的lambda表达式

+ Lambda表达式只能用来简化仅包含一个public方法的接口的创建

1. 只能是接口
   否则报：Target type of a lambda conversion must be an interface
2. 只能有一个public方法
   否则报：Multiple non-overriding abstract methods found xxx

双冒号表达形式

1. 与接口方法参数一样
2. 当然接口只能有一个public方法
   否则报错：AInterface is not a functional interface
3. 方法与接口的权限可以不一样
4. 返回类型：如果接口里面方法是void，双冒号后的方法可以任意返回类型，否则要一致

```java
public class Go {
    public static void main(String a[]) {
        //之前的写法
        testA(new AInterface() {
            @Override
            public void xxx(int i, int j) {
                
            }
        });
        //正确，相对与接口里面xxx方这是改成静态和换了个名字
        testA(Go::mydog);
        //正确，加了返回类型和public换成private，也是ok
        testA(Go::mydog2);
        
        //错误：Non-static method cannot be referenced from a static context
        testA(Go::mydog3);

        //这样写也是ok的。
        AInterface aInterface = Go::mydog;
        testA(aInterface);
    }

    public static void testA(AInterface t) {
        t.xxx(1, 2);
    }


    interface AInterface {
        void xxx(int i, int j);
    }

    public static boolean mydog(int i, int j) {
        System.out.println("mydog" + i + " & " + j);
        return false;
    }


    private static void mydog2(int i, int j) {
        System.out.println("mydo2" + i + " & " + j);
    }

    public void mydog3(int i, int j) {
        System.out.println("mydog3" + i + " & " + j);
    }
}

```

