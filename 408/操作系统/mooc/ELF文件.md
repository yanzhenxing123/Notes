# 逆向分析 ELF文件

## **目标文件的构成**

编译器编译源码后生成的文件叫目标文件（Object File）。目标文件从结构上讲，它是已编译后的可执行文件格式，只是还没有经过链接过程，其中有些符号或有些地址还没有调整。

目标文件本身就是按照可执行文件格式存储的，只是跟真正的可执行文件在结构上稍有不同。



### 格式

目标文件就是源代码编译之后但未进行链接的哪些中间文件（Windows 下 是 .obj，linux 下是.o）。它与可执行文件的内容和结构基本相同。

目标文件和可执行文件一般**采用同一种格式**，这种存储格式为 **ELF**（linux Executable Linkable Format），window 下 是 PE（Portable Executable）

在 linux 下 除了目标文件和可执行文件采用 ELF 格式外，静态库文件、动态库文件也都按照可执行文件 ELF 的格式存储。

| ELF 文件类型                    | 说明                                                         | 实例                                            |
| ------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- |
| 可重定位文件(Relocatable File)  | 此类文件包含代码和数据、可被链接为可执行文件或共享目标文件，静态链接库被归为此类 | Linux 下的 .o：hello.o                          |
| 可执行文件(Executable File)     | 此类文件包含了可以直接执行的文件                             | /bin/bash等                                     |
| 共享目标文件(Share Object File) | 此类文件包含了可以直接执行的文件                             | linux 的.so、/lib/x86_64-linux-gnu/libc-2.23.so |
| 核心转储文件(Core Dump File)    | 当进程意外终止，系统生成的核心转储文件                       | Linux 下的 core dump文件                        |



linux 可以通过 file 命令查看。目标文件和可执行文件跟操作系统和编译器密切相关，不同平台下的格式会有些差异。

**目标文件：**

- 文件格式：ELF 64bit
- LSB：小端
- x86-64：平台 (和工具链有关)
- relocatable：可重定位
- not stripped：没有 stripped

![img](https://pic2.zhimg.com/v2-77a46f411451a70c91ef891c3c403ffd_r.jpg)

**可执行程序：**

- 文件格式：ELF 64bit
- LSB：小端
- executable：可执行
- x86-64：平台
- statically linked：静态链接
- not stripped：没有 stripped

![img](https://pic3.zhimg.com/v2-4e2781bc1d0051e943634d0ebdbcb5c2_r.jpg)

**动态库：**

- 文件格式：ELF 64bit
- LSB：小端
- share object：共享文件
- x86-64：平台
- dynamically linked：动态链接

![img](https://pic3.zhimg.com/v2-af8966bf3b616fb152c1ff9e46b7f4c2_r.jpg)

### 组成

目前文件中的内容至少有编译后的机器指令代码和数据，除了这些，目标文件中还包含了链接时所必须的一些信息，比如符号表、调试信息、字符串等。

一般目标文件将这些信息按照不同的属性，以段（segment）的形式存储。

![img](https://pic1.zhimg.com/80/v2-4cceb23728732fcdf731e6149f8a0194_1440w.jpg)



- **代码段**（.text）：源代码编译过后的**机器指令**。

  eg：mov a b , ldr a, .... 

- 数据段（.data）：全局变量和局部静态变量被放在数据段，已经初始化的变量放在数据段。

- 只读数据段（.rodata）：const 修饰的变量和其他字符串常量。

- bss 段：为未初始化的符号，预留足够的空间。未初始化的变量在 bss 段。

- 其他段。

## **目标文件的分析**

下面我们根据一段代码来分析目标文件的内部结构（以 MIPS 平台为例）：

![img](https://img-blog.csdn.net/20150613232132687?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VubnkwNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### **ELF 文件头**

ELF 文件开头是一个头文件，它描述的是整个文件的属性，包括：文件的类型、目标硬件、目标操作系统等信息。

readelf -h 可读取 elf 文件头。头文件包含如下：

- ELF 魔数。
- 文件机器字节长度。
- 数据存储方式。
- 版本。
- 运行平台。
- ABI 版本。
- ELF 重定位类型。
- 硬件平台。
- 硬件平台版本。
- 入口地址（目标文件入口地址为 0，只有相对位置）。
- 程序的入口和长度。
- 段表的位置和长度。



![img](https://pic2.zhimg.com/v2-01fb278b9d1b45ad1d445592a15eb779_r.jpg)



**目标文件 ELF 头**

![img](https://pic2.zhimg.com/v2-43bb4aaedd1821334b61eb3b70aa9c15_r.jpg)

**可执行程序 ELF 头**

![img](https://pic4.zhimg.com/v2-819017448c7e2e730a3fe958e9fe970f_r.jpg)

### 段表

头文件之后，紧接着是一个段表，段表是一个描述文件各个段的数组。描述了文件中各个段在文件中的偏移位置以及段的属性。（readelf -S, objdump -h）

主要的段如下：

- 代码段(.text)。
- 数据段(.data)。
- bss 段。
- 只读数据段(.rodata)。
- .comment
- （符号表段）.symtab
- 符号表(.shstrtab)：Section String table
- （重定位代码段）.rel.text
- 等

![img](https://pic2.zhimg.com/v2-10411ad4e4d06b7858c8746252f30fbd_r.jpg)

各段详细信息及字节对齐：

![img](https://pic1.zhimg.com/v2-ff4a93a789684d2fec9e4e4700f76e74_r.jpg)

最终组成结构如下。中间Section Table 和 其他某些段因为对齐原因，会有若干字节的间隔。整个目标文件的大小刚好是 1588 字节。

![img](https://pic4.zhimg.com/v2-884a27d93391d6a8a17e9484ccb826af_r.jpg)

**代码段**

C 语言编译后的机器指令，都保存在代码段（.text）。我们可以通过 objdump 来查看段的信息。-s 可以将所有段以 16 进制打印出来。-d 可以将所有包含指令的段反汇编。

可以看到代码段保存的是 f 和 main 的指令。



![img](https://pic3.zhimg.com/v2-4a6b4a71975e42c92ddd5b0e6da9e24e_r.jpg)



![img](https://pic2.zhimg.com/80/v2-c3737ea2d67f3a4438af05191e083f1d_1440w.jpg)

**数据段**

数据段（.data）保存的是哪些已经初始化（非零）的全局变量（静态变量和非静态变量）和局部静态变量。

gint_val 和 static_val 存在数据段。0x64 和 0x65 刚好对应 100 和 101（ASCII d 和 e）

数据段是 16 字节对齐所以刚好占 16 个字节大小。对齐和平台有关。

![img](https://pic2.zhimg.com/v2-1524e1f8aabd10c29fe39a9dfbbb574d_r.jpg)

**只读数据段**

只读数据段（.rodata）,保存的是只读数据。一般是程序中 const 修饰的只读变量 和字符串常量（包括 printf 函数中的格式化字符串%d）。可以看到，我们可以通过优化.rodata 段的大小，进而优化程序的大小。

![img](https://pic1.zhimg.com/v2-e385b06e79a40f23ba496a53cb9b8820_r.jpg)

**bss 段**

bss 段（.bss），用来记录所有未初始化的全局变量（或者零初始化）和局部静态变量大小总和，然后为其预留位置。

未初始化的全局变量（或者零初始化）和局部静态变量，因为都是 0，所以在.data 段开辟存储空间存储 0 是没有必要的。

**符号表段**

符号表（.symtab），以数组结构的形式保存符号信息（函数和变量），对于函数和变量符号值就是他们的地址。(readelf -s)

![img](https://pic3.zhimg.com/v2-19eb0bd6897bd2eb201f1f8a8b11a5a6_r.jpg)

- f 和 main 函数的 Ndx 对应的值是 1，表示在.text 段（.text 段在段表中的索引是 1）,类似是 FUNC，value 分别是 0x00000000 和 0x00000040，表明两个函数指令字节码的首字节分别在.text 的 0x00000000 和 0x00000040 偏移处。
- printf 的 Ndx 是 UND，表明这个符号在 object_file.o 里面没有被定义，仅仅是引用。
- static_val.1843 和 gint_val 两个符号的 Ndx 都是 3，说明他们都被定义在数据段。value 分别是 0x00000000 和 0x00000004，表明两个函数指令字节码的首字节分别在.data 的 0x00000000 和 0x00000004 偏移处。

**重定位段表**

重定位表也是一个段，用于描述在重定位时连接器如何修改相应段里的内容。对于.text，对应的重定位表是.rel.text。使用objdump -r 查看重定位表。

![img](https://pic2.zhimg.com/v2-ab4bf8b83a7cf4ec8f2f9496d24ed319_r.jpg)

**自定义段表**

正常情况下，GCC 编译出来的目标文件会被放到.text 段，全局变量和静态变量会被放到.data 和.bss 段。

但是有些时候我们希望有些变量或者代码可以放到我们指定的段中去，以实现某些特定的功能。GCC 提供了一种扩展机制，可以指定段。

变量：

```c
__attribute__((section("start_var_init"))) int init_status = 1;
```



函数：

```c
__attribute__((section("start_fun_init"))) void start_func()
{    
    //init code    
    return ;
}
```



在全局变量或者函数前加入 _*attribute_*((section("name"))) 属性就可以把相应的变量或者函数放入到”name“段里。

示例：

![img](https://pic1.zhimg.com/v2-42b4b4a7b15f84268005b49db26b8218_r.jpg)

![img](https://pic1.zhimg.com/v2-22ebecc17aa74ad7995a8ea2c6cd3dc8_r.jpg)



![img](https://pic3.zhimg.com/v2-794c1787bb2e5e2940c6d27b54cf65b6_r.jpg)

##  总结

本文主要介绍了，ELF 文件组成结构，通过分析目标文件的组成，来理解程序的具体分布。了解目标文件的各段内容和作用，有助于我们提高对程序的掌控力。比如优化程序对应用程序进行加解密、调试等等。

### 相关命令

| 命令       | 说明                             |
| ---------- | -------------------------------- |
| readelf -h | 读取ELF文件头                    |
| readelf -S | 查看段的属性                     |
| readelf -s | 查看符号表                       |
| objdump -h | 查看段的属性                     |
| objdump -s | 将所有段的内容以十六进制打印出来 |
| objdump -d | 将所有包含的指令反汇编           |
| objdump -r | 查看重定位段                     |

**分段说明**

| 段名      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| .text     | 代码段。存放可执行文件的指令，这部分区域在程序运行前就已经确定。通过 objdump -s -d 查看 |
| .data     | 数据段。保存已经初始化（非零初始化）的全局变量和静态局部变量 |
| .bss      | bss段。未初始化（零初始化）的全局变量和静态局部变量保存在bss段，准确来说.bss段为他们预留了位置，等到最终链接时在分配到.bss段（具体和编译器有关） |
| .rodata   | 只读数据段。存放的是只读数据，一般是程序里面的只读变量(const修饰的变量)和字符串变量（printf 的格式化字符也算） |
| .comment  | 存放的是编译器的版本信息                                     |
| .symtab   | 符号表段。用来定位、重定位程序中符号定义和引用的信息，简单的理解就是符号表记录了该文件中的所有符号，所谓的符号就是经过修饰了的函数名或者变量名，不同的编译器有不同的修饰规则。 |
| .shstrtab | 字符串表段。存放着所有符号的名称字符串                       |
| .dynamic  | 动态链接信息。                                               |
| ...       | 其他                                                         |



