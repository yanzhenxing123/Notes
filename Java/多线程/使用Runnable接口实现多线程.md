





```java
package cn.itcast.day03.CreateNewThread2;

public class RunableImpl implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 60; i++) {
            System.out.println(Thread.currentThread().getName()+"--->" + i);

        }
    }
}
```







```java
package cn.itcast.day03.CreateNewThread2;


/*
创建多线程的第二种方式就是 实现Runnable的接口
然后在main方法中创建对象 再创建Thread对象，
并调用构造方法，用t进行初始化，
最后调用start方法

好处：避免了单继承 可以实现更多的接口

 */

public class Demo01Runable  {
    public static void main(String[] args) {
        RunableImpl run = new RunableImpl();
        Thread t = new Thread(run);
        t.start();
        for (int i = 0; i < 60; i++) {
            System.out.println(Thread.currentThread().getName()+"--->" + i);
        }
    }
}
```