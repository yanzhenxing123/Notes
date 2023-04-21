# basis

## 开机关机

![image-20200825142902897](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/142903-25745.png)

## 系统目录结构

1. 一切皆文件
2. 根目录/， `ls /`展示根目录下的所有文件



****

![image-20200825143409850](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/143410-328069.png)

![image-20200825143619862](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/143620-476489.png)

## 基本命令

### 目录管理

+ ls 
  + -a 
  + -l
  + -al
+ cd

![image-20200825150228612](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/150229-197982.png)

+ pwd 显示用户当前所在的目录
+ mkdir 创建目录
  +   mkdir - p test1/test2/test3 递归创建 创建多级目录 
+ rmdir 删除文件夹
  +  rmdir - p test1/test2/test3 删除多级目录 
+ cp 复制文件或者目录
  + cp test kuangstudy
+ rm 移除文件或dir

![image-20200825150905644](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/150906-525555.png)

+ mv

![image-20200825151014510](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200825151014510.png)

重命名文件夹 mv old_dir new_dir

## 文件内容查看

![image-20200825152207148](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/152207-958265.png)

head -n 20 看前20行

tail -n 20 倒着看

![image-20200825152840198](C:\Users\20924\AppData\Roaming\Typora\typora-user-images\image-20200825152840198.png)



## 硬链接和软链接

![image-20200825153130677](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/153131-840970.png)

![image-20200825153238307](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/153336-858541.png)

![image-20200825153339349](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/153340-319960.png)



## 进程

![image-20200825162328130](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202008/25/162329-631281.png)

### 杀死进程

kill -9 id

