# IP协议



IP协议是一个统称，是将IP获取路由表的协议

网络畅通的条件：数据包有去有回



## 静态路由

静态路由需要管理员告诉路由器所有没有直连的网络下一跳给谁

![image-20200612214322860](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200612214322860.png)





缺点：只适合小规模的网络 不能自动调增路由

## 动态路由

### RIP（不作为学习的重点）

**开放式的动态路由协议**

**自动选择最佳的路径 周期性的更新 每隔30秒更新 选择路径的标准是路由器的跳数 最大跳数是15跳 也不适合网络规模比较大的网络。**



### OSIP（也是动态路由协议）

**选择最佳路径是带宽 与上面不同的就是跳数和带宽之类的**





