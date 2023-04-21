# 

进程



## 进程之间的切换

进程也就是操作系统管理CPU，

```c++
if (!fork()){
    子进程所执行的代码
} else {
    父进程所执行的代码
}
```

`fork()` 子进程返回0， 父进程返回子进程id

## 多进程图像

就绪进程将PCB放在就绪队列中，等待进程将PCB放在等待队列中，

有程序在执行；有一些进程在等待执行，放在就绪队列中；有一些进程在等待某个事件，放在 磁盘等待队列
PCB 用来记录进程信息的 数据结构，帮助系统 感知、管理进程。

### 多进程之间的交替

![image-20210410210544121](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/10/211937-866921.png)





用PCB记录每一个进程。

进程交替的三个部分：队列操作+调度+切换
进程调度是一个很深刻的话题，如今也是 值得研究的课题

### 进程之间的调度切换

+ FIFO：先进先出，公平策略，没有考虑进程执行的任务的区别

+ +Priority：优先级，优先级怎么设定？依靠 优先级，可能会导致进程饥饿。

  切换进程时需要：
  保存 正在执行的进程的 现场：将 物理CPU的该进程的信息，包存到 一个结构体 PCB中
  恢复 要执行的进程的 现场：将该进程对应的PCB 恢复到 物理CPU中
  该过程需要 精细控制，需要用 汇编实现

>  锁实现生产者消费者counter不一致的问题：

```c
//生产者进程
// --给counter上锁
P.register = counter;//counter = 5
P.register = P.register +1; // P.register = 6, counter = 5

//此时消费者要执行，检查锁，发现counter锁上了，不能执行消费者代码

//生产者 继续执行
counter = P.register; // counter = 6
// --给counter解锁

//消费者执行
// -- 给counter上锁
C.register = counter;
C.register = C.register - 1;
counter = C.register; // counter = 5

```

