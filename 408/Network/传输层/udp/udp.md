udp是面向报文的：

![image-20200506170240183](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200506170240183.png)



udp的首部格式：

![image-20200506170404012](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200506170404012.png)

![image-20200506170421848](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200506170421848.png)

伪首部只是用来进行校验，不进行传输。

有一点点差错检验的能力，虽然很简单，但是很有效。

不提供可靠交付。虽然由校验和，但是顺序不一定一样。

![image-20200506170857400](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200506170857400.png)

udp的设计就是为了四个字”提高效率“

