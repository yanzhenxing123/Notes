# java类中的equals方法

**Object类中equals比较的是两个之间的地址**

这个方法是可以重载的，比如String类型，就进行了重载了，比较内容而不是地址

```java
String a = "abc";
String b = new String("abc");
System.out.println(a==b);  // 返回false
System.out.println(a.equals(b)); // 返回true
```





**与`==`的区别**

==就是引用类型的地址，而不是值。

