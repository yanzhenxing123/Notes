

<img src="https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/20/002208-359067.png" alt="image-20210420002207916" style="zoom:100%;" />

同时维护一个双向链表和字典

```python
"""
@Author: yanzx
@Date: 2021/4/19 21:09
@Description:
"""
import collections

# 继承OrderDict(有序字典)实现(相当于LikedHashMap)
class LRUCache1(collections.OrderedDict):

    def __init__(self, capacity: int):
        super().__init__()
        self.capacity = capacity


    def get(self, key: int) -> int:
        if key not in self:
            return -1
        self.move_to_end(key)
        return self[key]

    def put(self, key: int, value: int) -> None:
        if key in self:
            self.move_to_end(key)
        self[key] = value
        if len(self) > self.capacity:
            self.popitem(last=False)

# 同时维护链表和哈希表
class ListNode:
    def __init__(self, key, value):
        self.key = key  # 键
        self.value = value  # 值
        self.pre = None
        self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.head = ListNode(-1, -1)
        self.tail = ListNode(-1, -1)
        self.head.pre = self.tail
        self.tail.next = self.head
        self.hash_map = {}

    def move_to_end(self, node: ListNode):
        # 将给定的node放到尾部
        if self.tail.pre is None: # 如果为第一个数据
            self.head.next = node
            node.pre = self.head
        else:
            self.tail.pre.next = node
            node.pre = self.tail.pre

        node.next = self.tail
        self.tail.pre = node


    def get(self, key: int) -> int:
        # 如果字典中存在该键 就将字典中的值返回 并将这个节点放到双向链表的最后面
        node = self.hash_map.get(key)
        if not node:
            return -1
        # 如果为最后一个就不用动了
        if node.next == self.tail:
            return node.value

        # 先删除 再插到最后
        node.pre.next = node.next
        node.next.pre = node.pre
        self.move_to_end(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        # 如果存在该键，则更新
        if self.get(key) != -1:
            # 更新字段
            self.hash_map[key].value = value
            return
        # 如果不存在 那么添加到链表的尾部
        # 满了删除表头 同时删除字典中的键
        if len(self.hash_map) == self.capacity:
            delete_node = self.head.next
            self.head.next = self.head.next.next
            self.head.next.pre = self.head
            self.hash_map.pop(delete_node.key)

        # 将节点放到插到尾部
        node = ListNode(key, value)
        self.hash_map[key] = node
        self.move_to_end(node)

    # 打印过程
    def show(self):
        p = self.head.next
        print("head->", end="")
        while p != self.tail:
            print(p.key, end="->")
            p = p.next
        print("tail")

  
if __name__ == '__main__':
    # ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
    # [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
    lru = LRUCache(3)
    lru.put(1, 1)
    lru.show()
    lru.put(2, 2)
    lru.show()
    lru.get(1)
    lru.show()
    lru.put(3, 3)
    lru.show()
    lru.get(2)
    lru.show()
    lru.put(4, 4)
    lru.show()
    lru.get(1)
    lru.show()
    lru.get(3)
    lru.show()
    lru.get(4)
    lru.show()
```

执行上述过程，双向链表大致如下：

![image-20210420002222716](https://cdn.jsdelivr.net/gh/yanzhenxing123/blogImg@master/typora202104/20/002227-947735.png)