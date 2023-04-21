```python
# 节点
class Node():
    def __init__(self, value = None, next = None):
        self._value = value
        self._next = next

    def getValue(self):
        return self._value

    def getNext(self):
        return self._next

    def setValue(self, new_value):
        self._value = new_value

    def setNext(self, new_next):
        self._next = new_next

# 链表类
class LinkedList():
    def __init__(self):
        self._head = Node()
        self._tail = None
        self._length = 1

    # 判断是否为空
    def isEmpty(self):
        return self._head ==  None

    # 在链表前端添加Node
    def add(self, value):
        # 创建新节点
        self._length += 1
        new_node = Node(value, None)
        # 指向头部的节点
        new_node.setNext(self._head)
        # 头部指向新的节点
        self._head = new_node

    def append(self, value):

        self._length += 1
        new_node = Node(value, None)
        # 判断链表是否为空
        if self.isEmpty():
            self._head = new_node
        else:
            current_node = self._head
            while current_node.getNext() != None:
                current_node = current_node.getNext()
            current_node.setNext(new_node)


    def search(self, value):
        current_node = self._head
        isFound = False
        while current_node != None and not isFound:
            if current_node.getValue() == value:
                isFound = True
            else:
                current_node = current_node.getNext()
        return isFound

    def index(self, value):
        current_node = self._head
        count = 0
        isFound = False
        while current_node != None and not isFound:
            count += 1
            if current_node.getValue() == value:
                isFound = True
            else:
                current_node = current_node.getNext()
        if isFound:
            return count
        else:
            raise ValueError(" %s is not in linkedlist"  % str(value))

    def remove(self, value):
        current_node = self._head
        pre = None # 判断是否为头节点
        while current_node != None:
            if current_node.getValue() == value:
                if not pre:
                    self._length -= 1
                    self._head = current_node.getNext()
                else:
                    self._length -= 1
                    pre.setNext(current_node.getNext())
                break
            else:
                pre = current_node
                current_node = current_node.getNext()

    def insert(self, pos, value):
        if pos <= 1:
            self.add(value)
        elif pos > self._length:
            self._length += 1
            self.append(value)
        else:
            current_node = self._head
            temp = Node(value)
            count = 1
            pre = None
            while count < pos:
                count += 1
                pre = current_node
                current_node = current_node.getNext()
                self._length += 1
            pre.setNext(temp)
            temp.setNext(current_node)

    def out_data(self):
        current_node = self._head
        while current_node.getNext() != None:
            print(current_node.getValue())
            current_node = current_node.getNext()
        print(current_node.getValue())




if __name__ == '__main__':
    # head默认指向一个Node(None, Node)
    linked_list = LinkedList()
    print(linked_list.isEmpty())
    for i in range(10):
        linked_list.append(i)
    linked_list.out_data()
    index = linked_list.index(0)
```

