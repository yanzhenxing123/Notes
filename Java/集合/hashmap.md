# hashmap

## 源码

HashMap由数组+链表组成的，**数组是HashMap的主体**，链表则是主要为了解决哈希冲突而存在的，如果定位到的数组位置不含链表（当前entry的next指向null）,那么对于查找，添加等操作很快，仅需一次寻址即可；如果定位到的数组包含链表，对于添加操作，其时间复杂度为O(n)，首先遍历链表，存在即覆盖，否则新增；对于查找操作来讲，仍需遍历链表，然后通过key对象的equals方法逐一比对查找。所以，性能考虑，HashMap中的链表出现越少，性能才会越好。

```java
public class HashMap<K,V>extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
```

基本属性

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; //默认初始化大小 16 
static final float DEFAULT_LOAD_FACTOR = 0.75f;     //负载因子0.75
static final Entry<?,?>[] EMPTY_TABLE = {};         //初始化的默认数组
transient int size;     //HashMap中元素的数量
int threshold;          //判断是否需要调整HashMap的容量  
```

HashMap线程不安全

多线程环境下要使用 `CurrentHashMap`

## 区别与hashtable

hashtable线程安全，有synchronized关键字。

hashmap可以用null作为关键字，HashMap以null作为key时，总是存储在table数组的第一个节点上。

hashtable则不可以以null作为关键字。



