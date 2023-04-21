# hashCode与equals的区别和联系

## Object

源码

```java
    public boolean equals(Object obj) {
        return (this == obj);
    }
```

比较的是引用所指向的地址，即是不是同一个对象。



## hashcode

hashcode方法只有在集合中用到



如果重写后的hashcode方法中，某个属性参与到hashcode计算，那么就不能随意修改元素的值。

```java
package top.yanzx.test;

/**
 * @Author: yanzx
 * @Date: 2021/4/12 18:45
 * @Description:
 */
public class Hero {
    private int age;

    private String name;

    public void setAge(int age) {
        this.age = age;
    }



    public Hero(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        Hero obj1 = (Hero) obj;
        if (this.age == obj1.age){
            return true;
        }
        return false;

    }

    @Override
    public String toString() {
        return "Hero{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }

    @Override
    public int hashCode() {
        return this.age;
    }

}
```



测试

```java
package top.yanzx.test;

import java.util.HashSet;
import java.util.Set;

/**
 * @Author: yanzx
 * @Date: 2021/4/12 18:46
 * @Description:
 */
public class Main {
    public static void main(String[] args) {
        Hero hero = new Hero(20, "1");
        Hero hero2 = new Hero(20, "2");
        Set<Hero> heroSet = new HashSet<>();
        heroSet.add(hero);
        heroSet.add(hero2);
        System.out.println(heroSet.size());

//        System.out.println(heroSet);
        for (Hero hero1 : heroSet) {
            System.out.println(hero1);
        }

        // 修改age
        hero.setAge(100);
        // 移除元素
        heroSet.remove(hero);

        for (Hero hero1 : heroSet) {
            System.out.println(hero1);
        }


    }
}
```



## 集合类增删元素

add：先比较hashcode，如果不相同，那么加入元素，相同，再比较equals，不相同，插进去，相同，不插进去。

remove：比较hashcode

<img src="https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202105/19/114439-20637.png" alt="image-20210519114438751" style="zoom:80%;" />

### add

hashset：

```java
public boolean add(E e) {
	return map.put(e, PRESENT)==null;
    }
```

map.put源代码:

```java
 public V put(K key, V value) {
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key.hashCode());
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
 
        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```

### remove

HashSet中remove

```java
	public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }

```

ctrl+点击remove得到HashMap中remove

```java
public V remove(Object key) {
        Node<K,V> e;
        return (e = removeNode(hash(key), key, null, false, true)) == null ?
            null : e.value;
    }

```

可见并没有方法的具体过程，继续ctrl+点击removeNode得到HashMap中removeNode方法

```java
final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; K k; V v;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                if (p instanceof TreeNode)
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                             (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                ++modCount;
                --size;
                afterNodeRemoval(node);
                return node;
            }
        }
        return null;
    }

```

