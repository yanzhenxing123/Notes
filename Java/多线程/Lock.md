```java
package cn.itcast.day03.Lock;
/*
解决三：使用Lock接口
 */

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Sale implements Runnable{
    private int ticket = 100;
    // 创建锁对象
    Lock l = new ReentrantLock();



    @Override
    public void run() {
        while (true) {
            try {
                l.lock();
                if (ticket > 0){
                    System.out.println(Thread.currentThread().getName() + "正在卖---->" + ticket);
                    ticket--;
                }else {
                    break;
                }
                Thread.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }finally {
                l.unlock();
                // 无论程序是否发生异常，都会释放掉锁对象
            }

        }

    }
}
```





```java
package cn.itcast.day03.Lock;

public class Demo01 {
    public static void main(String[] args) {
        // 创建接口的实现对象
        // 注意创建的是一个对象 所以买的票是共享的
        Sale s = new Sale();
        Thread t = new Thread(s);
        Thread t1 = new Thread(s);
        Thread t2 = new Thread(s);
        t.start();
        t1.start();
        t2.start();
    }
}
```

**注意**：锁的位置一定是具有安全性的那一块代码。并且一定记着释放锁。

